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
### A. Libraries
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


### B. Accessing the `.csv` file
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


### C. Overview of Dataset
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



### D. Basic Descriptive Statistics
#### What are the mean, median, and standard deviation of the streams column?
> 

``` python
# Convert 'streams' to numeric
spotify_ds['streams'] = pd.to_numeric(spotify_ds['streams'],errors='coerce')

# Remove non-numeric rows
spotify_ds = spotify_ds.dropna(subset=['streams'])

# Calculate mean, median, and standard deviation
mean = spotify_ds['streams'].mean()
median = spotify_ds['streams'].median()
std = spotify_ds['streams'].std()

# Display the output
print('Mean: ' ,mean)
print('Median: ' ,median)
print('Standard Deviation: ' ,std)
```

${\textsf{\color{blue}Output:}}$
![image](https://github.com/user-attachments/assets/d42a69f8-2d31-45e5-8de8-8376616e4f17)



