![image](https://github.com/user-attachments/assets/2f8985d7-3309-4936-96c7-22430ce4141d)# Exploratory Data Analysis on Spotify 2023 Dataset
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
### Checking for duplicates
> Check for any duplicated songs
```python
#checks for duplicate songs (rows that have the same song name and artist)
spotifyclean[spotifyclean.duplicated(subset=['track_name', 'artist(s)_name'], keep = False)].sort_values(by='track_name')
```
![image](https://github.com/user-attachments/assets/146ce637-33e5-4483-b328-b092672dc71d)
> Drop the duplicated rows and keep the one with the higher number of streams
```python
#print the resulting number of duplicated rows
print(f"The Spotify dataset contains 3 duplicates which must be removed.")
#drop the duplicated rows and keep the higher streamed song
spotifyclean = spotifyclean.drop_duplicates(subset=['track_name', 'artist(s)_name'], keep = 'last')
print(f"After dropping the duplicated rows, we are now working with {spotifyclean.shape[0]} songs.")
```
![image](https://github.com/user-attachments/assets/e2a68e76-9e4d-45f5-880d-3952b222f7dc)
### Check for unwanted values and change certain datatypes
> Check the rows with `object` datatypes
```python
#checks the datatypes of each column if each column has the correct datatype
spotifyclean.dtypes
```
> Change the rows with numerical values from string to int/float for data visualization
### streams column
```python
#since the column 'streams' has mixed datatypes, drop the values that aren't numbers then change their datatypes to float
spotifyclean['streams'] = pd.to_numeric(spotifyclean['streams'], errors = 'coerce')
#drop the index of the unwanted value but sort the values first so its easier to find the index
spotifyclean = (spotifyclean.sort_values(by='streams', ascending = False)).drop(index=574)
```
### in_deezer_playlists column
```python
#Since numerical values above 999 have commas in the string, we have to remove the comma before changing the datatypes to float
spotifyclean['in_deezer_playlists'] = spotifyclean['in_deezer_playlists'].str.replace(',','').astype(int)
```
### in_shazam_charts column
```python
#Since numerical values above 999 have commas in the string, we have to remove the comma before changing the datatypes to float
spotifyclean['in_shazam_charts'] = spotifyclean['in_shazam_charts'].str.replace(',','').astype(int)
```
### Sorting of rows
> Sort the rows by number of streams from highest to lowest
```python
#Sort the values by the number of streams in descending order
spotifyclean = spotifyclean.sort_values(by='streams', ascending = False).reset_index(drop=True)
spotifyclean
```
![image](https://github.com/user-attachments/assets/4e9cf617-3866-4276-849e-ba60f3d6de62)
### Saving of Cleaned Dataset
> Save the new dataset in a csv file named `Spotify-2023_Cleaned.csv`.
```python
#write the dataset in a different csv for later use
spotifyclean.to_csv('Spotify-2023_Cleaned.csv')
```
> For the full code used in this analysis, please see the [Spotify EDA Notebook](SpotifyData.ipynb)

## GUIDE QUESTIONS
### Importing of Datasets
> Firstly, import the raw version of the dataset
```python
#read the uncleaned dataset from the csv file for references
uncleaned = pd.read_csv('spotify-2023.csv', encoding = 'latin1')
uncleaned
```
![image](https://github.com/user-attachments/assets/082da17f-d58a-45e5-b6f6-71daf87ca08c)
> Next, import the cleaned version of the dataset
```python
#read the cleaned dataset from the csv file
spotifydf = pd.read_csv('spotify-2023_Cleaned.csv', index_col = 0)
spotifydf
```
![image](https://github.com/user-attachments/assets/4e9cf617-3866-4276-849e-ba60f3d6de62)
### Overview of Dataset
- How many rows and columns does the dataset contain?
```python
#take the number of rows and columns and print its values
uncleaned_rows = uncleaned.shape[0]
uncleaned_cols = uncleaned.shape[1]
spotifydf_rows = spotifydf.shape[0]
spotifydf_cols = spotifydf.shape[1]
print(f"The dataset initially contained {uncleaned_rows} rows and {uncleaned_cols} columns\n")
print(f"After cleaning the dataset, it now contains {spotifydf_rows} rows and {spotifydf_cols} columns")
```
![image](https://github.com/user-attachments/assets/f09b1dcf-d280-4f43-9caa-5435d6115a7b)
- What are the data types of each column? Are there any missing values?
```python
#use for loop to check the datatypes of each column in the dataset
print("The datatypes of each columns are:\n")
for column, dtype in spotifydf.dtypes.items():
    print(f"The column {column} has a datype of {dtype}")
```
![image](https://github.com/user-attachments/assets/f2634a6f-6cab-4d00-b1ba-84f6423aa635)
```python
#print and compare the differences of the quantity of null values in the cleaned and uncleaned dataset
print("In the uncleaned dataset, there were", uncleaned.isnull().sum().sum(), "missing values in the dataset\n")
print("After cleaning the dataset, it now contains", spotifydf.isnull().sum().sum(), "missing values")
```
![image](https://github.com/user-attachments/assets/2e6e8dc1-17a8-46e8-a91f-6aefb6b58e9b)
### Basic Descriptive Statistics
- What are the mean, median, and standard deviation of the streams column?
```python
#print the mean of number of streams
streams_mean = np.floor(spotifydf['streams'].mean())
#print the median of number of streams
streams_median = np.floor(spotifydf['streams'].median())
#print the standard deviation of number of streams
streams_std = np.floor(spotifydf['streams'].std())
#Tabulate the data into a DataFrame for readability
streams_statsdf = pd.DataFrame({'Stream Statistics':['Mean', 'Median', 'Standard Deviation'],'Value':[streams_mean, streams_median, streams_std]})
streams_statsdf
```
![image](https://github.com/user-attachments/assets/f9966382-49d3-4532-966d-c27c56730a93)
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
```python
#graphing of distribution plot of the number of songs as a function of their release date
sns.displot(spotifydf, x = 'released_year', aspect = 2, kde = True, discrete = True)
```
![image](https://github.com/user-attachments/assets/436bbce7-d724-401a-8b5e-c836caed24c1)
!
```python
#graphing of distribution plot of the number of songs as a function of their release date
sns.displot((spotifydf[spotifydf['released_year'] >= 2000]), x = 'released_year', aspect = 2, kde = True, discrete = True)
plt.title('Number of Songs in Every Year above 2000 Distribution Graph')
```
![image](https://github.com/user-attachments/assets/e5291f9e-1e26-4209-9f28-d87e71f702d2)
!
- Any noticeable trends?
```python
#to check the span of the lower and upper bound of the released years
print("The songs released span from", min(spotifydf['released_year']), "to", max(spotifydf['released_year']))
```
![image](https://github.com/user-attachments/assets/7008d00b-ed74-47f6-afbf-9a75bad47792)
!
- Any outliers?
```python
def outlier(df, column):
    # Finds the outliers in a certain column in a dataset using the interquartile range formula
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    lowerbound = Q1 - 1.5*IQR
    upperbound = Q3 + 1.5*IQR

    outliers = df[(df[column] < lowerbound) | (df[column] > upperbound)][column]
    return outliers

#print the number of outliers in the graph of released_year
print('In the distribution graph of "released_year", there are', outlier(spotifydf, 'released_year').shape[0] ,'outliers.')
```
![image](https://github.com/user-attachments/assets/662616aa-0c14-4251-8c48-ae9d75a622fd)
!
- What is the distribution of artist_count?
```python
#graphing of distribution plot of number of songs as a function of the number of artists included in the song
plt.figure(figsize=(12,6))
sns.displot(spotifydf, x = 'artist_count', aspect = 2, kde = True, discrete = True)
plt.ylim(0,500)
plt.title('Artist Count Distribution Graph')
plt.ylabel('Number of Songs')
```
![image](https://github.com/user-attachments/assets/eb095711-ef49-4257-96d9-7a63159d38b2)
!
- Any noticeable trends?
!
- Any outliers?
```python
#print the number of outliers using the user-defined outlier function
print('In the distribution graph of "artist_count", there are', outlier(spotifydf, 'artist_count').shape[0] ,'outliers.')
```
![image](https://github.com/user-attachments/assets/bbe8bb99-2887-4ae2-8971-ca969aabb16b)
!
### Top Performers
- Which track has the highest number of streams? Display the top 5 most streamed tracks.
```python
#display the head of the table, sorted in descending order in streams.
spotifydf.head()[['track_name','artist(s)_name','streams']]
```
```python
#display the top 5 most streamed songs in the dataset using a bar graph
plt.figure(figsize=(12,6))
sns.barplot(spotifydf.head(),x='track_name',y='streams', dodge = 'auto' )
plt.tick_params(axis = 'x',rotation = 12)
plt.title('Top 5 Most Streamed Songs')
```
!
- Who are the top 5 most frequent artists based on the number of tracks in the dataset?
```python
#split the strings in 'artist(s)_name' to separate all artists involved in a list
spotifydf['artist(s)_name'] = spotifydf['artist(s)_name'].str.split(',')
spotifydf
```

```python
#create a new dataframe to separate all the list of artists in a different column
artistsdf = (spotifydf.loc[:, ['artist(s)_name']]).explode('artist(s)_name')
#remove the unwanted spaces so it won't be counted as a unique string
artistsdf['artist(s)_name'] = artistsdf['artist(s)_name'].str.strip()
#count all unique values (artists) and make a dataframe out of it
artist_count = artistsdf['artist(s)_name'].value_counts(ascending = False).to_frame().reset_index()
#change the index to start from 1 instead of 0
artist_count.index = range(1, len(artist_count) + 1)
print("The Top 5 Most Frequent Artists Are:")
artist_count.head()
```
```python
#create a bar graph of the most frequent artists
plt.figure(figsize=(12,6))
sns.barplot(artist_count.head(), x = 'artist(s)_name', y = 'count')
plt.title('Top 5 Most Frequent Artists')
plt.ylabel('Number of Appearances')
```
### Temporal Trends
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
```python
#create a line graph of the number of tracks released per year
yearlytracks = spotifydf.groupby('released_year').size()
sns.lineplot(x = yearlytracks.index , y = yearlytracks.values)
plt.title('Number of Tracks Released per Year')
plt.ylabel('Number of Tracks')
```
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
```python
#create a line graph of the number of tracks released per month
monthlytracks = spotifydf.groupby('released_month').size()
plt.figure(figsize=(12,6))
sns.lineplot(x = monthlytracks.index, y = monthlytracks.values)
plt.title('Number of Tracks Released per Month across All Years')
plt.ylabel('Number of Tracks')
months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec']
plt.xticks(ticks = monthlytracks.index, labels = months)
```
```python
#for loop to create line plots the years 2015 to 2024
plt.figure(figsize=(12,6))
for i in range(2018, 2024):
    releasespermonth = spotifydf[spotifydf['released_year'] == i].groupby('released_month').size()
    sns.lineplot(releasespermonth, label = i)
plt.legend()
plt.title('Number of tracks Released per Month from 2018-2023')
plt.ylabel('Number of Tracks')
plt.xticks(ticks = monthlytracks.index, labels = months)
```
### Genre and Music Characteristics
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
```python
#create a heatmap of the correlation of streams to the 6 other variables
plt.figure(figsize=(12,6))
sns.heatmap(spotifydf[['streams','bpm','danceability_%','valence_%','energy_%','acousticness_%','instrumentalness_%','liveness_%','speechiness_%']].corr(), annot=True, cmap = 'RdBu')
plt.title('Correlation of Streams and Music Characteristics')
```
- Is there a correlation between danceability_% and energy_%?
```python
#create a heatmap of the correlation of streams to the 6 other variables
plt.figure(figsize=(12,6))
sns.heatmap(spotifydf[['danceability_%','valence_%','energy_%','acousticness_%']].corr(), annot=True, cmap = 'RdBu')
plt.title('Correlation of Different Music Characteristics')
```
- How about valence_% and acousticness_%?

### Platform Popularity
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?
```python
#Create a new dataframe which stores the sum of all number of playlists of each platform
platformplaylists = pd.DataFrame({
    'Platform': ['Spotify', 'Apple', 'Deezer'],
    'Playlists': [spotifydf['in_spotify_playlists'].sum(), spotifydf['in_apple_charts'].sum(), spotifydf['in_deezer_playlists'].sum()]
}).sort_values('Playlists', ascending = False)

#print the dataframe for checking
print('The Number of Playlists in each Platform is as follows:')
print(platformplaylists)

#create a horizontal bar graph for visualization
plt.figure(figsize=(12,6))
sns.barplot(platformplaylists,x = 'Playlists', y = 'Platform', orient='h')
plt.title('Number of Playlists in each Platform')
```
### Advanced Analysis
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
#### Keys
```python
#create a new dataframe of the total number of streams of each key
streamsofkeys = spotifydf[['streams','key']].groupby('key').sum('streams').reset_index().sort_values('streams',ascending = False)
#create the bar graph for visualization
plt.figure(figsize=(12,6))
sns.barplot(streamsofkeys, x = 'key', y = 'streams', hue = 'key', palette = 'muted')
plt.title('Number of Streams per Key')
```
```python
#import necessary library to use z-score function
from scipy.stats import zscore

# Group the key values and sum its streams
streamsofkeys = spotifydf[['streams', 'key']].groupby('key').sum('streams').reset_index()

# calculate the z-score of each 
streamsofkeys['zscore'] = zscore(streamsofkeys['streams'])

# sort the dataframe by the zscore in descending order
streamsofkeys = streamsofkeys.sort_values('zscore', ascending=False)

# create a bar graph using the created dataframe
plt.figure(figsize=(12, 6))
sns.barplot(data=streamsofkeys, x = 'key', y = 'zscore', hue = 'key', palette='muted')
plt.title('Z-Score Normalized Streams per Key')
plt.xlabel('Key')
plt.ylabel('Z-Score of Streams')
```
#### Modes
```python
streamsofmodes = spotifydf[['streams','mode']].groupby('mode').sum('streams').reset_index().sort_values('streams', ascending = False)
streamsofmodes
sns.barplot(spotifydf, x = 'mode', y = 'streams')
plt.title('Number of Streams of Major vs Minor')
```
```python
plt.figure(figsize=(12,6))
sns.violinplot(spotifydf,x = 'mode', y = 'streams')
plt.title('Number of Streams of Major vs Minor')
```
- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.
```python
#create a new column with the sum of the playlists in all platforms
spotifydf['total_playlists'] = spotifydf['in_spotify_playlists'] + spotifydf['in_apple_playlists'] + spotifydf['in_deezer_playlists']

#separate the artists of each song to their individual rows
artistsplaylists = spotifydf[['artist(s)_name','total_playlists']].explode('artist(s)_name')

#remove the whitespaces in each string of the artist name so unique values wont be counted
artistsplaylists['artist(s)_name'] = artistsplaylists['artist(s)_name'].str.strip()

#group the dataframe by the artist name and take the sum of their number of playlists
artistsplaylists = artistsplaylists.groupby('artist(s)_name', as_index=False)['total_playlists'].sum()

#sort by total_playlist and reset its index so it counts from 0
artistsplaylists = artistsplaylists.sort_values('total_playlists', ascending = False).reset_index(drop=True)

#change the index of the dataframe to start from 1 instead of 0
artistsplaylists.index = range(1, len(artistsplaylists) + 1)

#display the top 5 most frequently appearing artist in playlists
artistsplaylists.head()
```
```python
#create the barplot to visualize the differences in their frequency
plt.figure(figsize=(12,6))
sns.barplot(artistsplaylists.head(),x = 'artist(s)_name', y = 'total_playlists')
plt.title('Frequency of Artists in Playlists')
```
```python
#create a new column with the sum of the charts in all platforms
spotifydf['total_charts'] = spotifydf['in_spotify_charts'] + spotifydf['in_apple_charts'] + spotifydf['in_deezer_charts'] + spotifydf['in_shazam_charts']

#separate the artists of each song to their individual rows
artistscharts = spotifydf[['artist(s)_name','total_charts']].explode('artist(s)_name')

#remove the whitespaces in each string of the artist name so unique values wont be counted
artistscharts['artist(s)_name'] = artistscharts['artist(s)_name'].str.strip()

#group the dataframe by the artist name and take the sum of their number of playlists
artistscharts = artistscharts.groupby('artist(s)_name', as_index=False)['total_charts'].sum()

#sort by total_playlist and reset its index so it counts from 0
artistscharts = artistscharts.sort_values('total_charts', ascending = False).reset_index(drop=True)

#change the index of the dataframe to start from 1 instead of 0
artistscharts.index = range(1, len(artistscharts) + 1)

#display the top 5 most frequently appearing artist in charts
artistscharts.head()
```
```python
#create the barplot to visualize the differences in their frequency
plt.figure(figsize=(12,6))
sns.barplot(artistscharts.head(),x = 'artist(s)_name', y = 'total_charts')
plt.title('Frequency of Artists in Charts')
```
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
