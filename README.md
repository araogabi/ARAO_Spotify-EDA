<h1 align="center">Python - Exploratory Data Analysis on Spotify 2023 Dataset</h1>
<p align="center">

## I. BACKGROUND
### What is EDA?
The objective of exploratory data analysis (also known as EDA) is to become familiar with the data by exploring deeply to examine and interpret the information we have. EDA allows us to examine, summarize, and analyze the dataset using Python libraries, which helps us identify patterns, trends, and the relationships between various data points.

### Dataset
The dataset titled "Most Streamed Spotify Songs 2023," which is accessible on [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023), will be examined in this deliverable. Track names, artist names, release dates, Spotify playlists and charts, streaming data, the existence of Apple Music and Deezer, Shazam charts, and other audio characteristics are all included in this dataset.


## II. OBJECTIVES
This deliverable aims to:
1. Understand the dataset structure, check for missing values, and examine the data types.
2. Create a summarized statistics of key metrics such as stream counts, release dates, and musical attributes (e.g., BPM, danceability).
3. Make use of visualization (e.g., bar charts, histograms, scatter plots) to showcase the trends and patterns.
4. Explore relationships between variables.
5. Provide insights and recommendations.


## III. RESULTS AND DISCUSSION
### <ins>A. Libraries<ins>
Before we dive into coding, we first need to import the necessary libraries that will assist in manipulating the data.
``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
1. **Import Numpy** - Importing `numpy` for numerical computations and array manipulation
2. **Import Pandas** - Importing `pandas` for data structure manipulations and operations
3. **Matplotlib** - Importing `matplotlib.pyplot` for visualization
4. **Seaborn** - Importing `seaborn` for creating statistical plots, built on top of Matplotlib


### <ins>B. Accessing the `.csv` file<ins>
``` python
spotify_ds = pd.read_csv('spotify-2023.csv')
spotify_ds
```

$${\textsf{\color{red}However, the character encoding of some .csv files doesn't match Jupyter Notebook's standard encoding.}}$$
$${\textsf{\color{red}We can convert the .csv file to .xlsx file by exporting it to Excel Workbook.}}$$

``` python
spotify_ds = pd.read_excel('spotify-2023.xlsx')
spotify_ds
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/9cb86f34-63f7-4f3d-bb8c-13bd47bf4619)

To adjust the display settings and show the complete DataFrame, use the following commands:
- pd.set_option('display.max_rows', None)
- pd.set_option('display.max_columns', None)

``` python
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
spotify_ds = pd.read_excel('spotify-2023.xlsx')
spotify_ds
```


### <ins>C. Overview of Dataset<ins>
#### How many rows and columns does the dataset contain?
> We could use the `.shape` command to get the dimensions of the dataset or data structure.
``` python
spotify_ds = pd.read_excel('spotify-2023.xlsx')
spotify_ds
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/f8324077-e167-417a-837d-acc9511bbcdc)


#### What are the data types of each column? Are there any missing values?
> To get the summary of the content of the dataset we could use the command `.info()`. Shown in the output that it gives us the summary of the data types on each column. <br /><br />
The command `.isnull()` is used to detect any missing values, it returns a DataFrame with Boolean values (e.g., If it is True it means there is a missing value, which is often represented as NaN or None). <br /><br />
We use `.sum()` command to calculate the sum of the numerical values. <br /><br />
Using it as `.isnull().sum()` sums up the True values in each column of the dataframe. **Remember: True = 1 and False = 0.**

``` python
print('Data Types:' ,spotify_ds.info())
print('\n\nMissing Values:\n',spotify_ds.isnull().sum())
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/49620804-a07e-482f-b393-a05879b6fb4f)
![image](https://github.com/user-attachments/assets/7febdc18-0d7d-4a9c-9d37-33aafea25d24)



### <ins>D. Basic Descriptive Statistics<ins>
#### What are the mean, median, and standard deviation of the streams column?
> The `pd.to_numeric()` function converts each values in 'streams' to a numeric value. However, `NaN` is displayed if the value cannot be converted into a numerical value because of the `errors='coerce'` argument. <br /><br />
When a column displays `NaN` value the line `spotify_ds = spotify_ds.dropna(subset=['streams'])` removes the rows where it is showned to ensure that only rows with numerical value is retained.

``` python
# Convert 'streams' to numeric
spotify_ds['streams'] = pd.to_numeric(spotify_ds['streams'],errors='coerce')

# Remove non-numeric rows
spotify_ds = spotify_ds.dropna(subset=['streams'])

