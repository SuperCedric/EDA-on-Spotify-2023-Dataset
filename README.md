# Exploratory Data Analysis on Spotify 2023 Dataset
This respository includes an Exploratory Data Anaylsis on the `Most Streamed Spotify Songs 2023` Dataset using Python.

## INTRODUCTION
This project presents an exploratory data analysis (EDA) of a Spotify dataset containing detailed information about various music tracks, artists, and playlists. The primary objective is to uncover patterns and trends related to musical characteristics, popularity metrics, temporal release trends, and platform preferences, which can provide insights to artists, labebls, and streaming services. To ensure a clear and logical flow of analysis, we have outlined guiding questions to address.

## DEPENDENCIES
1. Pandas: for data manipulation and cleaning.
```
import pandas as pd
```
2. Numpy: For numerical operations and calculations, especially for computing statistics
```
import numpy as np
```
3. Matplotlib: For data visualization, useful for creating charts and plots.
```
import matplotlib.pyplot as plt
```
4. Seaborn: For enhanced data visualizationn, building on top of matplotlib with more statistical plotting capabilities.
```
import seaborn as sns
```
5. Scipy: Specifically for its z-score calculation capabilities.
```
from scipy.stats import zscore
```
## DATASET CLEANING
Before addressing the guide questions, it's essential to clean the dataset to ensure data accuracy, consistency, and reliability of the analysis. Removing errors, duplicates, incomplete entries helps prevent skewed results and enhances the quality of insights. Standardizing formats and datatypes also allow for smoother comparisons and easier manipulation across analyses.

### Included in the `Spotify_Clean.ipynb` file are the Python codes used for data cleaning processes:
### Checking for null values
> First, we check if there are any null values in the dataset
```python
#check for any null values 
spotifyclean.isnull().any()
```
![image](https://github.com/user-attachments/assets/613f074e-e4cc-4ad5-86c1-d63a754177dc)
> Since there are null values, remove them from the dataset 
```python
#print the remaining rows after dropping the null values
spotifyclean = spotifyclean.dropna()
print(f"After dropping the rows with null values, we are now left with {spotifyclean.shape[0]} songs.")
```
![image](https://github.com/user-attachments/assets/2130b64b-8f04-4a62-be29-69396ed112ba)


## GUIDE QUESTIONS
### Overview of Dataset
- How many rows and columns does the dataset contain?
- What are the data types of each column? Are there any missing values?

### Basic Descriptive Statistics
- What are the mean, median, and standard deviation of the streams column?
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

### Top Performers
- Which track has the highest number of streams? Display the top 5 most streamed tracks.
- Who are the top 5 most frequent artists based on the number of tracks in the dataset?
 
### Temporal Trends
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

### Genre and Music Characteristics
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

### Platform Popularity
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

### Advanced Analysis
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

## Insights and Recommendations
- Since the track_name column contains special characters from various languages that `UTF-8` encoding cannot handle, I used `latin1` encoding to import the `spotify-2023.csv` file into the notebook.
- The dataset includes duplicate songs, missing values, and an unusual string entry in the streams column, which appears to be an error from data collection.
- The dataset contains columns with object data types that needed to be converted to integers to enable plotting and analysis.
- Analyzing the distribution of `released_year` reveals a trend where songs released closer to 2023 tend to have more streams, suggesting that newer songs generally attract more listeners.
- There is also a noticeable decline in the count of songs from 2023, likely due to the limited time newer tracks have had to accumulate streams.
- Additionally, the number of playlists on Spotify is significantly higher than on other platforms, probably because Spotify’s platform was the primary source, skewing the data in its favor.
- I realized that if certain keys consistently have high streams such as C#, it can indicate listener preference for songs in those musical keys. Major keys are often perceived as more "happy" or "uplifting," while Minor keys might be associated with "emotional" or "moody" songs.

## Changelog
### 1.0 (10/30/24)
- Initial commit of repository in Github
- Cleaned the spotify dataset by dropping null values, duplicates, and unwanted values in the `Spotify_Clean.ipynb` file
- Initial commit of the ipynb file to clean the dataset
- Initial commit of the draft of readme file

### 1.2 (10/31/24)
- Initial commit of the `SpotifyData.ipynb` file
- Additional changes to the readme file

### 1.3 (11/1/24 - 11/4/24)
- Adjustments and additions in the `Spotify_Clean.ipynb` file
- Adjustments and additions in the `SpotifyData.ipynb` file
- Additional changes to the readme file

### 1.4 (11/5/24)
- Finishing touches in the `Spotify_Clean.ipynb` file
- Finishing touches in the `SpotifyData.ipynb` file
- Additions to the readme file

## References
1. https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-202
3. https://www.youtube.com/watch?v=iaZQF8SLHJs&ab_channel=Ryan%26MattDataScience
4. https://www.linkedin.com/advice/0/how-can-you-use-pandas-detect-correct-outliers-skills-data-science-zl5oe#:~:text=Calculate%20the%20IQR%20by%20subtracting,to%20filter%20out%20the%20outliers.

## BUILT WITH
  - Jupyter Notebook

## LIBRARIES USED
  - PYTHON
    - Pandas
    - Matplotlib
    - Seaborn
    - Scipy

## AUTHOR
  - Cedric Kurt Taguba - [@SuperCedric](https://github.com/SuperCedric)
