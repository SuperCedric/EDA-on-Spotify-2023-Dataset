# EDA-on-Spotify-2023-Dataset

## Insights
### Cleaning of Dataset
- I noticed that there are duplicated songs in the dataset, null values, and unwanted values such as the singular string in the `streams` column (Assuming it was an error in the gathering of data). 
- There is another datatype named 'object' which is basically a datatype that can store various types of data like int, string, float, etc. This is shown when there is more than 1 datatype in a column
### Answering the Guide Questions
- The distribution graph of the `released_year` column reveals a trend indicating that songs released closer to the dataset’s collection year tend to have higher stream counts on Spotify. This pattern suggests that songs generally gain popularity the more recent they are.
- Furthermore, there is a noticeable decline in the number of songs included from the year 2023, the same year the data was collected. This likely reflects a threshold at which the trend ceases, presumably because songs require a certain period to accumulate streams.

## Changelog
### 1.0 (10/30/24)
- Initial commit of repository in Github
- Cleaned the spotify dataset by dropping null values, duplicates, and unwanted values in the `Spotify_Clean.ipynb` file
- Initial commit of the ipynb file to clean the dataset
- Initial commit of the draft of readme file

### 1.1 (10/31/24)
- Initial commit of the `SpotifyData.ipynb` file
- Additional changes to the readme file

### 1.2 (11/1/24)
- Adjustments and additions in the `Spotify_Clean.ipynb` file
- Adjustments and additions in the `SpotifyData.ipynb` file
- Additional changes to the readme file

## References
1. https://www.youtube.com/watch?v=iaZQF8SLHJs&ab_channel=Ryan%26MattDataScience
2. https://www.linkedin.com/advice/0/how-can-you-use-pandas-detect-correct-outliers-skills-data-science-zl5oe#:~:text=Calculate%20the%20IQR%20by%20subtracting,to%20filter%20out%20the%20outliers.
