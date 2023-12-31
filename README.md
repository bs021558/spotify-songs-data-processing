# Spotify Tracks Analysis

![dashboard](https://github.com/bs021558/spotify-songs-data-processing/assets/36155664/019da1a4-1c54-4818-b7b2-c78980df3b5e)

### Introduction

A project that data cleasing and ELT with Redshift and visualize with superset. For the dataset “spotify_songs.csv” was used.

### Dataset

[https://www.kaggle.com/datasets/joebeachcapital/30000-spotify-songs](https://www.kaggle.com/datasets/joebeachcapital/30000-spotify-songs)

### Architecture

![architecture](https://github.com/bs021558/spotify-songs-data-processing/assets/36155664/4173a446-1a85-4e67-82e7-4a3c23217f25)

### Data Cleansing Strategies

1. Drop rows containing nulls
    
    ```python
    df.dropna(inplace=True)
    ```
    
2. Format the column containing timestamps to parse to Redshift as “date”
    
    In the dataset, “track_album_release_date” column contains not only “YYYY-MM-DD” but also “YYYY-MM” and “YYYY”. So fill the incomplete part with "-01-01" or "-01” that it can be parsed by Redshift correctly.
    
    ```python
    df['track_album_release_date'] = pd.to_datetime(df['track_album_release_date'], errors='coerce')
    ```
    
3. Remove commas in data
    
    “spotify_songs.csv” has values containing commas(,) in itselves. However, when Redshift handles it as “external table” in S3 bucket, I could not find the way to pass the commas in double quotation using delimiter. I’ve tried to use openCSVSerde but another problem had happened with “\”. So I decided to simply remove them.
    
    ```python
    df = df.applymap(lambda x: str(x).replace(',', ''))
    ```
