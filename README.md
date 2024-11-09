<h1 align="center">𝄞⨾𓍢ִ໋ Python - Exploratory Data Analysis on Spotify 2023 Dataset ⨾𓍢ִ໋𝄞</h1>
<p align="center">

## | BACKGROUND ᯓ
### What is EDA?
The objective of exploratory data analysis (also known as EDA) is to become familiar with the data by exploring deeply to examine and interpret the information we have. EDA allows us to examine, summarize, and analyze the dataset using Python libraries, which helps us identify patterns, trends, and the relationships between various data points.

### Dataset
The dataset titled "Most Streamed Spotify Songs 2023," which is accessible on [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023), will be examined in this deliverable. Track names, artist names, release dates, Spotify playlists and charts, streaming data, the existence of Apple Music and Deezer, Shazam charts, and other audio characteristics are all included in this dataset.


## | OBJECTIVES ၊၊||၊
This deliverable aims to:
1. Understand the dataset structure, check for missing values, and examine the data types.
2. Create a summarized statistics of key metrics such as stream counts, release dates, and musical attributes (e.g., BPM, danceability).
3. Make use of visualization (e.g., bar charts, histograms, scatter plots) to showcase the trends and patterns.
4. Explore relationships between variables.
5. Provide insights and recommendations.


## | RESULTS AND DISCUSSION ୭ ˚.⁺⊹ .ᐟ
### <ins>A. Libraries<ins>
Before we dive into coding, we first need to import the necessary libraries that will assist in manipulating the data.
``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
1. **Numpy** - Importing `numpy` for numerical computations and array manipulation
2. **Pandas** - Importing `pandas` for data structure manipulations and operations
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


### <ins>E. Top Performers<ins>
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


### <ins>F. Temporal Trends<ins>
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
The graph reveals that January has the highest number of track releases, with May following closely behind. The concentration of releases in January could be a deliberate strategy by artists and labels, as songs launched early in the year have a greater opportunity to gather listeners over time. This extended period of exposure means these tracks can maintain or even increase their popularity as the year progresses, helping them stay relevant throughout the months.


### <ins>G. Genre and Music Characteristics<ins>
#### Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
``` python
# Convert columns to numeric
col = ['streams', 'bpm', 'danceability_%', 'valence_%', 'energy_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']
for col in col:
    spotify_ds[col] = pd.to_numeric(spotify_ds[col], errors='coerce')
    
# Correlation between streams and musical attributes
corr = spotify_ds[['streams', 'bpm', 'danceability_%', 'valence_%', 'energy_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']].corr()

# Heat map
plt.figure(figsize=(10, 4))
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0)
plt.title('Correlation Between Streams and Musical Attributes')
plt.xticks(rotation=45)
plt.show()
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/9e2411c6-e7f3-4051-a529-0ac9ff265e26)


#### Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?
``` python
dnc_enr = correlation.loc['danceability_%', 'energy_%']
val_acou = correlation.loc['valence_%', 'acousticness_%']

print(f"Correlation Between Danceability and Energy: {dnc_enr:.2f}")
print(f"Correlation Between Valence and Acousticness: {val_acou:.2f}")
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/611b44b7-a3df-4316-8fa5-9ae08161d81b)
> _**Analysis**_ <br />
The heatmap analysis of the dataset reveals that there is no strong relationship between the number of streams and other musical attributes. This suggests that no single musical characteristic can fully explain a song’s streaming popularity. Popular songs appear to depend on a combination of factors, rather than being influenced by one specific attribute. However, a few relationships do stand out. For instance, "danceability" and "valence" show a moderate positive correlation, meaning that songs that are more danceable tend to have a more positive mood or tone. Similarly, "energy" and "valence" also show a moderate positive correlation, suggesting that songs with higher energy levels often have a more upbeat or positive feel. On the other hand, "energy" and "acousticness" show the weakest correlation among the attributes, with a moderate negative relationship. This indicates that songs with higher acousticness tend to have less energy, and vice versa. Further, danceability and energy reveals to have a weak positive correlation, suggesting that more danceable songs also tend to have higher energy levels, creating an intense and active vibe. Meanwhile, "valence" and "acousticness" have a weak negative correlation, showing that more positive songs tend to have lower acousticness, though this relationship is not strong enough to be considered a rule. Lastly, the relationship between "energy" and "acousticness" has a moderate negative correlation, with a value of -0.08, emphasizing the contrast between songs with high energy and those with a more acoustic or mellow sound. These findings highlight that, while individual musical traits play a role in a song's popularity, no single factor dominates.