# Calculate mean, median, and standard deviation
mean = spotify_ds['streams'].mean() # .mean() is used to calculate the mean
median = spotify_ds['streams'].median() # .median() is used to calculate the median
std = spotify_ds['streams'].std() # .std() is used to calculate the standard deviation

# Display the output
print('Mean: ' ,mean)
print('Median: ' ,median)
print('Standard Deviation: ' ,std)
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/d42a69f8-2d31-45e5-8de8-8376616e4f17)

> _**Analysis**_ <br />
The average number of streams for the top songs was about 514 million, but the median was lower at 291 million, meaning while some songs were hugely popular, many others had more modest streaming numbers.


#### What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

``` python
# Convert 'artist_count' and 'released_year' columns to numeric
spotify_ds['artist_count'] = pd.to_numeric(spotify_ds['artist_count'], errors='coerce')
spotify_ds['released_year'] = pd.to_numeric(spotify_ds['released_year'], errors='coerce')

# Remove non-numeric rows
spotify_ds = spotify_ds.dropna(subset=['released_year', 'artist_count'])

# Create a figure with two subplots side by side
fig, ax = plt.subplots(1, 2, figsize=(10, 4))

# Plot
sns.histplot(spotify_ds['released_year'], bins=20, kde=True, ax=ax[0])
ax[0].set_title("Distribution of Released Year")
ax[0].set_xlabel("Release Year")
axes[0].set_ylabel("Frequency")

# Plot
sns.histplot(spotify_ds['artist_count'], bins=10, kde=True, ax=ax[1])
ax[1].set_title("Distribution of Artist Count")
ax[1].set_xlabel("Artist Count")
ax[1].set_ylabel("Frequency")
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/fd7f7901-67c8-45c1-afed-1f332744261c)
> _**Analysis**_ <br />
The histogram of _'Release Years'_ reveals that the majority of song were released in 2020, indicating a strong presence of recent music. Additionally, the histogram of _'Artist Count'_ shows that most of these songs feature a single artist. Tracks with more artists are less common, suggesting that collaborations are rarer than solo or duet.


### <ins>Top Performers<ins>
#### Which track has the highest number of streams? Display the top 5 most streamed tracks.

``` python
# Sort from highest to lowest
sort = spotify_ds.sort_values(by='streams', ascending=False)

# Display the top 5 most streamed tracks
top_5 = sort.head()
top_5.loc[:,['track_name','artist(s)_name','streams']]
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/2b2275c5-ac00-492e-ab01-acef884ae679)

> _**Analysis**_ <br />
The data shows that _"Blinding Lights"_ by _The Weeknd_ is the most-streamed track of 2023, with 3.7 billion streams.


#### Who are the top 5 most frequent artists based on the number of tracks in the dataset?
``` python
# Split to convert individual artist names into a list
artist = spotify_ds['artist(s)_name'].str.split(', ')

# Breaks down lists of artists into individual rows, counts the frequency of each artist, and then displays the top 5 most common artists
data = artist.explode().value_counts().head()

# Convert to dataframe
top5_artists = exploded_data.reset_index()
top5_artists.columns = ['Artist', 'Track Count']
top5_artists
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/dae9db38-af09-4e43-b63f-862aa2905ce5)

> _**Analysis**_ <br />
The analysis reveals that the top five most frequent artists in the dataset are led by Bad Bunny, with 40 tracks featured, followed by Taylor Swift with 38 tracks, The Weeknd with 37 tracks, and SZA and Kendrick Lamar, who both have 23 tracks featured.


### <ins>Temporal Trends<ins>
#### Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
``` python
plt.figure(figsize=(11, 4))
spotify_ds['released_year'].value_counts().sort_index().plot(kind='bar')
plt.title('Number of Tracks Released Per Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.show()
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/bf38b013-4180-437e-be6b-4de2a145bd2c)

> _**Analysis**_ <br />
The graph shows that songs released in 2022 have the most tracks, followed by songs from 2023 and so on. This suggests that songs from the previous year have had more time to accumulate streams, while newer releases are still catching up.


#### Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
``` python
plt.figure(figsize=(11, 4))
spotify_ds['released_month'].value_counts().sort_index().plot(kind='bar')
plt.title('Number of Tracks Released Per Month')
plt.xlabel('Month')
plt.ylabel('Number of Tracks')
plt.show()
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/d92906e2-41e9-43e9-b822-a8589ff8194d)

> _**Analysis**_ <br />

