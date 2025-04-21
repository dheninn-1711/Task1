# Anime Dataset Cleaning and Analysis

## Overview
</div>

This task involves cleaning and preprocessing an anime dataset to prepare it for analysis. The dataset contains various attributes related to anime titles, including scores, rankings, episodes, genres, and more. The goal is to ensure data integrity and usability for further analysis or visualization.

## Dataset

The dataset is in CSV format and contains the following columns:

- **anime_id**: Unique identifier for each anime.
- **title**: Title of the anime.
- **ranked**: Ranking of the anime.
- **score**: Score given to the anime.
- **episodes**: Number of episodes in the anime.
- **aired**: Release date of the anime.
- **genre**: Genre(s) of the anime.
- **synopsis**: Brief description of the anime.

## Requirements

- Python 3.x
- Pandas library
```bash
pip install pandas
```
## Code Explanation
The following steps were taken to clean and preprocess the dataset:

1. **Import Libraries**: Import the necessary libraries for data manipulation.
   ```python
   import pandas as pd
   ```
2. **Load Dataset**: Load the dataset from a CSV file into a Pandas DataFrame.
```python
anime_df = pd.read_csv("C:/Users/dheni/Downloads/anime.csv")
```
3. **Initial Dataset Info**: Display initial information about the dataset, including data types and non-null counts.
```python
print("Initial Dataset Info:")
anime_df.info()
```
4. **Drop Missing Values**: Remove rows with missing values in the 'ranked' and 'score' columns.
```python
anime_df.dropna(subset=['ranked', 'score'], inplace=True)
```
5. **Fill Missing Values**: Fill missing values in the 'episodes' column with 0 and in the 'synopsis' column with a default message.
```python
anime_df['episodes'].fillna(0, inplace=True)
anime_df['synopsis'].fillna('No description available', inplace=True)
```
6. **Remove Duplicates**: Remove duplicate rows based on 'title' and 'uid' columns.
```python
anime_df.drop_duplicates(subset=['title', 'uid'], inplace=True)
```
7. **Convert Data Types**: Convert the 'score', 'ranked', and 'episodes' columns to numeric types, coercing errors to NaN and filling NaN values appropriately.
```python
anime_df['score'] = pd.to_numeric(anime_df['score'], errors='coerce')
anime_df['ranked'] = pd.to_numeric(anime_df['ranked'], errors='coerce').fillna(0).astype(int)
anime_df['episodes'] = pd.to_numeric(anime_df['episodes'], errors='coerce').fillna(0).astype(int)
```
8. **Round Scores**: Round the 'score' values to the nearest 0.5.
```python
anime_df['score'] = (anime_df['score'] * 2).round() / 2
```
9. **Process Aired Dates**: Extract the start date from the 'aired' column, convert it to datetime format, and format it as a string.
```python
anime_df['aired'] = anime_df['aired'].str.split(' to ').str[0]
anime_df['aired'] = pd.to_datetime(anime_df['aired'], format='%b %d, %Y', errors='coerce')
anime_df['aired'] = anime_df['aired'].dt.strftime('%d-%m-%Y')
```
10. **Handle Missing Aired Dates**: Interpolate and forward fill any missing values in the 'aired' column.
```python
anime_df['aired'] = anime_df['aired'].interpolate(method='time')
anime_df['aired'] = anime_df['aired'].fillna(method='ffill')
```
11. **Clean Genre Column**: Clean the 'genre' column by stripping brackets, replacing quotes, and splitting into lists.
```python
anime_df['genre'] = anime_df['genre'].str.strip("[]").str.replace("'", "").str.split(', ').apply(lambda x: [g.lower().replace(' ', '_') for g in x])
```
12. **Truncate Synopsis**: Truncate the 'synopsis' column to the first 200 characters and append '...' to indicate truncation.
```python
anime_df['synopsis'] = anime_df['synopsis'].str.slice(0, 200) + '...'
```
13. **Rename Columns**: Rename columns for clarity.
```python
anime_df.rename(columns={'uid': 'anime_id', 'aired': 'released_date'}, inplace=True)
```
14. **Convert Column Names**: Convert all column names to uppercase for consistency.
```python
anime_df.columns = anime_df.columns.str.upper()
```
15. **Final Dataset Info**: Print cleaned dataset information, including data types and non-null counts.
```python
print("Cleaned Dataset Info:")
anime_df.info()
```
## Usage
You can run the script to clean the dataset and view the cleaned DataFrame. Make sure to update the file path in the pd.read_csv() function to point to your local CSV file.

```python
anime_df = pd.read_csv("C:/Users/dheni/Downloads/anime.csv")
```
## Conclusion
This task demonstrates the process of cleaning and preprocessing a dataset for analysis. The cleaned dataset can now be used for further analysis, visualization, or machine learning tasks.