### <ins>H. Platform Popularity<ins>
#### How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?
``` python
spotify_playlists = spotify_ds['in_spotify_playlists'].mean()
spotify_charts = spotify_ds['in_spotify_charts'].mean()
apple_playlists = spotify_ds['in_apple_playlists'].mean()

platforms = [spotify_playlists, spotify_charts, apple_playlists]
labels = ['Spotify Playlists', 'Spotify Charts', 'Apple Playlists']

plt.figure(figsize=(6, 4))
plt.bar(labels, platforms)
plt.ylabel('Average Number')
plt.title('Tracks in Spotify Playlist and Charts and Apple Playlists')
plt.show()
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/2b4aca78-0431-48ca-b443-827b9ecb137f)
> _**Analysis**_ <br />
This bar graph shows a big difference in the average number of tracks between Spotify Playlists, Spotify Charts, and Apple Playlists. Spotify Playlists have a much higher number of tracks compared to the other two. In fact, Spotify Charts and Apple Playlists have so few tracks on average that their bars are barely visible next to the Spotify Playlists bar. This means that Spotify Playlists usually offer a much larger variety of songs, while Spotify Charts and Apple Playlists have a fewer selection.


### <ins>I. Advanced Analysis<ins>
#### Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
``` python
ave_streams = spotify_ds.groupby('key')['streams'].mean().sort_values(ascending=False).reset_index()
ave_streams.columns = ['Key', 'Average Streams']
print("Average Streams by Key:")
display(ave_streams)
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/e5c29211-f233-45ec-8319-df2e489a3bd5)
> _**Analysis**_ <br />
The key with the highest average streams by key is C# with approximately 604 million, followed closely by E which is about 577 million and D# which is about 553 million. The keys with the lowest average streams are G (about 452 million) and A (about 409 million). This suggests that songs in certain keys, like C# and E, tend to perform better in terms of streams, while keys like G and A may be less popular among listeners.

#### Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.
``` python
pltfrm = ['in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists', 'in_apple_charts', 'in_deezer_playlists', 'in_deezer_charts']
artists = spotify_ds.groupby("artist(s)_name")[pltfrm].sum().sum(axis=1).sort_values(ascending=False)

top_15 = artists.head(15).reset_index()
top_15.columns = ['Artist', 'Number of Appearances']

plt.figure(figsize = (12, 6))
sns.barplot(data = top_15, x = 'Number of Appearances', y = 'Artist', dodge = False, legend = False)
plt.title('Top 15 Most Frequently Appearing Artists', fontsize = 20)
plt.xlabel('Number of Appearances', fontsize = 12)
plt.ylabel('Artist', fontsize = 12)
plt.grid(axis = 'x', linestyle = '--', alpha = 0)
plt.show()
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/0ce82629-5bd0-499f-927c-30c0ab0a78c2)
> _**Analysis**_ <br />
The Weeknd leads in appearances, with Ed Sheeran and Taylor Swift following closely behind. This suggests that users often add The Weeknd's music to their playlists, showing that his songs are popular and frequently chosen by listeners.

## | AUTHOR ✎ ⋆⑅˚₊
Angeline Jeannah E. Arao
- [GitHub](https://github.com/araogabi)
- [Facebook](https://www.facebook.com/araoangeline?mibextid=LQQJ4d)
