
 # NBA Salary Prediction Project
 
 The NBA is (almost) always fun to watch. I've been a basketball fan ever since 2012 when Linsanity was a big deal, and to this day I follow the NBA and all of its drama, even when recently things have been pretty boring because of the Warriors. (As of 6/5/19, I really hope the Raptors destroy them!) One matter that I was always curious about is the matter of salary. There are some superstars who get paid a crazy amount, even when some of them don't really earn it in terms of performance. On the other end of the spectrum some people make peanuts yet absolutely destroy expectations. What are the factors that truly determine how much an NBA player is paid? 

In this project we will find out.

## Importing Data
For my data, I've found a large number of sets that I feel will provide me with good analysis fodder, some data that will make it easy to determine the best model to estimate what parameters matter the most in calculating an NBA player's salary. 



For the sake of my data exploration and analysis, I need to import the library pandas.

Here I will use pandas to read all of the csv files.

I'm reading these files directly from my GitHub repository, which is why I have the variable 'urls'.


```
import pandas as pd

urls = ['https://raw.githubusercontent.com/jerrytigerxu/NBA-Salary-Prediction/master/data/Seasons_Stats.csv', 'https://raw.githubusercontent.com/jerrytigerxu/NBA-Salary-Prediction/master/data/1990_to_2018.csv', 'https://raw.githubusercontent.com/jerrytigerxu/NBA-Salary-Prediction/master/data/NBA%20Season%20Data.csv']

data = {} # I'm creating a dictionary where all of my data can be stored in one place

for f in urls:
  d = pd.read_csv(f)
  data[f.replace('.csv', '')[81:].replace('%20', '_')] = d
  
```


```
data.keys()
```




    dict_keys(['Seasons_Stats', '1990_to_2018', 'NBA_Season_Data'])



Now let's take a look at the data we have so far.


```
for file in data:
  print(file)
  data[file].info()
  print('\n')
```

    Seasons_Stats
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 24691 entries, 0 to 24690
    Data columns (total 53 columns):
    Unnamed: 0    24691 non-null int64
    Year          24624 non-null float64
    Player        24624 non-null object
    Pos           24624 non-null object
    Age           24616 non-null float64
    Tm            24624 non-null object
    G             24624 non-null float64
    GS            18233 non-null float64
    MP            24138 non-null float64
    PER           24101 non-null float64
    TS%           24538 non-null float64
    3PAr          18839 non-null float64
    FTr           24525 non-null float64
    ORB%          20792 non-null float64
    DRB%          20792 non-null float64
    TRB%          21571 non-null float64
    AST%          22555 non-null float64
    STL%          20792 non-null float64
    BLK%          20792 non-null float64
    TOV%          19582 non-null float64
    USG%          19640 non-null float64
    blanl         0 non-null float64
    OWS           24585 non-null float64
    DWS           24585 non-null float64
    WS            24585 non-null float64
    WS/48         24101 non-null float64
    blank2        0 non-null float64
    OBPM          20797 non-null float64
    DBPM          20797 non-null float64
    BPM           20797 non-null float64
    VORP          20797 non-null float64
    FG            24624 non-null float64
    FGA           24624 non-null float64
    FG%           24525 non-null float64
    3P            18927 non-null float64
    3PA           18927 non-null float64
    3P%           15416 non-null float64
    2P            24624 non-null float64
    2PA           24624 non-null float64
    2P%           24496 non-null float64
    eFG%          24525 non-null float64
    FT            24624 non-null float64
    FTA           24624 non-null float64
    FT%           23766 non-null float64
    ORB           20797 non-null float64
    DRB           20797 non-null float64
    TRB           24312 non-null float64
    AST           24624 non-null float64
    STL           20797 non-null float64
    BLK           20797 non-null float64
    TOV           19645 non-null float64
    PF            24624 non-null float64
    PTS           24624 non-null float64
    dtypes: float64(49), int64(1), object(3)
    memory usage: 10.0+ MB
    
    
    1990_to_2018
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 11838 entries, 0 to 11837
    Data columns (total 6 columns):
    player          11838 non-null object
    salary          11838 non-null int64
    season_end      11838 non-null int64
    season_start    11838 non-null int64
    team            11838 non-null object
    team_name       11838 non-null object
    dtypes: int64(3), object(3)
    memory usage: 555.0+ KB
    
    
    NBA_Season_Data
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 17729 entries, 0 to 17728
    Columns: 109 entries, Year to Rounded Age
    dtypes: float64(94), int64(7), object(8)
    memory usage: 14.7+ MB
    
    


## Data Cleaning

Currently we have three dataframes to deal with, so I'll start with the 'Seasons_Stats' dataframe.


```
# Have to remove the first column of the dataframe

data['Seasons_Stats'] = data['Seasons_Stats'].drop(data['Seasons_Stats'].columns[0], axis=1)


```


```
data['Seasons_Stats'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>blanl</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>blank2</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1950.0</td>
      <td>Curly Armstrong</td>
      <td>G-F</td>
      <td>31.0</td>
      <td>FTW</td>
      <td>63.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.368</td>
      <td>NaN</td>
      <td>0.467</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.1</td>
      <td>3.6</td>
      <td>3.5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>144.0</td>
      <td>516.0</td>
      <td>0.279</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>144.0</td>
      <td>516.0</td>
      <td>0.279</td>
      <td>0.279</td>
      <td>170.0</td>
      <td>241.0</td>
      <td>0.705</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>176.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>217.0</td>
      <td>458.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1950.0</td>
      <td>Cliff Barker</td>
      <td>SG</td>
      <td>29.0</td>
      <td>INO</td>
      <td>49.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.435</td>
      <td>NaN</td>
      <td>0.387</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.6</td>
      <td>0.6</td>
      <td>2.2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>102.0</td>
      <td>274.0</td>
      <td>0.372</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>102.0</td>
      <td>274.0</td>
      <td>0.372</td>
      <td>0.372</td>
      <td>75.0</td>
      <td>106.0</td>
      <td>0.708</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>109.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>99.0</td>
      <td>279.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1950.0</td>
      <td>Leo Barnhorst</td>
      <td>SF</td>
      <td>25.0</td>
      <td>CHS</td>
      <td>67.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.394</td>
      <td>NaN</td>
      <td>0.259</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.9</td>
      <td>2.8</td>
      <td>3.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>174.0</td>
      <td>499.0</td>
      <td>0.349</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>174.0</td>
      <td>499.0</td>
      <td>0.349</td>
      <td>0.349</td>
      <td>90.0</td>
      <td>129.0</td>
      <td>0.698</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>140.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>192.0</td>
      <td>438.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1950.0</td>
      <td>Ed Bartels</td>
      <td>F</td>
      <td>24.0</td>
      <td>TOT</td>
      <td>15.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.312</td>
      <td>NaN</td>
      <td>0.395</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.5</td>
      <td>-0.1</td>
      <td>-0.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.0</td>
      <td>86.0</td>
      <td>0.256</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.0</td>
      <td>86.0</td>
      <td>0.256</td>
      <td>0.256</td>
      <td>19.0</td>
      <td>34.0</td>
      <td>0.559</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1950.0</td>
      <td>Ed Bartels</td>
      <td>F</td>
      <td>24.0</td>
      <td>DNN</td>
      <td>13.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.308</td>
      <td>NaN</td>
      <td>0.378</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.5</td>
      <td>-0.1</td>
      <td>-0.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.0</td>
      <td>82.0</td>
      <td>0.256</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.0</td>
      <td>82.0</td>
      <td>0.256</td>
      <td>0.256</td>
      <td>17.0</td>
      <td>31.0</td>
      <td>0.548</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>27.0</td>
      <td>59.0</td>
    </tr>
  </tbody>
</table>
</div>



Taking a look at the head of the 'Seasons_Stats' data, I need to also deal with the data type of 'year' as well as figure out what to do with the lack of advanced data for the players from ye olden days.


```
data['Seasons_Stats']['Year'].head()
data['Seasons_Stats']['Year'].isnull().values.any()
#data['Seasons_Stats']['Year'] = data['Seasons_Stats']['Year'].astype('int64')
```




    True



Looks like there are some NaN values in the year column.


```
data['Seasons_Stats']['Year'].isnull().sum()

data['Seasons_Stats'][data['Seasons_Stats']['Year'].isnull() == True]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>blanl</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>blank2</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>312</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>487</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>618</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>779</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>911</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1021</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1128</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1236</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1348</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1577</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1682</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1808</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1942</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2078</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2211</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2347</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2481</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2659</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2866</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3068</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3314</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3580</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3850</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4096</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4373</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4648</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5006</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5381</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5726</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8680</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9107</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9546</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10006</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10448</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10907</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11357</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11839</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12292</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12838</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13413</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13961</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14469</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14966</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15504</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16005</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16489</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17075</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17661</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18225</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18742</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19338</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19921</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20500</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21126</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21678</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22252</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22864</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23516</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24095</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>67 rows × 52 columns</p>
</div>



I've found the rows where the 'year' column is NaN, and it appears that in those rows every single column is also NaN, which makes our lives a bit easier because we can just delete those directly.


```
data['Seasons_Stats'] = data['Seasons_Stats'].dropna(subset=['Year'])
```


```
data['Seasons_Stats']['Year'].isnull().sum()

# Nice! No more pesky NaN values in the 'year' column!
```




    0




```
data['Seasons_Stats']['Year'] = data['Seasons_Stats']['Year'].astype('int64')

# Now I can convert the years into integers
```


```
data['Seasons_Stats'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>blanl</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>blank2</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1950</td>
      <td>Curly Armstrong</td>
      <td>G-F</td>
      <td>31.0</td>
      <td>FTW</td>
      <td>63.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.368</td>
      <td>NaN</td>
      <td>0.467</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.1</td>
      <td>3.6</td>
      <td>3.5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>144.0</td>
      <td>516.0</td>
      <td>0.279</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>144.0</td>
      <td>516.0</td>
      <td>0.279</td>
      <td>0.279</td>
      <td>170.0</td>
      <td>241.0</td>
      <td>0.705</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>176.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>217.0</td>
      <td>458.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1950</td>
      <td>Cliff Barker</td>
      <td>SG</td>
      <td>29.0</td>
      <td>INO</td>
      <td>49.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.435</td>
      <td>NaN</td>
      <td>0.387</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.6</td>
      <td>0.6</td>
      <td>2.2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>102.0</td>
      <td>274.0</td>
      <td>0.372</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>102.0</td>
      <td>274.0</td>
      <td>0.372</td>
      <td>0.372</td>
      <td>75.0</td>
      <td>106.0</td>
      <td>0.708</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>109.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>99.0</td>
      <td>279.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1950</td>
      <td>Leo Barnhorst</td>
      <td>SF</td>
      <td>25.0</td>
      <td>CHS</td>
      <td>67.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.394</td>
      <td>NaN</td>
      <td>0.259</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.9</td>
      <td>2.8</td>
      <td>3.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>174.0</td>
      <td>499.0</td>
      <td>0.349</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>174.0</td>
      <td>499.0</td>
      <td>0.349</td>
      <td>0.349</td>
      <td>90.0</td>
      <td>129.0</td>
      <td>0.698</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>140.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>192.0</td>
      <td>438.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1950</td>
      <td>Ed Bartels</td>
      <td>F</td>
      <td>24.0</td>
      <td>TOT</td>
      <td>15.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.312</td>
      <td>NaN</td>
      <td>0.395</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.5</td>
      <td>-0.1</td>
      <td>-0.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.0</td>
      <td>86.0</td>
      <td>0.256</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.0</td>
      <td>86.0</td>
      <td>0.256</td>
      <td>0.256</td>
      <td>19.0</td>
      <td>34.0</td>
      <td>0.559</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1950</td>
      <td>Ed Bartels</td>
      <td>F</td>
      <td>24.0</td>
      <td>DNN</td>
      <td>13.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.308</td>
      <td>NaN</td>
      <td>0.378</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.5</td>
      <td>-0.1</td>
      <td>-0.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.0</td>
      <td>82.0</td>
      <td>0.256</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.0</td>
      <td>82.0</td>
      <td>0.256</td>
      <td>0.256</td>
      <td>17.0</td>
      <td>31.0</td>
      <td>0.548</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>27.0</td>
      <td>59.0</td>
    </tr>
  </tbody>
</table>
</div>



Because of the error that can come about through inflation and time-related issues, I need to cut down on the scope of this dataframe. First, I'll take the subset of player stats that is 1980 or later. Why? Because the 3-point line was first added between 1979 and 1980, thus, a lot of advanced stats would not have existed beforehand. 

Because I am cutting out nearly 30 years of NBA history, this NBA salary prediction algorithm would only make sense for 1980 and later. (Which is fine because that's when basketball actually started to be entertaining)


```
data['Seasons_Stats'] = data['Seasons_Stats'][data['Seasons_Stats']['Year'] > 1980]

data['Seasons_Stats'].head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>blanl</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>blank2</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6085</th>
      <td>1981</td>
      <td>Kareem Abdul-Jabbar*</td>
      <td>C</td>
      <td>33.0</td>
      <td>LAL</td>
      <td>80.0</td>
      <td>NaN</td>
      <td>2976.0</td>
      <td>25.5</td>
      <td>0.616</td>
      <td>0.001</td>
      <td>0.379</td>
      <td>7.6</td>
      <td>21.5</td>
      <td>15.0</td>
      <td>13.6</td>
      <td>0.9</td>
      <td>4.0</td>
      <td>12.8</td>
      <td>26.3</td>
      <td>NaN</td>
      <td>9.6</td>
      <td>4.6</td>
      <td>14.3</td>
      <td>0.230</td>
      <td>NaN</td>
      <td>3.9</td>
      <td>1.4</td>
      <td>5.3</td>
      <td>5.4</td>
      <td>836.0</td>
      <td>1457.0</td>
      <td>0.574</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>836.0</td>
      <td>1456.0</td>
      <td>0.574</td>
      <td>0.574</td>
      <td>423.0</td>
      <td>552.0</td>
      <td>0.766</td>
      <td>197.0</td>
      <td>624.0</td>
      <td>821.0</td>
      <td>272.0</td>
      <td>59.0</td>
      <td>228.0</td>
      <td>249.0</td>
      <td>244.0</td>
      <td>2095.0</td>
    </tr>
    <tr>
      <th>6086</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>39.0</td>
      <td>NaN</td>
      <td>298.0</td>
      <td>8.0</td>
      <td>0.459</td>
      <td>0.017</td>
      <td>0.373</td>
      <td>7.1</td>
      <td>10.6</td>
      <td>8.8</td>
      <td>8.0</td>
      <td>1.1</td>
      <td>0.6</td>
      <td>10.4</td>
      <td>10.3</td>
      <td>NaN</td>
      <td>0.2</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.062</td>
      <td>NaN</td>
      <td>-2.6</td>
      <td>-0.7</td>
      <td>-3.2</td>
      <td>-0.1</td>
      <td>25.0</td>
      <td>59.0</td>
      <td>0.424</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>25.0</td>
      <td>58.0</td>
      <td>0.431</td>
      <td>0.424</td>
      <td>13.0</td>
      <td>22.0</td>
      <td>0.591</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>48.0</td>
      <td>19.0</td>
      <td>7.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>34.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>6087</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>GSW</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>39.0</td>
      <td>3.2</td>
      <td>0.463</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2.8</td>
      <td>20.2</td>
      <td>11.4</td>
      <td>2.9</td>
      <td>1.2</td>
      <td>0.0</td>
      <td>31.6</td>
      <td>6.4</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-0.004</td>
      <td>NaN</td>
      <td>-6.0</td>
      <td>-0.2</td>
      <td>-6.2</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>0.333</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>0.667</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>6088</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>IND</td>
      <td>29.0</td>
      <td>NaN</td>
      <td>259.0</td>
      <td>8.7</td>
      <td>0.458</td>
      <td>0.018</td>
      <td>0.339</td>
      <td>7.8</td>
      <td>9.1</td>
      <td>8.4</td>
      <td>8.8</td>
      <td>1.1</td>
      <td>0.7</td>
      <td>8.5</td>
      <td>10.9</td>
      <td>NaN</td>
      <td>0.2</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.072</td>
      <td>NaN</td>
      <td>-2.0</td>
      <td>-0.8</td>
      <td>-2.8</td>
      <td>-0.1</td>
      <td>24.0</td>
      <td>56.0</td>
      <td>0.429</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>24.0</td>
      <td>55.0</td>
      <td>0.436</td>
      <td>0.429</td>
      <td>11.0</td>
      <td>19.0</td>
      <td>0.579</td>
      <td>19.0</td>
      <td>21.0</td>
      <td>40.0</td>
      <td>18.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>29.0</td>
      <td>59.0</td>
    </tr>
    <tr>
      <th>6089</th>
      <td>1981</td>
      <td>Alvan Adams</td>
      <td>C</td>
      <td>26.0</td>
      <td>PHO</td>
      <td>75.0</td>
      <td>NaN</td>
      <td>2054.0</td>
      <td>20.3</td>
      <td>0.567</td>
      <td>0.000</td>
      <td>0.298</td>
      <td>8.6</td>
      <td>20.5</td>
      <td>14.7</td>
      <td>24.5</td>
      <td>2.4</td>
      <td>1.9</td>
      <td>18.7</td>
      <td>23.0</td>
      <td>NaN</td>
      <td>3.3</td>
      <td>4.5</td>
      <td>7.7</td>
      <td>0.180</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>3.3</td>
      <td>5.3</td>
      <td>3.8</td>
      <td>458.0</td>
      <td>870.0</td>
      <td>0.526</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>458.0</td>
      <td>870.0</td>
      <td>0.526</td>
      <td>0.526</td>
      <td>199.0</td>
      <td>259.0</td>
      <td>0.768</td>
      <td>157.0</td>
      <td>389.0</td>
      <td>546.0</td>
      <td>344.0</td>
      <td>106.0</td>
      <td>69.0</td>
      <td>226.0</td>
      <td>226.0</td>
      <td>1115.0</td>
    </tr>
  </tbody>
</table>
</div>



Let's get rid of some columns that either don't make sense or aren't important.


```
data['Seasons_Stats'] = data['Seasons_Stats'].drop(['GS', 'blanl', 'blank2'], axis=1)
data['Seasons_Stats'].head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6085</th>
      <td>1981</td>
      <td>Kareem Abdul-Jabbar*</td>
      <td>C</td>
      <td>33.0</td>
      <td>LAL</td>
      <td>80.0</td>
      <td>2976.0</td>
      <td>25.5</td>
      <td>0.616</td>
      <td>0.001</td>
      <td>0.379</td>
      <td>7.6</td>
      <td>21.5</td>
      <td>15.0</td>
      <td>13.6</td>
      <td>0.9</td>
      <td>4.0</td>
      <td>12.8</td>
      <td>26.3</td>
      <td>9.6</td>
      <td>4.6</td>
      <td>14.3</td>
      <td>0.230</td>
      <td>3.9</td>
      <td>1.4</td>
      <td>5.3</td>
      <td>5.4</td>
      <td>836.0</td>
      <td>1457.0</td>
      <td>0.574</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>836.0</td>
      <td>1456.0</td>
      <td>0.574</td>
      <td>0.574</td>
      <td>423.0</td>
      <td>552.0</td>
      <td>0.766</td>
      <td>197.0</td>
      <td>624.0</td>
      <td>821.0</td>
      <td>272.0</td>
      <td>59.0</td>
      <td>228.0</td>
      <td>249.0</td>
      <td>244.0</td>
      <td>2095.0</td>
    </tr>
    <tr>
      <th>6086</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>39.0</td>
      <td>298.0</td>
      <td>8.0</td>
      <td>0.459</td>
      <td>0.017</td>
      <td>0.373</td>
      <td>7.1</td>
      <td>10.6</td>
      <td>8.8</td>
      <td>8.0</td>
      <td>1.1</td>
      <td>0.6</td>
      <td>10.4</td>
      <td>10.3</td>
      <td>0.2</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.062</td>
      <td>-2.6</td>
      <td>-0.7</td>
      <td>-3.2</td>
      <td>-0.1</td>
      <td>25.0</td>
      <td>59.0</td>
      <td>0.424</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>25.0</td>
      <td>58.0</td>
      <td>0.431</td>
      <td>0.424</td>
      <td>13.0</td>
      <td>22.0</td>
      <td>0.591</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>48.0</td>
      <td>19.0</td>
      <td>7.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>34.0</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>6087</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>GSW</td>
      <td>10.0</td>
      <td>39.0</td>
      <td>3.2</td>
      <td>0.463</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2.8</td>
      <td>20.2</td>
      <td>11.4</td>
      <td>2.9</td>
      <td>1.2</td>
      <td>0.0</td>
      <td>31.6</td>
      <td>6.4</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-0.004</td>
      <td>-6.0</td>
      <td>-0.2</td>
      <td>-6.2</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>0.333</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>0.667</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>6088</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>IND</td>
      <td>29.0</td>
      <td>259.0</td>
      <td>8.7</td>
      <td>0.458</td>
      <td>0.018</td>
      <td>0.339</td>
      <td>7.8</td>
      <td>9.1</td>
      <td>8.4</td>
      <td>8.8</td>
      <td>1.1</td>
      <td>0.7</td>
      <td>8.5</td>
      <td>10.9</td>
      <td>0.2</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.072</td>
      <td>-2.0</td>
      <td>-0.8</td>
      <td>-2.8</td>
      <td>-0.1</td>
      <td>24.0</td>
      <td>56.0</td>
      <td>0.429</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>24.0</td>
      <td>55.0</td>
      <td>0.436</td>
      <td>0.429</td>
      <td>11.0</td>
      <td>19.0</td>
      <td>0.579</td>
      <td>19.0</td>
      <td>21.0</td>
      <td>40.0</td>
      <td>18.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>29.0</td>
      <td>59.0</td>
    </tr>
    <tr>
      <th>6089</th>
      <td>1981</td>
      <td>Alvan Adams</td>
      <td>C</td>
      <td>26.0</td>
      <td>PHO</td>
      <td>75.0</td>
      <td>2054.0</td>
      <td>20.3</td>
      <td>0.567</td>
      <td>0.000</td>
      <td>0.298</td>
      <td>8.6</td>
      <td>20.5</td>
      <td>14.7</td>
      <td>24.5</td>
      <td>2.4</td>
      <td>1.9</td>
      <td>18.7</td>
      <td>23.0</td>
      <td>3.3</td>
      <td>4.5</td>
      <td>7.7</td>
      <td>0.180</td>
      <td>2.0</td>
      <td>3.3</td>
      <td>5.3</td>
      <td>3.8</td>
      <td>458.0</td>
      <td>870.0</td>
      <td>0.526</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>458.0</td>
      <td>870.0</td>
      <td>0.526</td>
      <td>0.526</td>
      <td>199.0</td>
      <td>259.0</td>
      <td>0.768</td>
      <td>157.0</td>
      <td>389.0</td>
      <td>546.0</td>
      <td>344.0</td>
      <td>106.0</td>
      <td>69.0</td>
      <td>226.0</td>
      <td>226.0</td>
      <td>1115.0</td>
    </tr>
  </tbody>
</table>
</div>



Now might be a good time to learn what the column names actually mean. Some of these abbreviations are difficult to decipher so I'll figure that out now.


```
data['Seasons_Stats'].columns
```




    Index(['Year', 'Player', 'Pos', 'Age', 'Tm', 'G', 'MP', 'PER', 'TS%', '3PAr',
           'FTr', 'ORB%', 'DRB%', 'TRB%', 'AST%', 'STL%', 'BLK%', 'TOV%', 'USG%',
           'OWS', 'DWS', 'WS', 'WS/48', 'OBPM', 'DBPM', 'BPM', 'VORP', 'FG', 'FGA',
           'FG%', '3P', '3PA', '3P%', '2P', '2PA', '2P%', 'eFG%', 'FT', 'FTA',
           'FT%', 'ORB', 'DRB', 'TRB', 'AST', 'STL', 'BLK', 'TOV', 'PF', 'PTS'],
          dtype='object')



Here are the meanings of the statistics:



*   'Pos' - position
*   'Tm' - team
*   'G' - games played
*   'MP' - minutes played
*   'PER' - player efficiency rating
*   'TS%' - true shooting percentage (weights 3-pointers higher)
*   '3PAr' - 3-point attempt rate
*   'Ftr' - free throw attempt rate
*   'ORB%' - offensive rebound percentage
*   'DRB%' - defensive rebound percentage
*   'TRB%' - total rebound percentage
*   'AST%' - assist percentage
*   'STL%' - steal percentage
*   'BLK%' - block percentage
*   'TOV%' - turnover percentage
*   'USG%' - usage rate
*   'OWS' - offensive win shares
*   'DWS' - defensive win shares
*   'WS' - win shares
*   'WS/48' - win shares over 48 minutes
*   'OBPM' - offensive box plus/minus
*   'DBPM' - defensive box plus/minus
*   'BPM' - box plus/minus
*   'VORP' - value over replacement player
*   'FG' - field goals made
*   'FGA' - field goals attempted
*   'FG%' - field goal percentage'
*   '3P' - 3-pointers made
*   '3PA' - 3-pointers attempted
*   '3P%' - 3-point percentage
*   '2P' - 2-pointers made
*   '2PA' - 2-pointers attempted
*   '2P%' - 2-point percentage'
*   'eFG%' - effective field goal percentage
*   'FT' - free throws made
*   'FTA' - free throws attempted
*   'FT%' - free throw percentage
*   'ORB' - offensive rebounds
*   'DRB' - defensive rebounds
*   'TRB' - total rebounds
*   'AST' - assists
*   'STL' - steals
*   'BLK' - blocks
*   'TOV' - turnovers
*   'PF' - personal fouls
*   'PTS' - points













































Before I can clean up other datasets and eventually join a few to make a big dataframe, I need to make sure that there are no more annoying NaN values lurking in the data


```
data['Seasons_Stats'].count()

# The total number of rows is 18570, but some of the columns are missing values. To simplify everything, I'm just going to turn those missing values to the value of zero.
```




    Year      18570
    Player    18570
    Pos       18570
    Age       18570
    Tm        18570
    G         18570
    MP        18570
    PER       18565
    TS%       18494
    3PAr      18483
    FTr       18483
    ORB%      18565
    DRB%      18565
    TRB%      18565
    AST%      18565
    STL%      18565
    BLK%      18565
    TOV%      18509
    USG%      18565
    OWS       18570
    DWS       18570
    WS        18570
    WS/48     18565
    OBPM      18570
    DBPM      18570
    BPM       18570
    VORP      18570
    FG        18570
    FGA       18570
    FG%       18483
    3P        18570
    3PA       18570
    3P%       15143
    2P        18570
    2PA       18570
    2P%       18454
    eFG%      18483
    FT        18570
    FTA       18570
    FT%       17828
    ORB       18570
    DRB       18570
    TRB       18570
    AST       18570
    STL       18570
    BLK       18570
    TOV       18570
    PF        18570
    PTS       18570
    dtype: int64




```
data['Seasons_Stats'] = data['Seasons_Stats'].fillna(0)

```


```
data['Seasons_Stats'].count()

# That's much better.
```




    Year      18570
    Player    18570
    Pos       18570
    Age       18570
    Tm        18570
    G         18570
    MP        18570
    PER       18570
    TS%       18570
    3PAr      18570
    FTr       18570
    ORB%      18570
    DRB%      18570
    TRB%      18570
    AST%      18570
    STL%      18570
    BLK%      18570
    TOV%      18570
    USG%      18570
    OWS       18570
    DWS       18570
    WS        18570
    WS/48     18570
    OBPM      18570
    DBPM      18570
    BPM       18570
    VORP      18570
    FG        18570
    FGA       18570
    FG%       18570
    3P        18570
    3PA       18570
    3P%       18570
    2P        18570
    2PA       18570
    2P%       18570
    eFG%      18570
    FT        18570
    FTA       18570
    FT%       18570
    ORB       18570
    DRB       18570
    TRB       18570
    AST       18570
    STL       18570
    BLK       18570
    TOV       18570
    PF        18570
    PTS       18570
    dtype: int64



## Salary Data

Lesson learned: data preparation and data cleaning are WITHOUT A DOUBT the most tedious and time-consuming part of data science.

After actually looking (briefly) through the datasets, I see that I need to perform some data wizardry with the 1990-2018 dataframe to extract important salary information.

As mentioned earlier, because of the effect of inflation and time-related issues, I can't just put all of the salaries together and then use machine learning algorithms on those. I need a way to standardize all of the data. That's why I decided to create some metrics like: player's salary as proportion of team payroll, team's payroll as proportion of total payroll (market size), player's salary as proportion of total payroll in the NBA.

Time to manipulate some more data.


```
data['1990_to_2018'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>player</th>
      <th>salary</th>
      <th>season_end</th>
      <th>season_start</th>
      <th>team</th>
      <th>team_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>1991</td>
      <td>1990</td>
      <td>ATL</td>
      <td>Atlanta Hawks</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>1991</td>
      <td>1990</td>
      <td>ATL</td>
      <td>Atlanta Hawks</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>1991</td>
      <td>1990</td>
      <td>ATL</td>
      <td>Atlanta Hawks</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>1991</td>
      <td>1990</td>
      <td>ATL</td>
      <td>Atlanta Hawks</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>1991</td>
      <td>1990</td>
      <td>ATL</td>
      <td>Atlanta Hawks</td>
    </tr>
  </tbody>
</table>
</div>




```
data['1990_to_2018'].count()
```




    player          11838
    salary          11838
    season_end      11838
    season_start    11838
    team            11838
    team_name       11838
    dtype: int64



It took me a while, but I managed to create some new features based on this dataset, including: team payroll, total NBA payroll, and year.

At first I just downloaded the file into Excel and did some Excel formulas to generate the values, but that was too inefficient, so I created a robust Python script that automated the task VERY quickly.

Here is the [notebook](https://colab.research.google.com/drive/1SV3Q8RLvyQvobya2SVshq9_D4qYUForU) on which I did this.

Now it's time to bring in the new and improved salary data.


```
url = 'https://raw.githubusercontent.com/jerrytigerxu/NBA-Salary-Prediction/master/data/nba_salaries.csv'

data['salaries'] = pd.read_csv(url)

data['salaries'] = data['salaries'].drop(data['salaries'].columns[0], axis=1)
```


```
data['salaries'].head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1991</td>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1991</td>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1991</td>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991</td>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1991</td>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
    </tr>
  </tbody>
</table>
</div>



## Feature Engineering

Now is where the fun really begins! 

Let's make some more features that will aid us in our analysis of what are the most predictors of a high salary.

A few I'll make right off the bat are: 

*   Salary as proportion of team payroll (player leverage)
*   Salary as proportion of total NBA payroll (league weight)
*   Team payroll as proportion of total NBA payroll (team market size)
*   Region of US












```
# Creating a Player Leverage column

data['salaries']['Player Leverage'] = data['salaries']['Salary'] / data['salaries']['Team Payroll'] 
```


```
data['salaries'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1991</td>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.204574</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1991</td>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.175580</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1991</td>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.131792</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991</td>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.076099</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1991</td>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.068021</td>
    </tr>
  </tbody>
</table>
</div>




```
# Creating a League Weight column
data['salaries']['League Weight'] = data['salaries']['Salary'] / data['salaries']['Total NBA Payroll']
```


```
data['salaries'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1991</td>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.204574</td>
      <td>0.008196</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1991</td>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.175580</td>
      <td>0.007034</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1991</td>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.131792</td>
      <td>0.005280</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991</td>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.076099</td>
      <td>0.003049</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1991</td>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.068021</td>
      <td>0.002725</td>
    </tr>
  </tbody>
</table>
</div>




```
# Creating a Team Market Size column
data['salaries']['Team Market Size'] = data['salaries']['Team Payroll'] / data['salaries']['Total NBA Payroll']
```


```
data['salaries'].head(50)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1991</td>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.204574</td>
      <td>0.008196</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1991</td>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.175580</td>
      <td>0.007034</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1991</td>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.131792</td>
      <td>0.005280</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991</td>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.076099</td>
      <td>0.003049</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1991</td>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.068021</td>
      <td>0.002725</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1991</td>
      <td>Tim McCormick</td>
      <td>775000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.065896</td>
      <td>0.002640</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1991</td>
      <td>Kevin Willis</td>
      <td>685000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.058243</td>
      <td>0.002333</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1991</td>
      <td>Alexander Volkov</td>
      <td>650000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.055267</td>
      <td>0.002214</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1991</td>
      <td>John Battle</td>
      <td>590000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.050166</td>
      <td>0.002010</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1991</td>
      <td>Sidney Moncrief</td>
      <td>510000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.043364</td>
      <td>0.001737</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1991</td>
      <td>Spud Webb</td>
      <td>510000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.043364</td>
      <td>0.001737</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1991</td>
      <td>Gary Leonard</td>
      <td>200000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.017005</td>
      <td>0.000681</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1991</td>
      <td>Duane Ferrell</td>
      <td>125000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.010628</td>
      <td>0.000426</td>
      <td>0.040063</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1991</td>
      <td>Robert Parish</td>
      <td>2500000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.222104</td>
      <td>0.008516</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1991</td>
      <td>Larry Bird</td>
      <td>1500000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.133262</td>
      <td>0.005110</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1991</td>
      <td>Kevin McHale</td>
      <td>1400000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.124378</td>
      <td>0.004769</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1991</td>
      <td>Brian Shaw</td>
      <td>1212000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.107676</td>
      <td>0.004129</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1991</td>
      <td>Joe Kleine</td>
      <td>850000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.075515</td>
      <td>0.002895</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1991</td>
      <td>Ed Pinckney</td>
      <td>750000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.066631</td>
      <td>0.002555</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1991</td>
      <td>John Bagley</td>
      <td>550000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.048863</td>
      <td>0.001874</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1991</td>
      <td>Dee Brown</td>
      <td>547000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.048596</td>
      <td>0.001863</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1991</td>
      <td>Michael Smith</td>
      <td>525000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.046642</td>
      <td>0.001788</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1991</td>
      <td>Reggie Lewis</td>
      <td>400000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.035537</td>
      <td>0.001363</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1991</td>
      <td>Kevin Gamble</td>
      <td>375000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.033316</td>
      <td>0.001277</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1991</td>
      <td>Derek Smith</td>
      <td>315000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.027985</td>
      <td>0.001073</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1991</td>
      <td>Stojko Vrankovic</td>
      <td>222000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.019723</td>
      <td>0.000756</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1991</td>
      <td>Dave Popson</td>
      <td>80000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.007107</td>
      <td>0.000273</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1991</td>
      <td>A.J. Wynder</td>
      <td>30000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.002665</td>
      <td>0.000102</td>
      <td>0.038343</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1991</td>
      <td>Mike Gminski</td>
      <td>1650000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.158395</td>
      <td>0.005621</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1991</td>
      <td>Kendall Gill</td>
      <td>1500000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.143995</td>
      <td>0.005110</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>30</th>
      <td>1991</td>
      <td>J.R. Reid</td>
      <td>1250000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.119996</td>
      <td>0.004258</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>31</th>
      <td>1991</td>
      <td>Johnny Newman</td>
      <td>1200000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.115196</td>
      <td>0.004088</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>32</th>
      <td>1991</td>
      <td>Kelly Tripucka</td>
      <td>1000000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.095997</td>
      <td>0.003406</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>33</th>
      <td>1991</td>
      <td>Dell Curry</td>
      <td>900000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.086397</td>
      <td>0.003066</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>34</th>
      <td>1991</td>
      <td>Muggsy Bogues</td>
      <td>805000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.077278</td>
      <td>0.002742</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>35</th>
      <td>1991</td>
      <td>Rex Chapman</td>
      <td>675000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.064798</td>
      <td>0.002299</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>36</th>
      <td>1991</td>
      <td>Eric Leckner</td>
      <td>485000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.046559</td>
      <td>0.001652</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>37</th>
      <td>1991</td>
      <td>Kenny Gattison</td>
      <td>355000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.034079</td>
      <td>0.001209</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>38</th>
      <td>1991</td>
      <td>Randolph Keys</td>
      <td>322000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.030911</td>
      <td>0.001097</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>39</th>
      <td>1991</td>
      <td>Steve Scheffler</td>
      <td>200000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.019199</td>
      <td>0.000681</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>40</th>
      <td>1991</td>
      <td>Scott Haffner</td>
      <td>75000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.007200</td>
      <td>0.000255</td>
      <td>0.035485</td>
    </tr>
    <tr>
      <th>41</th>
      <td>1991</td>
      <td>Michael Jordan</td>
      <td>2500000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.249004</td>
      <td>0.008516</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>42</th>
      <td>1991</td>
      <td>Bill Cartwright</td>
      <td>1100000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.109562</td>
      <td>0.003747</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>43</th>
      <td>1991</td>
      <td>Horace Grant</td>
      <td>1000000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.099602</td>
      <td>0.003406</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>44</th>
      <td>1991</td>
      <td>Stacey King</td>
      <td>1000000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.099602</td>
      <td>0.003406</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>45</th>
      <td>1991</td>
      <td>Dennis Hopson</td>
      <td>915000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.091135</td>
      <td>0.003117</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>46</th>
      <td>1991</td>
      <td>Scottie Pippen</td>
      <td>765000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.076195</td>
      <td>0.002606</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>47</th>
      <td>1991</td>
      <td>Cliff Levingston</td>
      <td>750000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.074701</td>
      <td>0.002555</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>48</th>
      <td>1991</td>
      <td>Craig Hodges</td>
      <td>600000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.059761</td>
      <td>0.002044</td>
      <td>0.034200</td>
    </tr>
    <tr>
      <th>49</th>
      <td>1991</td>
      <td>Will Perdue</td>
      <td>450000</td>
      <td>Chicago Bulls</td>
      <td>10040000</td>
      <td>293563000</td>
      <td>0.044821</td>
      <td>0.001533</td>
      <td>0.034200</td>
    </tr>
  </tbody>
</table>
</div>




```
# Just for fun, using the League Weight column, I can quickly pinpoint which player was paid the most in a particular year

highest = data['salaries']['League Weight'] == max(data['salaries'][data['salaries']['Year'] == 1991]['League Weight'])

data['salaries'][highest]['Player']
```




    224    Patrick Ewing
    Name: Player, dtype: object




```
# Let's find the find the highest paid players for every year 

start_year = 1991

for i in range(17):
  highest = data['salaries']['League Weight'] == max(data['salaries'][data['salaries']['Year'] == (1991+i)]['League Weight'])

  print(data['salaries'][highest]['Player'].values)
  
```

    ['Patrick Ewing']
    ['Larry Bird']
    ['David Robinson']
    ['David Robinson']
    ['David Robinson']
    ['Patrick Ewing']
    ['Michael Jordan']
    ['Michael Jordan']
    ['Patrick Ewing']
    ["Shaquille O\\'Neal"]
    ['Kevin Garnett']
    ['Kevin Garnett']
    ['Kevin Garnett']
    ['Kevin Garnett']
    ["Shaquille O\\'Neal"]
    ["Shaquille O\\'Neal"]
    ['Kevin Garnett']


Now let's create the US Region column.

I'll make it simple and divide up the country (as well as Toronto) into four regions based on how the US government does it: Northeast, Midwest, South, and West


**Northeast**: New York, Boston, Brooklyn, Philadelphia, Toronto

**Midwest**: Cleveland, Detroit, Indiana, Milwaukee, Minnesota, Chicago

**South**: Atlanta, Charlotte, Miami, Orlando, New Orleans, Washington, Dallas, Oklahoma City, Houston, San Antonio, Memphis

**West**: Golden State, Denver, Utah, Sacramento, Los Angeles Lakers, Los Angeles Clippers, Phoenix, Portland




```
data['salaries']['Team'].unique()
```




    array(['Atlanta Hawks', 'Boston Celtic', 'Charlotte Hornets',
           'Chicago Bulls', 'Cleveland Caveliers', 'Dallas Mavericks',
           'Denver Nuggets', 'Detroit Pistons', 'Golden State Warriors',
           'Houston Rockets', 'Indiana Pacers', 'Los Angeles Clippers',
           'Los Angeles Lakers', 'Miami Heat', 'Milwaukee Bucks',
           'Minnesota Timberwolves', 'Brooklyn Nets', 'New York Knicks',
           'Oklahoma City Thunder', 'Orlando Magic', 'Philadelphia 76ers',
           'Phoenix Suns', 'Portland Trail Blazers', 'Sacramento Kings',
           'San Antonio Spurs', 'Utah Jazz', 'Washington Wizards',
           'Memphis Grizzlies', 'Toronto Raptors', 'New Orleans Pelicans'],
          dtype=object)




```
data['salaries']['US Region'] = 0

# Dummy entry
```


```
data['salaries'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1991</td>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.204574</td>
      <td>0.008196</td>
      <td>0.040063</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1991</td>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.175580</td>
      <td>0.007034</td>
      <td>0.040063</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1991</td>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.131792</td>
      <td>0.005280</td>
      <td>0.040063</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991</td>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.076099</td>
      <td>0.003049</td>
      <td>0.040063</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1991</td>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.068021</td>
      <td>0.002725</td>
      <td>0.040063</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```
for rowNum in range(len(data['salaries'].index.values)):
  if data['salaries'].iloc[rowNum]['Team'] in ['New York Knicks', 'Boston Celtic', 'Brooklyn Nets', 'Philadelphia 76ers', 'Toronto Raptors']:
    data['salaries'].loc[rowNum, 'US Region'] = 'Northeast'
  elif data['salaries'].iloc[rowNum]['Team'] in ['Cleveland Caveliers', 'Detroit Pistons', 'Indiana Pacers', 'Milwaukee Bucks', 'Minnesota Timberwolves', 'Chicago Bulls']:
    data['salaries'].loc[rowNum, 'US Region'] = 'Midwest'
  elif data['salaries'].iloc[rowNum]['Team'] in ['Atlanta Hawks', 'Charlotte Hornets', 'Miami Heat', 'Orlando Magic', 'New Orleans Pelicans', 'Washington Wizards', 'Dallas Mavericks', 'Oklahoma City Thunder', 'Houston Rockets', 'San Antonio Spurs', 'Memphis Grizzlies']:
    data['salaries'].loc[rowNum, 'US Region'] = 'South'
  elif data['salaries'].iloc[rowNum]['Team'] in ['Golden State Warriors', 'Denver Nuggets', 'Utah Jazz', 'Sacramento Kings', 'Los Angeles Lakers', 'Los Angeles Clippers', 'Phoenix Suns', 'Portland Trail Blazers']:
    data['salaries'].loc[rowNum, 'US Region'] = 'West'
```


```
data['salaries'].head(100)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1991</td>
      <td>Moses Malone</td>
      <td>2406000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.204574</td>
      <td>0.008196</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1991</td>
      <td>Dominique Wilkins</td>
      <td>2065000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.175580</td>
      <td>0.007034</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1991</td>
      <td>Jon Koncak</td>
      <td>1550000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.131792</td>
      <td>0.005280</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991</td>
      <td>Doc Rivers</td>
      <td>895000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.076099</td>
      <td>0.003049</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1991</td>
      <td>Rumeal Robinson</td>
      <td>800000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.068021</td>
      <td>0.002725</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1991</td>
      <td>Tim McCormick</td>
      <td>775000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.065896</td>
      <td>0.002640</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1991</td>
      <td>Kevin Willis</td>
      <td>685000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.058243</td>
      <td>0.002333</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1991</td>
      <td>Alexander Volkov</td>
      <td>650000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.055267</td>
      <td>0.002214</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1991</td>
      <td>John Battle</td>
      <td>590000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.050166</td>
      <td>0.002010</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1991</td>
      <td>Sidney Moncrief</td>
      <td>510000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.043364</td>
      <td>0.001737</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1991</td>
      <td>Spud Webb</td>
      <td>510000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.043364</td>
      <td>0.001737</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1991</td>
      <td>Gary Leonard</td>
      <td>200000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.017005</td>
      <td>0.000681</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1991</td>
      <td>Duane Ferrell</td>
      <td>125000</td>
      <td>Atlanta Hawks</td>
      <td>11761000</td>
      <td>293563000</td>
      <td>0.010628</td>
      <td>0.000426</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1991</td>
      <td>Robert Parish</td>
      <td>2500000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.222104</td>
      <td>0.008516</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1991</td>
      <td>Larry Bird</td>
      <td>1500000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.133262</td>
      <td>0.005110</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1991</td>
      <td>Kevin McHale</td>
      <td>1400000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.124378</td>
      <td>0.004769</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1991</td>
      <td>Brian Shaw</td>
      <td>1212000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.107676</td>
      <td>0.004129</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1991</td>
      <td>Joe Kleine</td>
      <td>850000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.075515</td>
      <td>0.002895</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1991</td>
      <td>Ed Pinckney</td>
      <td>750000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.066631</td>
      <td>0.002555</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1991</td>
      <td>John Bagley</td>
      <td>550000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.048863</td>
      <td>0.001874</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1991</td>
      <td>Dee Brown</td>
      <td>547000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.048596</td>
      <td>0.001863</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1991</td>
      <td>Michael Smith</td>
      <td>525000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.046642</td>
      <td>0.001788</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1991</td>
      <td>Reggie Lewis</td>
      <td>400000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.035537</td>
      <td>0.001363</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1991</td>
      <td>Kevin Gamble</td>
      <td>375000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.033316</td>
      <td>0.001277</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1991</td>
      <td>Derek Smith</td>
      <td>315000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.027985</td>
      <td>0.001073</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1991</td>
      <td>Stojko Vrankovic</td>
      <td>222000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.019723</td>
      <td>0.000756</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1991</td>
      <td>Dave Popson</td>
      <td>80000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.007107</td>
      <td>0.000273</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1991</td>
      <td>A.J. Wynder</td>
      <td>30000</td>
      <td>Boston Celtic</td>
      <td>11256000</td>
      <td>293563000</td>
      <td>0.002665</td>
      <td>0.000102</td>
      <td>0.038343</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1991</td>
      <td>Mike Gminski</td>
      <td>1650000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.158395</td>
      <td>0.005621</td>
      <td>0.035485</td>
      <td>South</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1991</td>
      <td>Kendall Gill</td>
      <td>1500000</td>
      <td>Charlotte Hornets</td>
      <td>10417000</td>
      <td>293563000</td>
      <td>0.143995</td>
      <td>0.005110</td>
      <td>0.035485</td>
      <td>South</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>70</th>
      <td>1991</td>
      <td>Fat Lever</td>
      <td>1519000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.129907</td>
      <td>0.005174</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>71</th>
      <td>1991</td>
      <td>Alex English</td>
      <td>1500000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.128282</td>
      <td>0.005110</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>72</th>
      <td>1991</td>
      <td>Herb Williams</td>
      <td>1000000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.085521</td>
      <td>0.003406</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>73</th>
      <td>1991</td>
      <td>Rodney McCray</td>
      <td>985000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.084238</td>
      <td>0.003355</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>74</th>
      <td>1991</td>
      <td>James Donaldson</td>
      <td>880000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.075259</td>
      <td>0.002998</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>75</th>
      <td>1991</td>
      <td>Roy Tarpley</td>
      <td>765000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.065424</td>
      <td>0.002606</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>76</th>
      <td>1991</td>
      <td>Randy White</td>
      <td>730000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.062431</td>
      <td>0.002487</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>77</th>
      <td>1991</td>
      <td>Brad Davis</td>
      <td>600000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.051313</td>
      <td>0.002044</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>78</th>
      <td>1991</td>
      <td>Steve Alford</td>
      <td>250000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.021380</td>
      <td>0.000852</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>79</th>
      <td>1991</td>
      <td>John Shasky</td>
      <td>150000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.012828</td>
      <td>0.000511</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>80</th>
      <td>1991</td>
      <td>Jim Grandholm</td>
      <td>115000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.009835</td>
      <td>0.000392</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>81</th>
      <td>1991</td>
      <td>Kelvin Upshaw</td>
      <td>30000</td>
      <td>Dallas Mavericks</td>
      <td>11693000</td>
      <td>293563000</td>
      <td>0.002566</td>
      <td>0.000102</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>82</th>
      <td>1991</td>
      <td>Blair Rasmussen</td>
      <td>2185000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.211418</td>
      <td>0.007443</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>83</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>1660000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.160619</td>
      <td>0.005655</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>84</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>1365000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>85</th>
      <td>1991</td>
      <td>Joe Wolf</td>
      <td>990000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.095791</td>
      <td>0.003372</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>86</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>825000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.079826</td>
      <td>0.002810</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>87</th>
      <td>1991</td>
      <td>Orlando Woolridge</td>
      <td>805000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.077891</td>
      <td>0.002742</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>88</th>
      <td>1991</td>
      <td>Todd Lichti</td>
      <td>565000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.054669</td>
      <td>0.001925</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>89</th>
      <td>1991</td>
      <td>Bill Hanzlik</td>
      <td>510000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.049347</td>
      <td>0.001737</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>90</th>
      <td>1991</td>
      <td>Jerome Lane</td>
      <td>500000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.048379</td>
      <td>0.001703</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>91</th>
      <td>1991</td>
      <td>Anthony Cook</td>
      <td>370000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.035801</td>
      <td>0.001260</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>92</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>350000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>93</th>
      <td>1991</td>
      <td>Marcus Liberty</td>
      <td>210000</td>
      <td>Denver Nuggets</td>
      <td>10335000</td>
      <td>293563000</td>
      <td>0.020319</td>
      <td>0.000715</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>94</th>
      <td>1991</td>
      <td>Isiah Thomas</td>
      <td>2720000</td>
      <td>Detroit Pistons</td>
      <td>12910000</td>
      <td>293563000</td>
      <td>0.210689</td>
      <td>0.009265</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>95</th>
      <td>1991</td>
      <td>Bill Laimbeer</td>
      <td>1510000</td>
      <td>Detroit Pistons</td>
      <td>12910000</td>
      <td>293563000</td>
      <td>0.116964</td>
      <td>0.005144</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>96</th>
      <td>1991</td>
      <td>Vinnie Johnson</td>
      <td>1400000</td>
      <td>Detroit Pistons</td>
      <td>12910000</td>
      <td>293563000</td>
      <td>0.108443</td>
      <td>0.004769</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>97</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>1115000</td>
      <td>Detroit Pistons</td>
      <td>12910000</td>
      <td>293563000</td>
      <td>0.086367</td>
      <td>0.003798</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>98</th>
      <td>1991</td>
      <td>Joe Dumars</td>
      <td>1035000</td>
      <td>Detroit Pistons</td>
      <td>12910000</td>
      <td>293563000</td>
      <td>0.080170</td>
      <td>0.003526</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>99</th>
      <td>1991</td>
      <td>James Edwards</td>
      <td>935000</td>
      <td>Detroit Pistons</td>
      <td>12910000</td>
      <td>293563000</td>
      <td>0.072424</td>
      <td>0.003185</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 10 columns</p>
</div>



## Merging Datasets

Now it's time to put together the two datasets that we've spent a lot of time working on!

Because the salary data starts in the 90s, I'm going to have to cut off another decade off of my 'Seasons_Stats' dataframe. This might hurt some of the analysis because it only encompasses the modern era of the NBA, but because of the proportion features I've created, much of the effect of time should (hopefully) be mitigated.


```
data['player_stats'] = pd.merge(data['Seasons_Stats'], data['salaries'], how='left', left_on=['Year', 'Player'], right_on=['Year', 'Player'])
```




```
data['player_stats'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1981</td>
      <td>Kareem Abdul-Jabbar*</td>
      <td>C</td>
      <td>33.0</td>
      <td>LAL</td>
      <td>80.0</td>
      <td>2976.0</td>
      <td>25.5</td>
      <td>0.616</td>
      <td>0.001</td>
      <td>0.379</td>
      <td>7.6</td>
      <td>21.5</td>
      <td>15.0</td>
      <td>13.6</td>
      <td>0.9</td>
      <td>4.0</td>
      <td>12.8</td>
      <td>26.3</td>
      <td>9.6</td>
      <td>4.6</td>
      <td>14.3</td>
      <td>0.230</td>
      <td>3.9</td>
      <td>1.4</td>
      <td>5.3</td>
      <td>5.4</td>
      <td>836.0</td>
      <td>1457.0</td>
      <td>0.574</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>836.0</td>
      <td>1456.0</td>
      <td>0.574</td>
      <td>0.574</td>
      <td>423.0</td>
      <td>552.0</td>
      <td>0.766</td>
      <td>197.0</td>
      <td>624.0</td>
      <td>821.0</td>
      <td>272.0</td>
      <td>59.0</td>
      <td>228.0</td>
      <td>249.0</td>
      <td>244.0</td>
      <td>2095.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>39.0</td>
      <td>298.0</td>
      <td>8.0</td>
      <td>0.459</td>
      <td>0.017</td>
      <td>0.373</td>
      <td>7.1</td>
      <td>10.6</td>
      <td>8.8</td>
      <td>8.0</td>
      <td>1.1</td>
      <td>0.6</td>
      <td>10.4</td>
      <td>10.3</td>
      <td>0.2</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.062</td>
      <td>-2.6</td>
      <td>-0.7</td>
      <td>-3.2</td>
      <td>-0.1</td>
      <td>25.0</td>
      <td>59.0</td>
      <td>0.424</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>25.0</td>
      <td>58.0</td>
      <td>0.431</td>
      <td>0.424</td>
      <td>13.0</td>
      <td>22.0</td>
      <td>0.591</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>48.0</td>
      <td>19.0</td>
      <td>7.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>34.0</td>
      <td>63.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>GSW</td>
      <td>10.0</td>
      <td>39.0</td>
      <td>3.2</td>
      <td>0.463</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2.8</td>
      <td>20.2</td>
      <td>11.4</td>
      <td>2.9</td>
      <td>1.2</td>
      <td>0.0</td>
      <td>31.6</td>
      <td>6.4</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-0.004</td>
      <td>-6.0</td>
      <td>-0.2</td>
      <td>-6.2</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>0.333</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>0.667</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1981</td>
      <td>Tom Abernethy</td>
      <td>SF</td>
      <td>26.0</td>
      <td>IND</td>
      <td>29.0</td>
      <td>259.0</td>
      <td>8.7</td>
      <td>0.458</td>
      <td>0.018</td>
      <td>0.339</td>
      <td>7.8</td>
      <td>9.1</td>
      <td>8.4</td>
      <td>8.8</td>
      <td>1.1</td>
      <td>0.7</td>
      <td>8.5</td>
      <td>10.9</td>
      <td>0.2</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.072</td>
      <td>-2.0</td>
      <td>-0.8</td>
      <td>-2.8</td>
      <td>-0.1</td>
      <td>24.0</td>
      <td>56.0</td>
      <td>0.429</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>24.0</td>
      <td>55.0</td>
      <td>0.436</td>
      <td>0.429</td>
      <td>11.0</td>
      <td>19.0</td>
      <td>0.579</td>
      <td>19.0</td>
      <td>21.0</td>
      <td>40.0</td>
      <td>18.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>29.0</td>
      <td>59.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1981</td>
      <td>Alvan Adams</td>
      <td>C</td>
      <td>26.0</td>
      <td>PHO</td>
      <td>75.0</td>
      <td>2054.0</td>
      <td>20.3</td>
      <td>0.567</td>
      <td>0.000</td>
      <td>0.298</td>
      <td>8.6</td>
      <td>20.5</td>
      <td>14.7</td>
      <td>24.5</td>
      <td>2.4</td>
      <td>1.9</td>
      <td>18.7</td>
      <td>23.0</td>
      <td>3.3</td>
      <td>4.5</td>
      <td>7.7</td>
      <td>0.180</td>
      <td>2.0</td>
      <td>3.3</td>
      <td>5.3</td>
      <td>3.8</td>
      <td>458.0</td>
      <td>870.0</td>
      <td>0.526</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>458.0</td>
      <td>870.0</td>
      <td>0.526</td>
      <td>0.526</td>
      <td>199.0</td>
      <td>259.0</td>
      <td>0.768</td>
      <td>157.0</td>
      <td>389.0</td>
      <td>546.0</td>
      <td>344.0</td>
      <td>106.0</td>
      <td>69.0</td>
      <td>226.0</td>
      <td>226.0</td>
      <td>1115.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```
data['player_stats'] = data['player_stats'][data['player_stats']['Year'] > 1990]
```


```
data['player_stats'].head(200)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3912</th>
      <td>1991</td>
      <td>Alaa Abdelnaby</td>
      <td>PF</td>
      <td>22.0</td>
      <td>POR</td>
      <td>43.0</td>
      <td>290.0</td>
      <td>13.1</td>
      <td>0.499</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>10.4</td>
      <td>23.4</td>
      <td>17.0</td>
      <td>5.8</td>
      <td>0.7</td>
      <td>2.5</td>
      <td>14.0</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.079</td>
      <td>-4.2</td>
      <td>-0.7</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.474</td>
      <td>25.0</td>
      <td>44.0</td>
      <td>0.568</td>
      <td>27.0</td>
      <td>62.0</td>
      <td>89.0</td>
      <td>12.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>22.0</td>
      <td>39.0</td>
      <td>135.0</td>
      <td>395000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.035221</td>
      <td>0.001346</td>
      <td>0.038203</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3913</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>PG</td>
      <td>21.0</td>
      <td>DEN</td>
      <td>67.0</td>
      <td>1505.0</td>
      <td>12.2</td>
      <td>0.448</td>
      <td>0.099</td>
      <td>0.097</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>3.8</td>
      <td>19.2</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>27.2</td>
      <td>-0.7</td>
      <td>-0.3</td>
      <td>-1.0</td>
      <td>-0.031</td>
      <td>-1.7</td>
      <td>-4.4</td>
      <td>-6.1</td>
      <td>-1.6</td>
      <td>417.0</td>
      <td>1009.0</td>
      <td>0.413</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>0.240</td>
      <td>393.0</td>
      <td>909.0</td>
      <td>0.432</td>
      <td>0.425</td>
      <td>84.0</td>
      <td>98.0</td>
      <td>0.857</td>
      <td>34.0</td>
      <td>87.0</td>
      <td>121.0</td>
      <td>206.0</td>
      <td>55.0</td>
      <td>4.0</td>
      <td>110.0</td>
      <td>149.0</td>
      <td>942.0</td>
      <td>1660000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.160619</td>
      <td>0.005655</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3914</th>
      <td>1991</td>
      <td>Mark Acres</td>
      <td>C</td>
      <td>28.0</td>
      <td>ORL</td>
      <td>68.0</td>
      <td>1313.0</td>
      <td>9.2</td>
      <td>0.551</td>
      <td>0.014</td>
      <td>0.472</td>
      <td>11.3</td>
      <td>18.7</td>
      <td>14.9</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>14.0</td>
      <td>9.3</td>
      <td>1.4</td>
      <td>1.1</td>
      <td>2.5</td>
      <td>0.090</td>
      <td>-2.1</td>
      <td>0.3</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>109.0</td>
      <td>214.0</td>
      <td>0.509</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>108.0</td>
      <td>211.0</td>
      <td>0.512</td>
      <td>0.512</td>
      <td>66.0</td>
      <td>101.0</td>
      <td>0.653</td>
      <td>140.0</td>
      <td>219.0</td>
      <td>359.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>42.0</td>
      <td>218.0</td>
      <td>285.0</td>
      <td>437000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.058019</td>
      <td>0.001489</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3915</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>PG</td>
      <td>28.0</td>
      <td>DEN</td>
      <td>66.0</td>
      <td>2346.0</td>
      <td>22.3</td>
      <td>0.530</td>
      <td>0.397</td>
      <td>0.372</td>
      <td>2.1</td>
      <td>8.8</td>
      <td>5.2</td>
      <td>39.4</td>
      <td>2.6</td>
      <td>0.1</td>
      <td>12.7</td>
      <td>28.5</td>
      <td>5.8</td>
      <td>0.4</td>
      <td>6.3</td>
      <td>0.128</td>
      <td>7.1</td>
      <td>-2.7</td>
      <td>4.4</td>
      <td>3.8</td>
      <td>560.0</td>
      <td>1421.0</td>
      <td>0.394</td>
      <td>167.0</td>
      <td>564.0</td>
      <td>0.296</td>
      <td>393.0</td>
      <td>857.0</td>
      <td>0.459</td>
      <td>0.453</td>
      <td>465.0</td>
      <td>529.0</td>
      <td>0.879</td>
      <td>58.0</td>
      <td>198.0</td>
      <td>256.0</td>
      <td>693.0</td>
      <td>147.0</td>
      <td>6.0</td>
      <td>240.0</td>
      <td>162.0</td>
      <td>1752.0</td>
      <td>825000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.079826</td>
      <td>0.002810</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3916</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>SF</td>
      <td>31.0</td>
      <td>DET</td>
      <td>78.0</td>
      <td>2006.0</td>
      <td>16.7</td>
      <td>0.526</td>
      <td>0.086</td>
      <td>0.349</td>
      <td>7.6</td>
      <td>13.7</td>
      <td>10.7</td>
      <td>11.6</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>10.9</td>
      <td>25.7</td>
      <td>2.8</td>
      <td>2.7</td>
      <td>5.5</td>
      <td>0.132</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>0.7</td>
      <td>1.4</td>
      <td>420.0</td>
      <td>909.0</td>
      <td>0.462</td>
      <td>24.0</td>
      <td>78.0</td>
      <td>0.308</td>
      <td>396.0</td>
      <td>831.0</td>
      <td>0.477</td>
      <td>0.475</td>
      <td>240.0</td>
      <td>317.0</td>
      <td>0.757</td>
      <td>134.0</td>
      <td>240.0</td>
      <td>374.0</td>
      <td>139.0</td>
      <td>47.0</td>
      <td>20.0</td>
      <td>128.0</td>
      <td>209.0</td>
      <td>1104.0</td>
      <td>1115000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.086367</td>
      <td>0.003798</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3917</th>
      <td>1991</td>
      <td>Danny Ainge</td>
      <td>SG</td>
      <td>31.0</td>
      <td>POR</td>
      <td>80.0</td>
      <td>1710.0</td>
      <td>17.0</td>
      <td>0.574</td>
      <td>0.352</td>
      <td>0.193</td>
      <td>2.9</td>
      <td>10.2</td>
      <td>6.6</td>
      <td>23.8</td>
      <td>1.8</td>
      <td>0.5</td>
      <td>11.4</td>
      <td>20.8</td>
      <td>4.1</td>
      <td>2.1</td>
      <td>6.2</td>
      <td>0.175</td>
      <td>3.4</td>
      <td>-0.6</td>
      <td>2.8</td>
      <td>2.1</td>
      <td>337.0</td>
      <td>714.0</td>
      <td>0.472</td>
      <td>102.0</td>
      <td>251.0</td>
      <td>0.406</td>
      <td>235.0</td>
      <td>463.0</td>
      <td>0.508</td>
      <td>0.543</td>
      <td>114.0</td>
      <td>138.0</td>
      <td>0.826</td>
      <td>45.0</td>
      <td>160.0</td>
      <td>205.0</td>
      <td>285.0</td>
      <td>63.0</td>
      <td>13.0</td>
      <td>100.0</td>
      <td>195.0</td>
      <td>890.0</td>
      <td>725000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.064646</td>
      <td>0.002470</td>
      <td>0.038203</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3918</th>
      <td>1991</td>
      <td>Mark Alarie</td>
      <td>PF</td>
      <td>27.0</td>
      <td>WSB</td>
      <td>42.0</td>
      <td>587.0</td>
      <td>11.3</td>
      <td>0.496</td>
      <td>0.093</td>
      <td>0.213</td>
      <td>7.6</td>
      <td>14.2</td>
      <td>10.9</td>
      <td>11.2</td>
      <td>1.3</td>
      <td>0.8</td>
      <td>14.0</td>
      <td>20.3</td>
      <td>0.1</td>
      <td>0.6</td>
      <td>0.6</td>
      <td>0.050</td>
      <td>-2.4</td>
      <td>-1.1</td>
      <td>-3.6</td>
      <td>-0.2</td>
      <td>99.0</td>
      <td>225.0</td>
      <td>0.440</td>
      <td>5.0</td>
      <td>21.0</td>
      <td>0.238</td>
      <td>94.0</td>
      <td>204.0</td>
      <td>0.461</td>
      <td>0.451</td>
      <td>41.0</td>
      <td>48.0</td>
      <td>0.854</td>
      <td>41.0</td>
      <td>76.0</td>
      <td>117.0</td>
      <td>45.0</td>
      <td>15.0</td>
      <td>8.0</td>
      <td>40.0</td>
      <td>88.0</td>
      <td>244.0</td>
      <td>500000.0</td>
      <td>Washington Wizards</td>
      <td>9610000.0</td>
      <td>293563000.0</td>
      <td>0.052029</td>
      <td>0.001703</td>
      <td>0.032736</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3919</th>
      <td>1991</td>
      <td>Steve Alford</td>
      <td>PG</td>
      <td>26.0</td>
      <td>DAL</td>
      <td>34.0</td>
      <td>236.0</td>
      <td>20.5</td>
      <td>0.578</td>
      <td>0.197</td>
      <td>0.265</td>
      <td>4.8</td>
      <td>6.8</td>
      <td>5.8</td>
      <td>16.4</td>
      <td>1.7</td>
      <td>0.3</td>
      <td>10.9</td>
      <td>27.5</td>
      <td>0.5</td>
      <td>0.1</td>
      <td>0.6</td>
      <td>0.121</td>
      <td>2.4</td>
      <td>-3.8</td>
      <td>-1.4</td>
      <td>0.0</td>
      <td>59.0</td>
      <td>117.0</td>
      <td>0.504</td>
      <td>7.0</td>
      <td>23.0</td>
      <td>0.304</td>
      <td>52.0</td>
      <td>94.0</td>
      <td>0.553</td>
      <td>0.534</td>
      <td>26.0</td>
      <td>31.0</td>
      <td>0.839</td>
      <td>10.0</td>
      <td>14.0</td>
      <td>24.0</td>
      <td>22.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>16.0</td>
      <td>11.0</td>
      <td>151.0</td>
      <td>250000.0</td>
      <td>Dallas Mavericks</td>
      <td>11693000.0</td>
      <td>293563000.0</td>
      <td>0.021380</td>
      <td>0.000852</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3920</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>68.0</td>
      <td>924.0</td>
      <td>9.1</td>
      <td>0.455</td>
      <td>0.004</td>
      <td>0.426</td>
      <td>10.0</td>
      <td>25.9</td>
      <td>17.3</td>
      <td>2.1</td>
      <td>1.7</td>
      <td>2.7</td>
      <td>20.8</td>
      <td>16.3</td>
      <td>-1.2</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>-0.017</td>
      <td>-6.8</td>
      <td>-0.8</td>
      <td>-7.5</td>
      <td>-1.3</td>
      <td>116.0</td>
      <td>270.0</td>
      <td>0.430</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>116.0</td>
      <td>269.0</td>
      <td>0.431</td>
      <td>0.430</td>
      <td>60.0</td>
      <td>115.0</td>
      <td>0.522</td>
      <td>97.0</td>
      <td>221.0</td>
      <td>318.0</td>
      <td>16.0</td>
      <td>35.0</td>
      <td>45.0</td>
      <td>84.0</td>
      <td>140.0</td>
      <td>292.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3921</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>MIL</td>
      <td>26.0</td>
      <td>247.0</td>
      <td>7.5</td>
      <td>0.410</td>
      <td>0.014</td>
      <td>0.384</td>
      <td>12.3</td>
      <td>24.1</td>
      <td>18.1</td>
      <td>1.7</td>
      <td>1.6</td>
      <td>2.3</td>
      <td>20.5</td>
      <td>18.6</td>
      <td>-0.5</td>
      <td>0.4</td>
      <td>-0.1</td>
      <td>-0.027</td>
      <td>-7.8</td>
      <td>-1.3</td>
      <td>-9.1</td>
      <td>-0.4</td>
      <td>27.0</td>
      <td>73.0</td>
      <td>0.370</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>27.0</td>
      <td>72.0</td>
      <td>0.375</td>
      <td>0.370</td>
      <td>16.0</td>
      <td>28.0</td>
      <td>0.571</td>
      <td>26.0</td>
      <td>49.0</td>
      <td>75.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>22.0</td>
      <td>29.0</td>
      <td>70.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3922</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>NJN</td>
      <td>1.0</td>
      <td>18.0</td>
      <td>26.4</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>22.4</td>
      <td>12.1</td>
      <td>17.5</td>
      <td>9.1</td>
      <td>5.4</td>
      <td>0.0</td>
      <td>20.0</td>
      <td>11.2</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.300</td>
      <td>8.3</td>
      <td>2.5</td>
      <td>10.9</td>
      <td>0.1</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>8.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3923</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>DEN</td>
      <td>41.0</td>
      <td>659.0</td>
      <td>9.2</td>
      <td>0.463</td>
      <td>0.000</td>
      <td>0.451</td>
      <td>8.7</td>
      <td>27.0</td>
      <td>17.0</td>
      <td>2.1</td>
      <td>1.6</td>
      <td>2.9</td>
      <td>20.9</td>
      <td>15.6</td>
      <td>-0.8</td>
      <td>0.5</td>
      <td>-0.3</td>
      <td>-0.022</td>
      <td>-6.8</td>
      <td>-0.6</td>
      <td>-7.5</td>
      <td>-0.9</td>
      <td>85.0</td>
      <td>193.0</td>
      <td>0.440</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>85.0</td>
      <td>193.0</td>
      <td>0.440</td>
      <td>0.440</td>
      <td>44.0</td>
      <td>87.0</td>
      <td>0.506</td>
      <td>67.0</td>
      <td>170.0</td>
      <td>237.0</td>
      <td>12.0</td>
      <td>25.0</td>
      <td>36.0</td>
      <td>61.0</td>
      <td>107.0</td>
      <td>214.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3924</th>
      <td>1991</td>
      <td>Nick Anderson</td>
      <td>SG</td>
      <td>23.0</td>
      <td>ORL</td>
      <td>70.0</td>
      <td>1971.0</td>
      <td>15.1</td>
      <td>0.510</td>
      <td>0.068</td>
      <td>0.302</td>
      <td>4.9</td>
      <td>16.7</td>
      <td>10.7</td>
      <td>8.5</td>
      <td>1.8</td>
      <td>1.3</td>
      <td>10.4</td>
      <td>22.4</td>
      <td>1.2</td>
      <td>1.9</td>
      <td>3.1</td>
      <td>0.075</td>
      <td>-1.1</td>
      <td>0.1</td>
      <td>-1.0</td>
      <td>0.5</td>
      <td>400.0</td>
      <td>857.0</td>
      <td>0.467</td>
      <td>17.0</td>
      <td>58.0</td>
      <td>0.293</td>
      <td>383.0</td>
      <td>799.0</td>
      <td>0.479</td>
      <td>0.477</td>
      <td>173.0</td>
      <td>259.0</td>
      <td>0.668</td>
      <td>92.0</td>
      <td>294.0</td>
      <td>386.0</td>
      <td>106.0</td>
      <td>74.0</td>
      <td>44.0</td>
      <td>113.0</td>
      <td>145.0</td>
      <td>990.0</td>
      <td>725000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.096256</td>
      <td>0.002470</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3925</th>
      <td>1991</td>
      <td>Ron Anderson</td>
      <td>SF</td>
      <td>32.0</td>
      <td>PHI</td>
      <td>82.0</td>
      <td>2340.0</td>
      <td>15.5</td>
      <td>0.524</td>
      <td>0.041</td>
      <td>0.188</td>
      <td>5.0</td>
      <td>12.4</td>
      <td>8.8</td>
      <td>8.2</td>
      <td>1.4</td>
      <td>0.3</td>
      <td>8.1</td>
      <td>23.2</td>
      <td>2.3</td>
      <td>1.8</td>
      <td>4.1</td>
      <td>0.085</td>
      <td>-0.5</td>
      <td>-1.8</td>
      <td>-2.3</td>
      <td>-0.2</td>
      <td>512.0</td>
      <td>1055.0</td>
      <td>0.485</td>
      <td>9.0</td>
      <td>43.0</td>
      <td>0.209</td>
      <td>503.0</td>
      <td>1012.0</td>
      <td>0.497</td>
      <td>0.490</td>
      <td>165.0</td>
      <td>198.0</td>
      <td>0.833</td>
      <td>103.0</td>
      <td>264.0</td>
      <td>367.0</td>
      <td>115.0</td>
      <td>65.0</td>
      <td>13.0</td>
      <td>100.0</td>
      <td>163.0</td>
      <td>1198.0</td>
      <td>425000.0</td>
      <td>Philadelphia 76ers</td>
      <td>11640000.0</td>
      <td>293563000.0</td>
      <td>0.036512</td>
      <td>0.001448</td>
      <td>0.039651</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>3926</th>
      <td>1991</td>
      <td>Willie Anderson</td>
      <td>SG</td>
      <td>24.0</td>
      <td>SAS</td>
      <td>75.0</td>
      <td>2592.0</td>
      <td>13.0</td>
      <td>0.499</td>
      <td>0.035</td>
      <td>0.215</td>
      <td>3.1</td>
      <td>11.5</td>
      <td>7.5</td>
      <td>20.2</td>
      <td>1.5</td>
      <td>1.1</td>
      <td>13.3</td>
      <td>20.1</td>
      <td>1.3</td>
      <td>3.5</td>
      <td>4.8</td>
      <td>0.089</td>
      <td>-0.8</td>
      <td>0.9</td>
      <td>0.1</td>
      <td>1.4</td>
      <td>453.0</td>
      <td>991.0</td>
      <td>0.457</td>
      <td>7.0</td>
      <td>35.0</td>
      <td>0.200</td>
      <td>446.0</td>
      <td>956.0</td>
      <td>0.467</td>
      <td>0.461</td>
      <td>170.0</td>
      <td>213.0</td>
      <td>0.798</td>
      <td>68.0</td>
      <td>283.0</td>
      <td>351.0</td>
      <td>358.0</td>
      <td>79.0</td>
      <td>46.0</td>
      <td>167.0</td>
      <td>226.0</td>
      <td>1083.0</td>
      <td>725000.0</td>
      <td>San Antonio Spurs</td>
      <td>11057000.0</td>
      <td>293563000.0</td>
      <td>0.065569</td>
      <td>0.002470</td>
      <td>0.037665</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3927</th>
      <td>1991</td>
      <td>Michael Ansley</td>
      <td>SF</td>
      <td>23.0</td>
      <td>ORL</td>
      <td>67.0</td>
      <td>877.0</td>
      <td>16.9</td>
      <td>0.594</td>
      <td>0.000</td>
      <td>0.483</td>
      <td>14.7</td>
      <td>16.8</td>
      <td>15.7</td>
      <td>4.3</td>
      <td>1.5</td>
      <td>0.5</td>
      <td>9.1</td>
      <td>16.3</td>
      <td>2.3</td>
      <td>0.7</td>
      <td>3.1</td>
      <td>0.168</td>
      <td>1.2</td>
      <td>-1.2</td>
      <td>0.0</td>
      <td>0.4</td>
      <td>144.0</td>
      <td>263.0</td>
      <td>0.548</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>144.0</td>
      <td>263.0</td>
      <td>0.548</td>
      <td>0.548</td>
      <td>91.0</td>
      <td>127.0</td>
      <td>0.717</td>
      <td>122.0</td>
      <td>131.0</td>
      <td>253.0</td>
      <td>25.0</td>
      <td>27.0</td>
      <td>7.0</td>
      <td>32.0</td>
      <td>125.0</td>
      <td>379.0</td>
      <td>200000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.026553</td>
      <td>0.000681</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3928</th>
      <td>1991</td>
      <td>B.J. Armstrong</td>
      <td>PG</td>
      <td>23.0</td>
      <td>CHI</td>
      <td>82.0</td>
      <td>1731.0</td>
      <td>14.8</td>
      <td>0.529</td>
      <td>0.047</td>
      <td>0.176</td>
      <td>1.7</td>
      <td>8.3</td>
      <td>5.1</td>
      <td>23.4</td>
      <td>2.0</td>
      <td>0.1</td>
      <td>13.6</td>
      <td>19.5</td>
      <td>2.3</td>
      <td>1.8</td>
      <td>4.1</td>
      <td>0.114</td>
      <td>-0.4</td>
      <td>-1.4</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>304.0</td>
      <td>632.0</td>
      <td>0.481</td>
      <td>15.0</td>
      <td>30.0</td>
      <td>0.500</td>
      <td>289.0</td>
      <td>602.0</td>
      <td>0.480</td>
      <td>0.493</td>
      <td>97.0</td>
      <td>111.0</td>
      <td>0.874</td>
      <td>25.0</td>
      <td>124.0</td>
      <td>149.0</td>
      <td>301.0</td>
      <td>70.0</td>
      <td>4.0</td>
      <td>107.0</td>
      <td>118.0</td>
      <td>720.0</td>
      <td>425000.0</td>
      <td>Chicago Bulls</td>
      <td>10040000.0</td>
      <td>293563000.0</td>
      <td>0.042331</td>
      <td>0.001448</td>
      <td>0.034200</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3929</th>
      <td>1991</td>
      <td>Vincent Askew</td>
      <td>SG</td>
      <td>24.0</td>
      <td>GSW</td>
      <td>7.0</td>
      <td>85.0</td>
      <td>10.9</td>
      <td>0.553</td>
      <td>0.000</td>
      <td>0.440</td>
      <td>9.1</td>
      <td>5.2</td>
      <td>7.1</td>
      <td>20.1</td>
      <td>1.1</td>
      <td>0.0</td>
      <td>16.7</td>
      <td>16.8</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.2</td>
      <td>0.093</td>
      <td>0.1</td>
      <td>-2.7</td>
      <td>-2.6</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>25.0</td>
      <td>0.480</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>12.0</td>
      <td>25.0</td>
      <td>0.480</td>
      <td>0.480</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>0.818</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>21.0</td>
      <td>33.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3930</th>
      <td>1991</td>
      <td>Keith Askins</td>
      <td>SF</td>
      <td>23.0</td>
      <td>MIA</td>
      <td>39.0</td>
      <td>266.0</td>
      <td>13.5</td>
      <td>0.467</td>
      <td>0.309</td>
      <td>0.309</td>
      <td>12.4</td>
      <td>16.2</td>
      <td>14.3</td>
      <td>10.2</td>
      <td>2.9</td>
      <td>3.0</td>
      <td>10.7</td>
      <td>15.8</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>0.103</td>
      <td>-0.6</td>
      <td>2.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>34.0</td>
      <td>81.0</td>
      <td>0.420</td>
      <td>6.0</td>
      <td>25.0</td>
      <td>0.240</td>
      <td>28.0</td>
      <td>56.0</td>
      <td>0.500</td>
      <td>0.457</td>
      <td>12.0</td>
      <td>25.0</td>
      <td>0.480</td>
      <td>30.0</td>
      <td>38.0</td>
      <td>68.0</td>
      <td>19.0</td>
      <td>16.0</td>
      <td>13.0</td>
      <td>11.0</td>
      <td>46.0</td>
      <td>86.0</td>
      <td>150000.0</td>
      <td>Miami Heat</td>
      <td>8510000.0</td>
      <td>293563000.0</td>
      <td>0.017626</td>
      <td>0.000511</td>
      <td>0.028989</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3931</th>
      <td>1991</td>
      <td>Milos Babic</td>
      <td>PF</td>
      <td>22.0</td>
      <td>CLE</td>
      <td>12.0</td>
      <td>52.0</td>
      <td>6.0</td>
      <td>0.391</td>
      <td>0.000</td>
      <td>0.632</td>
      <td>13.4</td>
      <td>6.7</td>
      <td>10.0</td>
      <td>10.9</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>17.1</td>
      <td>24.6</td>
      <td>-0.1</td>
      <td>0.0</td>
      <td>-0.1</td>
      <td>-0.085</td>
      <td>-6.0</td>
      <td>-2.6</td>
      <td>-8.6</td>
      <td>-0.1</td>
      <td>6.0</td>
      <td>19.0</td>
      <td>0.316</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>6.0</td>
      <td>19.0</td>
      <td>0.316</td>
      <td>0.316</td>
      <td>7.0</td>
      <td>12.0</td>
      <td>0.583</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>9.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>19.0</td>
      <td>200000.0</td>
      <td>Cleveland Caveliers</td>
      <td>14403000.0</td>
      <td>293563000.0</td>
      <td>0.013886</td>
      <td>0.000681</td>
      <td>0.049063</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3932</th>
      <td>1991</td>
      <td>Thurl Bailey</td>
      <td>PF</td>
      <td>29.0</td>
      <td>UTA</td>
      <td>82.0</td>
      <td>2486.0</td>
      <td>12.5</td>
      <td>0.513</td>
      <td>0.003</td>
      <td>0.311</td>
      <td>5.1</td>
      <td>13.6</td>
      <td>9.6</td>
      <td>7.7</td>
      <td>1.1</td>
      <td>2.3</td>
      <td>11.6</td>
      <td>20.0</td>
      <td>0.6</td>
      <td>3.1</td>
      <td>3.7</td>
      <td>0.072</td>
      <td>-2.4</td>
      <td>1.2</td>
      <td>-1.2</td>
      <td>0.5</td>
      <td>399.0</td>
      <td>872.0</td>
      <td>0.458</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.000</td>
      <td>399.0</td>
      <td>869.0</td>
      <td>0.459</td>
      <td>0.458</td>
      <td>219.0</td>
      <td>271.0</td>
      <td>0.808</td>
      <td>101.0</td>
      <td>306.0</td>
      <td>407.0</td>
      <td>124.0</td>
      <td>53.0</td>
      <td>91.0</td>
      <td>130.0</td>
      <td>160.0</td>
      <td>1017.0</td>
      <td>1000000.0</td>
      <td>Utah Jazz</td>
      <td>10695000.0</td>
      <td>293563000.0</td>
      <td>0.093502</td>
      <td>0.003406</td>
      <td>0.036432</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3933</th>
      <td>1991</td>
      <td>Cedric Ball</td>
      <td>PF</td>
      <td>22.0</td>
      <td>LAC</td>
      <td>7.0</td>
      <td>26.0</td>
      <td>10.8</td>
      <td>0.450</td>
      <td>0.000</td>
      <td>0.250</td>
      <td>20.4</td>
      <td>25.3</td>
      <td>22.8</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.6</td>
      <td>18.4</td>
      <td>17.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.054</td>
      <td>-7.4</td>
      <td>-4.3</td>
      <td>-11.7</td>
      <td>-0.1</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>0.375</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>0.375</td>
      <td>0.375</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1.000</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>11.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3934</th>
      <td>1991</td>
      <td>Ken Bannister</td>
      <td>PF</td>
      <td>30.0</td>
      <td>LAC</td>
      <td>47.0</td>
      <td>339.0</td>
      <td>7.8</td>
      <td>0.506</td>
      <td>0.012</td>
      <td>0.802</td>
      <td>10.6</td>
      <td>20.0</td>
      <td>15.3</td>
      <td>3.6</td>
      <td>0.7</td>
      <td>1.2</td>
      <td>18.6</td>
      <td>16.1</td>
      <td>-0.2</td>
      <td>0.4</td>
      <td>0.2</td>
      <td>0.028</td>
      <td>-5.0</td>
      <td>-1.2</td>
      <td>-6.3</td>
      <td>-0.4</td>
      <td>43.0</td>
      <td>81.0</td>
      <td>0.531</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>43.0</td>
      <td>80.0</td>
      <td>0.538</td>
      <td>0.531</td>
      <td>25.0</td>
      <td>65.0</td>
      <td>0.385</td>
      <td>34.0</td>
      <td>62.0</td>
      <td>96.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>25.0</td>
      <td>73.0</td>
      <td>111.0</td>
      <td>400000.0</td>
      <td>Los Angeles Clippers</td>
      <td>10245000.0</td>
      <td>293563000.0</td>
      <td>0.039043</td>
      <td>0.001363</td>
      <td>0.034899</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3935</th>
      <td>1991</td>
      <td>Ken Bannister</td>
      <td>PF</td>
      <td>30.0</td>
      <td>LAC</td>
      <td>47.0</td>
      <td>339.0</td>
      <td>7.8</td>
      <td>0.506</td>
      <td>0.012</td>
      <td>0.802</td>
      <td>10.6</td>
      <td>20.0</td>
      <td>15.3</td>
      <td>3.6</td>
      <td>0.7</td>
      <td>1.2</td>
      <td>18.6</td>
      <td>16.1</td>
      <td>-0.2</td>
      <td>0.4</td>
      <td>0.2</td>
      <td>0.028</td>
      <td>-5.0</td>
      <td>-1.2</td>
      <td>-6.3</td>
      <td>-0.4</td>
      <td>43.0</td>
      <td>81.0</td>
      <td>0.531</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>43.0</td>
      <td>80.0</td>
      <td>0.538</td>
      <td>0.531</td>
      <td>25.0</td>
      <td>65.0</td>
      <td>0.385</td>
      <td>34.0</td>
      <td>62.0</td>
      <td>96.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>25.0</td>
      <td>73.0</td>
      <td>111.0</td>
      <td>50000.0</td>
      <td>Utah Jazz</td>
      <td>10695000.0</td>
      <td>293563000.0</td>
      <td>0.004675</td>
      <td>0.000170</td>
      <td>0.036432</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3936</th>
      <td>1991</td>
      <td>Charles Barkley*</td>
      <td>SF</td>
      <td>27.0</td>
      <td>PHI</td>
      <td>67.0</td>
      <td>2498.0</td>
      <td>28.9</td>
      <td>0.635</td>
      <td>0.133</td>
      <td>0.564</td>
      <td>11.8</td>
      <td>18.6</td>
      <td>15.3</td>
      <td>20.6</td>
      <td>2.2</td>
      <td>0.8</td>
      <td>12.6</td>
      <td>29.1</td>
      <td>10.3</td>
      <td>3.1</td>
      <td>13.4</td>
      <td>0.258</td>
      <td>9.0</td>
      <td>0.9</td>
      <td>9.9</td>
      <td>7.4</td>
      <td>665.0</td>
      <td>1167.0</td>
      <td>0.570</td>
      <td>44.0</td>
      <td>155.0</td>
      <td>0.284</td>
      <td>621.0</td>
      <td>1012.0</td>
      <td>0.614</td>
      <td>0.589</td>
      <td>475.0</td>
      <td>658.0</td>
      <td>0.722</td>
      <td>258.0</td>
      <td>422.0</td>
      <td>680.0</td>
      <td>284.0</td>
      <td>110.0</td>
      <td>33.0</td>
      <td>210.0</td>
      <td>173.0</td>
      <td>1849.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3937</th>
      <td>1991</td>
      <td>Dana Barros</td>
      <td>PG</td>
      <td>23.0</td>
      <td>SEA</td>
      <td>66.0</td>
      <td>750.0</td>
      <td>18.8</td>
      <td>0.600</td>
      <td>0.260</td>
      <td>0.273</td>
      <td>2.7</td>
      <td>8.7</td>
      <td>5.7</td>
      <td>21.9</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>13.4</td>
      <td>22.5</td>
      <td>2.1</td>
      <td>0.4</td>
      <td>2.5</td>
      <td>0.158</td>
      <td>3.0</td>
      <td>-3.2</td>
      <td>-0.2</td>
      <td>0.3</td>
      <td>154.0</td>
      <td>311.0</td>
      <td>0.495</td>
      <td>32.0</td>
      <td>81.0</td>
      <td>0.395</td>
      <td>122.0</td>
      <td>230.0</td>
      <td>0.530</td>
      <td>0.547</td>
      <td>78.0</td>
      <td>85.0</td>
      <td>0.918</td>
      <td>17.0</td>
      <td>54.0</td>
      <td>71.0</td>
      <td>111.0</td>
      <td>23.0</td>
      <td>1.0</td>
      <td>54.0</td>
      <td>40.0</td>
      <td>418.0</td>
      <td>500000.0</td>
      <td>Oklahoma City Thunder</td>
      <td>10590000.0</td>
      <td>293563000.0</td>
      <td>0.047214</td>
      <td>0.001703</td>
      <td>0.036074</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3938</th>
      <td>1991</td>
      <td>John Battle</td>
      <td>SG</td>
      <td>28.0</td>
      <td>ATL</td>
      <td>79.0</td>
      <td>1863.0</td>
      <td>16.4</td>
      <td>0.538</td>
      <td>0.057</td>
      <td>0.367</td>
      <td>1.9</td>
      <td>7.6</td>
      <td>4.7</td>
      <td>18.4</td>
      <td>1.2</td>
      <td>0.2</td>
      <td>10.1</td>
      <td>24.7</td>
      <td>3.4</td>
      <td>0.6</td>
      <td>4.0</td>
      <td>0.102</td>
      <td>0.7</td>
      <td>-3.1</td>
      <td>-2.4</td>
      <td>-0.2</td>
      <td>397.0</td>
      <td>862.0</td>
      <td>0.461</td>
      <td>14.0</td>
      <td>49.0</td>
      <td>0.286</td>
      <td>383.0</td>
      <td>813.0</td>
      <td>0.471</td>
      <td>0.469</td>
      <td>270.0</td>
      <td>316.0</td>
      <td>0.854</td>
      <td>34.0</td>
      <td>125.0</td>
      <td>159.0</td>
      <td>217.0</td>
      <td>45.0</td>
      <td>6.0</td>
      <td>113.0</td>
      <td>145.0</td>
      <td>1078.0</td>
      <td>590000.0</td>
      <td>Atlanta Hawks</td>
      <td>11761000.0</td>
      <td>293563000.0</td>
      <td>0.050166</td>
      <td>0.002010</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3939</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>SG</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>56.0</td>
      <td>945.0</td>
      <td>12.4</td>
      <td>0.525</td>
      <td>0.085</td>
      <td>0.330</td>
      <td>8.2</td>
      <td>10.3</td>
      <td>9.2</td>
      <td>7.9</td>
      <td>2.8</td>
      <td>1.0</td>
      <td>14.1</td>
      <td>14.7</td>
      <td>0.7</td>
      <td>0.6</td>
      <td>1.3</td>
      <td>0.068</td>
      <td>-1.1</td>
      <td>0.4</td>
      <td>-0.7</td>
      <td>0.3</td>
      <td>133.0</td>
      <td>282.0</td>
      <td>0.472</td>
      <td>3.0</td>
      <td>24.0</td>
      <td>0.125</td>
      <td>130.0</td>
      <td>258.0</td>
      <td>0.504</td>
      <td>0.477</td>
      <td>70.0</td>
      <td>93.0</td>
      <td>0.753</td>
      <td>83.0</td>
      <td>93.0</td>
      <td>176.0</td>
      <td>62.0</td>
      <td>60.0</td>
      <td>18.0</td>
      <td>53.0</td>
      <td>108.0</td>
      <td>339.0</td>
      <td>350000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3940</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>SG</td>
      <td>26.0</td>
      <td>PHO</td>
      <td>16.0</td>
      <td>263.0</td>
      <td>12.8</td>
      <td>0.486</td>
      <td>0.023</td>
      <td>0.337</td>
      <td>9.2</td>
      <td>12.7</td>
      <td>11.0</td>
      <td>7.5</td>
      <td>3.4</td>
      <td>1.3</td>
      <td>14.7</td>
      <td>17.9</td>
      <td>0.0</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.081</td>
      <td>-1.7</td>
      <td>1.8</td>
      <td>0.2</td>
      <td>0.1</td>
      <td>38.0</td>
      <td>86.0</td>
      <td>0.442</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>38.0</td>
      <td>84.0</td>
      <td>0.452</td>
      <td>0.442</td>
      <td>20.0</td>
      <td>29.0</td>
      <td>0.690</td>
      <td>21.0</td>
      <td>32.0</td>
      <td>53.0</td>
      <td>15.0</td>
      <td>19.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>25.0</td>
      <td>96.0</td>
      <td>350000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3941</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>SG</td>
      <td>26.0</td>
      <td>DEN</td>
      <td>40.0</td>
      <td>682.0</td>
      <td>12.3</td>
      <td>0.542</td>
      <td>0.112</td>
      <td>0.327</td>
      <td>7.8</td>
      <td>9.4</td>
      <td>8.5</td>
      <td>8.1</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>13.8</td>
      <td>13.4</td>
      <td>0.7</td>
      <td>0.2</td>
      <td>0.9</td>
      <td>0.063</td>
      <td>-0.8</td>
      <td>-0.2</td>
      <td>-1.0</td>
      <td>0.2</td>
      <td>95.0</td>
      <td>196.0</td>
      <td>0.485</td>
      <td>3.0</td>
      <td>22.0</td>
      <td>0.136</td>
      <td>92.0</td>
      <td>174.0</td>
      <td>0.529</td>
      <td>0.492</td>
      <td>50.0</td>
      <td>64.0</td>
      <td>0.781</td>
      <td>62.0</td>
      <td>61.0</td>
      <td>123.0</td>
      <td>47.0</td>
      <td>41.0</td>
      <td>12.0</td>
      <td>36.0</td>
      <td>83.0</td>
      <td>243.0</td>
      <td>350000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4082</th>
      <td>1991</td>
      <td>Derek Harper</td>
      <td>PG</td>
      <td>29.0</td>
      <td>DAL</td>
      <td>77.0</td>
      <td>2879.0</td>
      <td>19.4</td>
      <td>0.543</td>
      <td>0.201</td>
      <td>0.319</td>
      <td>2.3</td>
      <td>6.9</td>
      <td>4.6</td>
      <td>30.7</td>
      <td>2.6</td>
      <td>0.3</td>
      <td>11.2</td>
      <td>24.2</td>
      <td>5.7</td>
      <td>2.0</td>
      <td>7.6</td>
      <td>0.127</td>
      <td>4.1</td>
      <td>-1.3</td>
      <td>2.8</td>
      <td>3.5</td>
      <td>572.0</td>
      <td>1226.0</td>
      <td>0.467</td>
      <td>89.0</td>
      <td>246.0</td>
      <td>0.362</td>
      <td>483.0</td>
      <td>980.0</td>
      <td>0.493</td>
      <td>0.503</td>
      <td>286.0</td>
      <td>391.0</td>
      <td>0.731</td>
      <td>59.0</td>
      <td>174.0</td>
      <td>233.0</td>
      <td>548.0</td>
      <td>147.0</td>
      <td>14.0</td>
      <td>177.0</td>
      <td>222.0</td>
      <td>1519.0</td>
      <td>1519000.0</td>
      <td>Dallas Mavericks</td>
      <td>11693000.0</td>
      <td>293563000.0</td>
      <td>0.129907</td>
      <td>0.005174</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4083</th>
      <td>1991</td>
      <td>Ron Harper</td>
      <td>SG</td>
      <td>27.0</td>
      <td>LAC</td>
      <td>39.0</td>
      <td>1383.0</td>
      <td>14.5</td>
      <td>0.463</td>
      <td>0.203</td>
      <td>0.298</td>
      <td>4.4</td>
      <td>10.3</td>
      <td>7.3</td>
      <td>23.2</td>
      <td>2.3</td>
      <td>1.5</td>
      <td>13.5</td>
      <td>28.0</td>
      <td>-0.8</td>
      <td>1.6</td>
      <td>0.9</td>
      <td>0.030</td>
      <td>0.3</td>
      <td>0.4</td>
      <td>0.7</td>
      <td>1.0</td>
      <td>285.0</td>
      <td>729.0</td>
      <td>0.391</td>
      <td>48.0</td>
      <td>148.0</td>
      <td>0.324</td>
      <td>237.0</td>
      <td>581.0</td>
      <td>0.408</td>
      <td>0.424</td>
      <td>145.0</td>
      <td>217.0</td>
      <td>0.668</td>
      <td>58.0</td>
      <td>130.0</td>
      <td>188.0</td>
      <td>209.0</td>
      <td>66.0</td>
      <td>35.0</td>
      <td>129.0</td>
      <td>111.0</td>
      <td>763.0</td>
      <td>1365000.0</td>
      <td>Los Angeles Clippers</td>
      <td>10245000.0</td>
      <td>293563000.0</td>
      <td>0.133236</td>
      <td>0.004650</td>
      <td>0.034899</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4084</th>
      <td>1991</td>
      <td>Tony Harris</td>
      <td>SG</td>
      <td>23.0</td>
      <td>PHI</td>
      <td>6.0</td>
      <td>41.0</td>
      <td>-5.0</td>
      <td>0.282</td>
      <td>0.125</td>
      <td>0.250</td>
      <td>0.0</td>
      <td>2.7</td>
      <td>1.4</td>
      <td>0.0</td>
      <td>1.2</td>
      <td>0.0</td>
      <td>14.5</td>
      <td>22.1</td>
      <td>-0.2</td>
      <td>0.0</td>
      <td>-0.2</td>
      <td>-0.248</td>
      <td>-12.5</td>
      <td>-4.8</td>
      <td>-17.4</td>
      <td>-0.2</td>
      <td>4.0</td>
      <td>16.0</td>
      <td>0.250</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>4.0</td>
      <td>14.0</td>
      <td>0.286</td>
      <td>0.250</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>0.500</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>5.0</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4085</th>
      <td>1991</td>
      <td>Scott Hastings</td>
      <td>PF</td>
      <td>30.0</td>
      <td>DET</td>
      <td>27.0</td>
      <td>113.0</td>
      <td>15.8</td>
      <td>0.712</td>
      <td>0.143</td>
      <td>0.464</td>
      <td>14.1</td>
      <td>14.2</td>
      <td>14.2</td>
      <td>9.3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>17.2</td>
      <td>15.8</td>
      <td>0.4</td>
      <td>0.1</td>
      <td>0.5</td>
      <td>0.216</td>
      <td>2.6</td>
      <td>-1.3</td>
      <td>1.3</td>
      <td>0.1</td>
      <td>16.0</td>
      <td>28.0</td>
      <td>0.571</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>0.750</td>
      <td>13.0</td>
      <td>24.0</td>
      <td>0.542</td>
      <td>0.625</td>
      <td>13.0</td>
      <td>13.0</td>
      <td>1.000</td>
      <td>14.0</td>
      <td>14.0</td>
      <td>28.0</td>
      <td>7.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.0</td>
      <td>23.0</td>
      <td>48.0</td>
      <td>450000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.034857</td>
      <td>0.001533</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4086</th>
      <td>1991</td>
      <td>Hersey Hawkins</td>
      <td>SG</td>
      <td>24.0</td>
      <td>PHI</td>
      <td>80.0</td>
      <td>3110.0</td>
      <td>19.4</td>
      <td>0.592</td>
      <td>0.216</td>
      <td>0.440</td>
      <td>1.8</td>
      <td>9.3</td>
      <td>5.6</td>
      <td>15.3</td>
      <td>2.9</td>
      <td>0.7</td>
      <td>12.5</td>
      <td>24.0</td>
      <td>6.5</td>
      <td>3.3</td>
      <td>9.8</td>
      <td>0.151</td>
      <td>3.4</td>
      <td>0.0</td>
      <td>3.5</td>
      <td>4.3</td>
      <td>590.0</td>
      <td>1251.0</td>
      <td>0.472</td>
      <td>108.0</td>
      <td>270.0</td>
      <td>0.400</td>
      <td>482.0</td>
      <td>981.0</td>
      <td>0.491</td>
      <td>0.515</td>
      <td>479.0</td>
      <td>550.0</td>
      <td>0.871</td>
      <td>48.0</td>
      <td>262.0</td>
      <td>310.0</td>
      <td>299.0</td>
      <td>178.0</td>
      <td>39.0</td>
      <td>213.0</td>
      <td>182.0</td>
      <td>1767.0</td>
      <td>1000000.0</td>
      <td>Philadelphia 76ers</td>
      <td>11640000.0</td>
      <td>293563000.0</td>
      <td>0.085911</td>
      <td>0.003406</td>
      <td>0.039651</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4087</th>
      <td>1991</td>
      <td>Gerald Henderson</td>
      <td>PG</td>
      <td>35.0</td>
      <td>DET</td>
      <td>23.0</td>
      <td>392.0</td>
      <td>10.1</td>
      <td>0.487</td>
      <td>0.179</td>
      <td>0.179</td>
      <td>2.3</td>
      <td>8.5</td>
      <td>5.4</td>
      <td>23.3</td>
      <td>1.6</td>
      <td>0.3</td>
      <td>18.2</td>
      <td>17.3</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.061</td>
      <td>-2.1</td>
      <td>-0.3</td>
      <td>-2.4</td>
      <td>0.0</td>
      <td>50.0</td>
      <td>117.0</td>
      <td>0.427</td>
      <td>7.0</td>
      <td>21.0</td>
      <td>0.333</td>
      <td>43.0</td>
      <td>96.0</td>
      <td>0.448</td>
      <td>0.457</td>
      <td>16.0</td>
      <td>21.0</td>
      <td>0.762</td>
      <td>8.0</td>
      <td>29.0</td>
      <td>37.0</td>
      <td>62.0</td>
      <td>12.0</td>
      <td>2.0</td>
      <td>28.0</td>
      <td>43.0</td>
      <td>123.0</td>
      <td>125000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.009682</td>
      <td>0.000426</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4088</th>
      <td>1991</td>
      <td>Steve Henson</td>
      <td>PG</td>
      <td>22.0</td>
      <td>MIL</td>
      <td>68.0</td>
      <td>690.0</td>
      <td>11.5</td>
      <td>0.516</td>
      <td>0.286</td>
      <td>0.222</td>
      <td>2.4</td>
      <td>6.5</td>
      <td>4.4</td>
      <td>26.2</td>
      <td>2.3</td>
      <td>0.0</td>
      <td>17.2</td>
      <td>15.6</td>
      <td>0.7</td>
      <td>0.6</td>
      <td>1.2</td>
      <td>0.085</td>
      <td>-1.1</td>
      <td>-2.2</td>
      <td>-3.4</td>
      <td>-0.2</td>
      <td>79.0</td>
      <td>189.0</td>
      <td>0.418</td>
      <td>18.0</td>
      <td>54.0</td>
      <td>0.333</td>
      <td>61.0</td>
      <td>135.0</td>
      <td>0.452</td>
      <td>0.466</td>
      <td>38.0</td>
      <td>42.0</td>
      <td>0.905</td>
      <td>14.0</td>
      <td>37.0</td>
      <td>51.0</td>
      <td>131.0</td>
      <td>32.0</td>
      <td>0.0</td>
      <td>43.0</td>
      <td>83.0</td>
      <td>214.0</td>
      <td>120000.0</td>
      <td>Milwaukee Bucks</td>
      <td>11595000.0</td>
      <td>293563000.0</td>
      <td>0.010349</td>
      <td>0.000409</td>
      <td>0.039497</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4089</th>
      <td>1991</td>
      <td>Mike Higgins</td>
      <td>SF</td>
      <td>23.0</td>
      <td>SAC</td>
      <td>7.0</td>
      <td>61.0</td>
      <td>4.1</td>
      <td>0.612</td>
      <td>0.000</td>
      <td>0.700</td>
      <td>7.2</td>
      <td>1.9</td>
      <td>4.6</td>
      <td>4.8</td>
      <td>0.0</td>
      <td>2.1</td>
      <td>23.4</td>
      <td>12.2</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.017</td>
      <td>-4.1</td>
      <td>-1.7</td>
      <td>-5.8</td>
      <td>-0.1</td>
      <td>6.0</td>
      <td>10.0</td>
      <td>0.600</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>6.0</td>
      <td>10.0</td>
      <td>0.600</td>
      <td>0.600</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>0.571</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4090</th>
      <td>1991</td>
      <td>Rod Higgins</td>
      <td>SF</td>
      <td>31.0</td>
      <td>GSW</td>
      <td>82.0</td>
      <td>2024.0</td>
      <td>13.4</td>
      <td>0.589</td>
      <td>0.394</td>
      <td>0.404</td>
      <td>5.9</td>
      <td>13.3</td>
      <td>9.6</td>
      <td>7.2</td>
      <td>1.2</td>
      <td>1.1</td>
      <td>9.0</td>
      <td>14.3</td>
      <td>4.2</td>
      <td>1.2</td>
      <td>5.3</td>
      <td>0.127</td>
      <td>1.7</td>
      <td>-0.7</td>
      <td>0.9</td>
      <td>1.5</td>
      <td>259.0</td>
      <td>559.0</td>
      <td>0.463</td>
      <td>73.0</td>
      <td>220.0</td>
      <td>0.332</td>
      <td>186.0</td>
      <td>339.0</td>
      <td>0.549</td>
      <td>0.529</td>
      <td>185.0</td>
      <td>226.0</td>
      <td>0.819</td>
      <td>109.0</td>
      <td>245.0</td>
      <td>354.0</td>
      <td>113.0</td>
      <td>52.0</td>
      <td>37.0</td>
      <td>65.0</td>
      <td>198.0</td>
      <td>776.0</td>
      <td>500000.0</td>
      <td>Golden State Warriors</td>
      <td>11150000.0</td>
      <td>293563000.0</td>
      <td>0.044843</td>
      <td>0.001703</td>
      <td>0.037982</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4091</th>
      <td>1991</td>
      <td>Sean Higgins</td>
      <td>SF</td>
      <td>22.0</td>
      <td>SAS</td>
      <td>50.0</td>
      <td>464.0</td>
      <td>9.1</td>
      <td>0.497</td>
      <td>0.090</td>
      <td>0.156</td>
      <td>4.5</td>
      <td>10.2</td>
      <td>7.5</td>
      <td>11.6</td>
      <td>0.8</td>
      <td>0.1</td>
      <td>17.8</td>
      <td>24.8</td>
      <td>-0.5</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>-4.3</td>
      <td>-2.5</td>
      <td>-6.8</td>
      <td>-0.6</td>
      <td>97.0</td>
      <td>212.0</td>
      <td>0.458</td>
      <td>3.0</td>
      <td>19.0</td>
      <td>0.158</td>
      <td>94.0</td>
      <td>193.0</td>
      <td>0.487</td>
      <td>0.465</td>
      <td>28.0</td>
      <td>33.0</td>
      <td>0.848</td>
      <td>18.0</td>
      <td>45.0</td>
      <td>63.0</td>
      <td>35.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>49.0</td>
      <td>53.0</td>
      <td>225.0</td>
      <td>150000.0</td>
      <td>San Antonio Spurs</td>
      <td>11057000.0</td>
      <td>293563000.0</td>
      <td>0.013566</td>
      <td>0.000511</td>
      <td>0.037665</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4092</th>
      <td>1991</td>
      <td>Tyrone Hill</td>
      <td>PF</td>
      <td>22.0</td>
      <td>GSW</td>
      <td>74.0</td>
      <td>1192.0</td>
      <td>10.5</td>
      <td>0.533</td>
      <td>0.000</td>
      <td>0.508</td>
      <td>14.5</td>
      <td>20.9</td>
      <td>17.7</td>
      <td>2.1</td>
      <td>1.3</td>
      <td>1.5</td>
      <td>16.4</td>
      <td>14.7</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>2.1</td>
      <td>0.084</td>
      <td>-2.4</td>
      <td>-1.5</td>
      <td>-3.9</td>
      <td>-0.6</td>
      <td>147.0</td>
      <td>299.0</td>
      <td>0.492</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>147.0</td>
      <td>299.0</td>
      <td>0.492</td>
      <td>0.492</td>
      <td>96.0</td>
      <td>152.0</td>
      <td>0.632</td>
      <td>157.0</td>
      <td>226.0</td>
      <td>383.0</td>
      <td>19.0</td>
      <td>33.0</td>
      <td>30.0</td>
      <td>72.0</td>
      <td>264.0</td>
      <td>390.0</td>
      <td>1000000.0</td>
      <td>Golden State Warriors</td>
      <td>11150000.0</td>
      <td>293563000.0</td>
      <td>0.089686</td>
      <td>0.003406</td>
      <td>0.037982</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4093</th>
      <td>1991</td>
      <td>Roy Hinson</td>
      <td>PF</td>
      <td>29.0</td>
      <td>NJN</td>
      <td>9.0</td>
      <td>91.0</td>
      <td>11.1</td>
      <td>0.508</td>
      <td>0.000</td>
      <td>0.077</td>
      <td>6.7</td>
      <td>15.6</td>
      <td>11.0</td>
      <td>7.1</td>
      <td>0.0</td>
      <td>1.9</td>
      <td>13.0</td>
      <td>20.5</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.036</td>
      <td>-4.1</td>
      <td>-2.4</td>
      <td>-6.5</td>
      <td>-0.1</td>
      <td>20.0</td>
      <td>39.0</td>
      <td>0.513</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>20.0</td>
      <td>39.0</td>
      <td>0.513</td>
      <td>0.513</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>6.0</td>
      <td>13.0</td>
      <td>19.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>14.0</td>
      <td>41.0</td>
      <td>1250000.0</td>
      <td>Brooklyn Nets</td>
      <td>11410000.0</td>
      <td>293563000.0</td>
      <td>0.109553</td>
      <td>0.004258</td>
      <td>0.038867</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4094</th>
      <td>1991</td>
      <td>Craig Hodges</td>
      <td>SG</td>
      <td>30.0</td>
      <td>CHI</td>
      <td>73.0</td>
      <td>843.0</td>
      <td>12.4</td>
      <td>0.509</td>
      <td>0.334</td>
      <td>0.078</td>
      <td>1.4</td>
      <td>4.4</td>
      <td>2.9</td>
      <td>15.4</td>
      <td>2.0</td>
      <td>0.1</td>
      <td>9.0</td>
      <td>19.8</td>
      <td>1.0</td>
      <td>0.8</td>
      <td>1.7</td>
      <td>0.099</td>
      <td>-0.2</td>
      <td>-3.1</td>
      <td>-3.3</td>
      <td>-0.3</td>
      <td>146.0</td>
      <td>344.0</td>
      <td>0.424</td>
      <td>44.0</td>
      <td>115.0</td>
      <td>0.383</td>
      <td>102.0</td>
      <td>229.0</td>
      <td>0.445</td>
      <td>0.488</td>
      <td>26.0</td>
      <td>27.0</td>
      <td>0.963</td>
      <td>10.0</td>
      <td>32.0</td>
      <td>42.0</td>
      <td>97.0</td>
      <td>34.0</td>
      <td>2.0</td>
      <td>35.0</td>
      <td>74.0</td>
      <td>362.0</td>
      <td>600000.0</td>
      <td>Chicago Bulls</td>
      <td>10040000.0</td>
      <td>293563000.0</td>
      <td>0.059761</td>
      <td>0.002044</td>
      <td>0.034200</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4095</th>
      <td>1991</td>
      <td>Dave Hoppen</td>
      <td>C</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>30.0</td>
      <td>155.0</td>
      <td>11.7</td>
      <td>0.596</td>
      <td>0.045</td>
      <td>0.500</td>
      <td>13.1</td>
      <td>15.7</td>
      <td>14.4</td>
      <td>2.9</td>
      <td>1.0</td>
      <td>0.4</td>
      <td>19.5</td>
      <td>18.5</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.2</td>
      <td>0.073</td>
      <td>-2.5</td>
      <td>-3.3</td>
      <td>-5.8</td>
      <td>-0.1</td>
      <td>24.0</td>
      <td>44.0</td>
      <td>0.545</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>24.0</td>
      <td>42.0</td>
      <td>0.571</td>
      <td>0.545</td>
      <td>16.0</td>
      <td>22.0</td>
      <td>0.727</td>
      <td>18.0</td>
      <td>21.0</td>
      <td>39.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>29.0</td>
      <td>64.0</td>
      <td>400000.0</td>
      <td>Philadelphia 76ers</td>
      <td>11640000.0</td>
      <td>293563000.0</td>
      <td>0.034364</td>
      <td>0.001363</td>
      <td>0.039651</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4096</th>
      <td>1991</td>
      <td>Dave Hoppen</td>
      <td>C</td>
      <td>26.0</td>
      <td>CHH</td>
      <td>19.0</td>
      <td>112.0</td>
      <td>11.2</td>
      <td>0.604</td>
      <td>0.031</td>
      <td>0.313</td>
      <td>14.0</td>
      <td>16.9</td>
      <td>15.4</td>
      <td>4.0</td>
      <td>0.9</td>
      <td>0.6</td>
      <td>24.8</td>
      <td>18.4</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.038</td>
      <td>-2.8</td>
      <td>-2.2</td>
      <td>-5.0</td>
      <td>-0.1</td>
      <td>18.0</td>
      <td>32.0</td>
      <td>0.563</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>18.0</td>
      <td>31.0</td>
      <td>0.581</td>
      <td>0.563</td>
      <td>8.0</td>
      <td>10.0</td>
      <td>0.800</td>
      <td>14.0</td>
      <td>16.0</td>
      <td>30.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>12.0</td>
      <td>18.0</td>
      <td>44.0</td>
      <td>400000.0</td>
      <td>Philadelphia 76ers</td>
      <td>11640000.0</td>
      <td>293563000.0</td>
      <td>0.034364</td>
      <td>0.001363</td>
      <td>0.039651</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4097</th>
      <td>1991</td>
      <td>Dave Hoppen</td>
      <td>C</td>
      <td>26.0</td>
      <td>PHI</td>
      <td>11.0</td>
      <td>43.0</td>
      <td>13.2</td>
      <td>0.579</td>
      <td>0.083</td>
      <td>1.000</td>
      <td>10.7</td>
      <td>12.8</td>
      <td>11.7</td>
      <td>0.0</td>
      <td>1.2</td>
      <td>0.0</td>
      <td>5.5</td>
      <td>18.6</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.164</td>
      <td>-1.5</td>
      <td>-6.2</td>
      <td>-7.8</td>
      <td>-0.1</td>
      <td>6.0</td>
      <td>12.0</td>
      <td>0.500</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>6.0</td>
      <td>11.0</td>
      <td>0.545</td>
      <td>0.500</td>
      <td>8.0</td>
      <td>12.0</td>
      <td>0.667</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>11.0</td>
      <td>20.0</td>
      <td>400000.0</td>
      <td>Philadelphia 76ers</td>
      <td>11640000.0</td>
      <td>293563000.0</td>
      <td>0.034364</td>
      <td>0.001363</td>
      <td>0.039651</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4098</th>
      <td>1991</td>
      <td>Dennis Hopson</td>
      <td>SF</td>
      <td>25.0</td>
      <td>CHI</td>
      <td>61.0</td>
      <td>728.0</td>
      <td>10.0</td>
      <td>0.471</td>
      <td>0.020</td>
      <td>0.340</td>
      <td>8.0</td>
      <td>9.6</td>
      <td>8.8</td>
      <td>11.5</td>
      <td>1.7</td>
      <td>1.2</td>
      <td>17.4</td>
      <td>19.9</td>
      <td>-0.3</td>
      <td>0.8</td>
      <td>0.5</td>
      <td>0.035</td>
      <td>-3.2</td>
      <td>-0.5</td>
      <td>-3.8</td>
      <td>-0.3</td>
      <td>104.0</td>
      <td>244.0</td>
      <td>0.426</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>0.200</td>
      <td>103.0</td>
      <td>239.0</td>
      <td>0.431</td>
      <td>0.428</td>
      <td>55.0</td>
      <td>83.0</td>
      <td>0.663</td>
      <td>49.0</td>
      <td>60.0</td>
      <td>109.0</td>
      <td>65.0</td>
      <td>25.0</td>
      <td>14.0</td>
      <td>59.0</td>
      <td>79.0</td>
      <td>264.0</td>
      <td>915000.0</td>
      <td>Chicago Bulls</td>
      <td>10040000.0</td>
      <td>293563000.0</td>
      <td>0.091135</td>
      <td>0.003117</td>
      <td>0.034200</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4099</th>
      <td>1991</td>
      <td>Jeff Hornacek</td>
      <td>SG</td>
      <td>27.0</td>
      <td>PHO</td>
      <td>80.0</td>
      <td>2733.0</td>
      <td>17.8</td>
      <td>0.587</td>
      <td>0.139</td>
      <td>0.213</td>
      <td>3.1</td>
      <td>9.4</td>
      <td>6.4</td>
      <td>21.2</td>
      <td>1.9</td>
      <td>0.3</td>
      <td>10.2</td>
      <td>19.1</td>
      <td>6.9</td>
      <td>2.8</td>
      <td>9.7</td>
      <td>0.171</td>
      <td>3.6</td>
      <td>0.1</td>
      <td>3.7</td>
      <td>4.0</td>
      <td>544.0</td>
      <td>1051.0</td>
      <td>0.518</td>
      <td>61.0</td>
      <td>146.0</td>
      <td>0.418</td>
      <td>483.0</td>
      <td>905.0</td>
      <td>0.534</td>
      <td>0.547</td>
      <td>201.0</td>
      <td>224.0</td>
      <td>0.897</td>
      <td>74.0</td>
      <td>247.0</td>
      <td>321.0</td>
      <td>409.0</td>
      <td>111.0</td>
      <td>16.0</td>
      <td>130.0</td>
      <td>185.0</td>
      <td>1350.0</td>
      <td>1100000.0</td>
      <td>Phoenix Suns</td>
      <td>11463000.0</td>
      <td>293563000.0</td>
      <td>0.095961</td>
      <td>0.003747</td>
      <td>0.039048</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4100</th>
      <td>1991</td>
      <td>Jay Humphries</td>
      <td>PG</td>
      <td>28.0</td>
      <td>MIL</td>
      <td>80.0</td>
      <td>2726.0</td>
      <td>17.4</td>
      <td>0.570</td>
      <td>0.168</td>
      <td>0.249</td>
      <td>2.4</td>
      <td>7.3</td>
      <td>4.8</td>
      <td>29.7</td>
      <td>2.4</td>
      <td>0.2</td>
      <td>12.4</td>
      <td>19.1</td>
      <td>6.3</td>
      <td>2.3</td>
      <td>8.6</td>
      <td>0.152</td>
      <td>3.0</td>
      <td>-1.1</td>
      <td>2.0</td>
      <td>2.7</td>
      <td>482.0</td>
      <td>960.0</td>
      <td>0.502</td>
      <td>60.0</td>
      <td>161.0</td>
      <td>0.373</td>
      <td>422.0</td>
      <td>799.0</td>
      <td>0.528</td>
      <td>0.533</td>
      <td>191.0</td>
      <td>239.0</td>
      <td>0.799</td>
      <td>57.0</td>
      <td>163.0</td>
      <td>220.0</td>
      <td>538.0</td>
      <td>129.0</td>
      <td>7.0</td>
      <td>151.0</td>
      <td>237.0</td>
      <td>1215.0</td>
      <td>650000.0</td>
      <td>Milwaukee Bucks</td>
      <td>11595000.0</td>
      <td>293563000.0</td>
      <td>0.056059</td>
      <td>0.002214</td>
      <td>0.039497</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4101</th>
      <td>1991</td>
      <td>Byron Irvin</td>
      <td>SG</td>
      <td>24.0</td>
      <td>WSB</td>
      <td>33.0</td>
      <td>316.0</td>
      <td>17.5</td>
      <td>0.549</td>
      <td>0.039</td>
      <td>0.473</td>
      <td>8.3</td>
      <td>7.3</td>
      <td>7.8</td>
      <td>11.4</td>
      <td>2.3</td>
      <td>0.4</td>
      <td>9.3</td>
      <td>22.7</td>
      <td>0.7</td>
      <td>0.3</td>
      <td>0.9</td>
      <td>0.143</td>
      <td>0.7</td>
      <td>-1.9</td>
      <td>-1.2</td>
      <td>0.1</td>
      <td>60.0</td>
      <td>129.0</td>
      <td>0.465</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>0.200</td>
      <td>59.0</td>
      <td>124.0</td>
      <td>0.476</td>
      <td>0.469</td>
      <td>50.0</td>
      <td>61.0</td>
      <td>0.820</td>
      <td>24.0</td>
      <td>21.0</td>
      <td>45.0</td>
      <td>24.0</td>
      <td>15.0</td>
      <td>2.0</td>
      <td>16.0</td>
      <td>32.0</td>
      <td>171.0</td>
      <td>375000.0</td>
      <td>Washington Wizards</td>
      <td>9610000.0</td>
      <td>293563000.0</td>
      <td>0.039022</td>
      <td>0.001277</td>
      <td>0.032736</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4102</th>
      <td>1991</td>
      <td>Mark Jackson</td>
      <td>PG</td>
      <td>25.0</td>
      <td>NYK</td>
      <td>72.0</td>
      <td>1595.0</td>
      <td>18.2</td>
      <td>0.545</td>
      <td>0.100</td>
      <td>0.315</td>
      <td>4.6</td>
      <td>9.5</td>
      <td>7.1</td>
      <td>42.2</td>
      <td>1.9</td>
      <td>0.3</td>
      <td>18.9</td>
      <td>19.5</td>
      <td>2.7</td>
      <td>1.5</td>
      <td>4.2</td>
      <td>0.126</td>
      <td>1.7</td>
      <td>-1.4</td>
      <td>0.4</td>
      <td>1.0</td>
      <td>250.0</td>
      <td>508.0</td>
      <td>0.492</td>
      <td>13.0</td>
      <td>51.0</td>
      <td>0.255</td>
      <td>237.0</td>
      <td>457.0</td>
      <td>0.519</td>
      <td>0.505</td>
      <td>117.0</td>
      <td>160.0</td>
      <td>0.731</td>
      <td>62.0</td>
      <td>135.0</td>
      <td>197.0</td>
      <td>452.0</td>
      <td>60.0</td>
      <td>9.0</td>
      <td>135.0</td>
      <td>81.0</td>
      <td>630.0</td>
      <td>1785000.0</td>
      <td>New York Knicks</td>
      <td>13290000.0</td>
      <td>293563000.0</td>
      <td>0.134312</td>
      <td>0.006080</td>
      <td>0.045271</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4103</th>
      <td>1991</td>
      <td>Dave Jamerson</td>
      <td>SG</td>
      <td>23.0</td>
      <td>HOU</td>
      <td>37.0</td>
      <td>202.0</td>
      <td>12.2</td>
      <td>0.452</td>
      <td>0.168</td>
      <td>0.239</td>
      <td>4.8</td>
      <td>11.0</td>
      <td>7.9</td>
      <td>20.8</td>
      <td>1.4</td>
      <td>0.3</td>
      <td>13.8</td>
      <td>29.5</td>
      <td>-0.1</td>
      <td>0.2</td>
      <td>0.1</td>
      <td>0.026</td>
      <td>-2.8</td>
      <td>-2.2</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>43.0</td>
      <td>113.0</td>
      <td>0.381</td>
      <td>5.0</td>
      <td>19.0</td>
      <td>0.263</td>
      <td>38.0</td>
      <td>94.0</td>
      <td>0.404</td>
      <td>0.403</td>
      <td>22.0</td>
      <td>27.0</td>
      <td>0.815</td>
      <td>9.0</td>
      <td>21.0</td>
      <td>30.0</td>
      <td>27.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>20.0</td>
      <td>24.0</td>
      <td>113.0</td>
      <td>650000.0</td>
      <td>Houston Rockets</td>
      <td>10500000.0</td>
      <td>293563000.0</td>
      <td>0.061905</td>
      <td>0.002214</td>
      <td>0.035767</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4104</th>
      <td>1991</td>
      <td>Henry James</td>
      <td>PF</td>
      <td>25.0</td>
      <td>CLE</td>
      <td>37.0</td>
      <td>505.0</td>
      <td>14.7</td>
      <td>0.525</td>
      <td>0.236</td>
      <td>0.283</td>
      <td>6.0</td>
      <td>12.1</td>
      <td>9.1</td>
      <td>10.6</td>
      <td>1.5</td>
      <td>0.6</td>
      <td>11.5</td>
      <td>27.9</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>0.053</td>
      <td>-0.3</td>
      <td>-3.2</td>
      <td>-3.5</td>
      <td>-0.2</td>
      <td>112.0</td>
      <td>254.0</td>
      <td>0.441</td>
      <td>24.0</td>
      <td>60.0</td>
      <td>0.400</td>
      <td>88.0</td>
      <td>194.0</td>
      <td>0.454</td>
      <td>0.488</td>
      <td>52.0</td>
      <td>72.0</td>
      <td>0.722</td>
      <td>26.0</td>
      <td>53.0</td>
      <td>79.0</td>
      <td>32.0</td>
      <td>15.0</td>
      <td>5.0</td>
      <td>37.0</td>
      <td>59.0</td>
      <td>300.0</td>
      <td>75000.0</td>
      <td>Cleveland Caveliers</td>
      <td>14403000.0</td>
      <td>293563000.0</td>
      <td>0.005207</td>
      <td>0.000255</td>
      <td>0.049063</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4105</th>
      <td>1991</td>
      <td>Les Jepsen</td>
      <td>C</td>
      <td>23.0</td>
      <td>GSW</td>
      <td>21.0</td>
      <td>105.0</td>
      <td>7.7</td>
      <td>0.350</td>
      <td>0.028</td>
      <td>0.250</td>
      <td>17.8</td>
      <td>21.0</td>
      <td>19.4</td>
      <td>1.2</td>
      <td>0.4</td>
      <td>1.7</td>
      <td>7.0</td>
      <td>16.3</td>
      <td>-0.1</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.010</td>
      <td>-5.7</td>
      <td>-4.2</td>
      <td>-9.9</td>
      <td>-0.2</td>
      <td>11.0</td>
      <td>36.0</td>
      <td>0.306</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>11.0</td>
      <td>35.0</td>
      <td>0.314</td>
      <td>0.306</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>0.667</td>
      <td>17.0</td>
      <td>20.0</td>
      <td>37.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>16.0</td>
      <td>28.0</td>
      <td>505000.0</td>
      <td>Golden State Warriors</td>
      <td>11150000.0</td>
      <td>293563000.0</td>
      <td>0.045291</td>
      <td>0.001720</td>
      <td>0.037982</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4106</th>
      <td>1991</td>
      <td>Avery Johnson</td>
      <td>PG</td>
      <td>25.0</td>
      <td>TOT</td>
      <td>68.0</td>
      <td>959.0</td>
      <td>13.4</td>
      <td>0.507</td>
      <td>0.032</td>
      <td>0.314</td>
      <td>2.4</td>
      <td>6.0</td>
      <td>4.3</td>
      <td>31.4</td>
      <td>2.3</td>
      <td>0.2</td>
      <td>19.0</td>
      <td>16.2</td>
      <td>0.7</td>
      <td>0.9</td>
      <td>1.6</td>
      <td>0.082</td>
      <td>-1.8</td>
      <td>-1.8</td>
      <td>-3.6</td>
      <td>-0.4</td>
      <td>130.0</td>
      <td>277.0</td>
      <td>0.469</td>
      <td>1.0</td>
      <td>9.0</td>
      <td>0.111</td>
      <td>129.0</td>
      <td>268.0</td>
      <td>0.481</td>
      <td>0.471</td>
      <td>59.0</td>
      <td>87.0</td>
      <td>0.678</td>
      <td>22.0</td>
      <td>55.0</td>
      <td>77.0</td>
      <td>230.0</td>
      <td>47.0</td>
      <td>4.0</td>
      <td>74.0</td>
      <td>62.0</td>
      <td>320.0</td>
      <td>50000.0</td>
      <td>San Antonio Spurs</td>
      <td>11057000.0</td>
      <td>293563000.0</td>
      <td>0.004522</td>
      <td>0.000170</td>
      <td>0.037665</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4107</th>
      <td>1991</td>
      <td>Avery Johnson</td>
      <td>PG</td>
      <td>25.0</td>
      <td>DEN</td>
      <td>21.0</td>
      <td>217.0</td>
      <td>14.2</td>
      <td>0.481</td>
      <td>0.059</td>
      <td>0.471</td>
      <td>3.6</td>
      <td>5.8</td>
      <td>4.6</td>
      <td>41.5</td>
      <td>2.7</td>
      <td>0.5</td>
      <td>24.8</td>
      <td>17.7</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.014</td>
      <td>-2.5</td>
      <td>-3.7</td>
      <td>-6.2</td>
      <td>-0.2</td>
      <td>29.0</td>
      <td>68.0</td>
      <td>0.426</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.000</td>
      <td>29.0</td>
      <td>64.0</td>
      <td>0.453</td>
      <td>0.426</td>
      <td>21.0</td>
      <td>32.0</td>
      <td>0.656</td>
      <td>9.0</td>
      <td>12.0</td>
      <td>21.0</td>
      <td>77.0</td>
      <td>14.0</td>
      <td>2.0</td>
      <td>27.0</td>
      <td>22.0</td>
      <td>79.0</td>
      <td>50000.0</td>
      <td>San Antonio Spurs</td>
      <td>11057000.0</td>
      <td>293563000.0</td>
      <td>0.004522</td>
      <td>0.000170</td>
      <td>0.037665</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4108</th>
      <td>1991</td>
      <td>Avery Johnson</td>
      <td>PG</td>
      <td>25.0</td>
      <td>SAS</td>
      <td>47.0</td>
      <td>742.0</td>
      <td>13.2</td>
      <td>0.517</td>
      <td>0.024</td>
      <td>0.263</td>
      <td>2.0</td>
      <td>6.1</td>
      <td>4.2</td>
      <td>28.5</td>
      <td>2.2</td>
      <td>0.2</td>
      <td>16.8</td>
      <td>15.7</td>
      <td>0.7</td>
      <td>0.9</td>
      <td>1.6</td>
      <td>0.101</td>
      <td>-1.6</td>
      <td>-1.3</td>
      <td>-2.8</td>
      <td>-0.2</td>
      <td>101.0</td>
      <td>209.0</td>
      <td>0.483</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>0.200</td>
      <td>100.0</td>
      <td>204.0</td>
      <td>0.490</td>
      <td>0.486</td>
      <td>38.0</td>
      <td>55.0</td>
      <td>0.691</td>
      <td>13.0</td>
      <td>43.0</td>
      <td>56.0</td>
      <td>153.0</td>
      <td>33.0</td>
      <td>2.0</td>
      <td>47.0</td>
      <td>40.0</td>
      <td>241.0</td>
      <td>50000.0</td>
      <td>San Antonio Spurs</td>
      <td>11057000.0</td>
      <td>293563000.0</td>
      <td>0.004522</td>
      <td>0.000170</td>
      <td>0.037665</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4109</th>
      <td>1991</td>
      <td>Buck Johnson</td>
      <td>SF</td>
      <td>27.0</td>
      <td>HOU</td>
      <td>73.0</td>
      <td>2279.0</td>
      <td>12.9</td>
      <td>0.512</td>
      <td>0.017</td>
      <td>0.247</td>
      <td>5.1</td>
      <td>10.3</td>
      <td>7.7</td>
      <td>9.2</td>
      <td>1.7</td>
      <td>1.2</td>
      <td>11.2</td>
      <td>19.7</td>
      <td>1.6</td>
      <td>3.0</td>
      <td>4.6</td>
      <td>0.097</td>
      <td>-1.5</td>
      <td>1.0</td>
      <td>-0.5</td>
      <td>0.9</td>
      <td>416.0</td>
      <td>873.0</td>
      <td>0.477</td>
      <td>2.0</td>
      <td>15.0</td>
      <td>0.133</td>
      <td>414.0</td>
      <td>858.0</td>
      <td>0.483</td>
      <td>0.478</td>
      <td>157.0</td>
      <td>216.0</td>
      <td>0.727</td>
      <td>108.0</td>
      <td>222.0</td>
      <td>330.0</td>
      <td>142.0</td>
      <td>81.0</td>
      <td>47.0</td>
      <td>122.0</td>
      <td>240.0</td>
      <td>991.0</td>
      <td>675000.0</td>
      <td>Houston Rockets</td>
      <td>10500000.0</td>
      <td>293563000.0</td>
      <td>0.064286</td>
      <td>0.002299</td>
      <td>0.035767</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4110</th>
      <td>1991</td>
      <td>Eddie Johnson</td>
      <td>SG</td>
      <td>31.0</td>
      <td>TOT</td>
      <td>81.0</td>
      <td>2085.0</td>
      <td>17.8</td>
      <td>0.548</td>
      <td>0.107</td>
      <td>0.229</td>
      <td>6.0</td>
      <td>9.3</td>
      <td>7.7</td>
      <td>8.5</td>
      <td>1.4</td>
      <td>0.3</td>
      <td>9.0</td>
      <td>27.2</td>
      <td>4.3</td>
      <td>1.2</td>
      <td>5.5</td>
      <td>0.127</td>
      <td>1.8</td>
      <td>-3.1</td>
      <td>-1.3</td>
      <td>0.4</td>
      <td>543.0</td>
      <td>1122.0</td>
      <td>0.484</td>
      <td>39.0</td>
      <td>120.0</td>
      <td>0.325</td>
      <td>504.0</td>
      <td>1002.0</td>
      <td>0.503</td>
      <td>0.501</td>
      <td>229.0</td>
      <td>257.0</td>
      <td>0.891</td>
      <td>107.0</td>
      <td>164.0</td>
      <td>271.0</td>
      <td>111.0</td>
      <td>58.0</td>
      <td>9.0</td>
      <td>122.0</td>
      <td>181.0</td>
      <td>1354.0</td>
      <td>1000000.0</td>
      <td>Oklahoma City Thunder</td>
      <td>10590000.0</td>
      <td>293563000.0</td>
      <td>0.094429</td>
      <td>0.003406</td>
      <td>0.036074</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4111</th>
      <td>1991</td>
      <td>Eddie Johnson</td>
      <td>SG</td>
      <td>31.0</td>
      <td>PHO</td>
      <td>15.0</td>
      <td>312.0</td>
      <td>14.5</td>
      <td>0.511</td>
      <td>0.113</td>
      <td>0.156</td>
      <td>5.9</td>
      <td>10.0</td>
      <td>8.1</td>
      <td>8.7</td>
      <td>1.4</td>
      <td>0.4</td>
      <td>10.8</td>
      <td>29.1</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>0.3</td>
      <td>0.051</td>
      <td>-0.9</td>
      <td>-2.9</td>
      <td>-3.9</td>
      <td>-0.1</td>
      <td>88.0</td>
      <td>186.0</td>
      <td>0.473</td>
      <td>6.0</td>
      <td>21.0</td>
      <td>0.286</td>
      <td>82.0</td>
      <td>165.0</td>
      <td>0.497</td>
      <td>0.489</td>
      <td>21.0</td>
      <td>29.0</td>
      <td>0.724</td>
      <td>16.0</td>
      <td>30.0</td>
      <td>46.0</td>
      <td>17.0</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>24.0</td>
      <td>37.0</td>
      <td>203.0</td>
      <td>1000000.0</td>
      <td>Oklahoma City Thunder</td>
      <td>10590000.0</td>
      <td>293563000.0</td>
      <td>0.094429</td>
      <td>0.003406</td>
      <td>0.036074</td>
      <td>South</td>
    </tr>
  </tbody>
</table>
<p>200 rows × 57 columns</p>
</div>



Very nice! 

There are a few rows where the 'salaries' data didn't contain the players listed in the 'Seasons_Stats' dataframe, so for the sake of simplicity (because those players probably weren't that big of a deal anyways) I'll just get rid of those rows


```
data['player_stats'] = data['player_stats'].dropna()
```


```
data['player_stats'].count()

# No NA values!
```




    Year                 12730
    Player               12730
    Pos                  12730
    Age                  12730
    Tm                   12730
    G                    12730
    MP                   12730
    PER                  12730
    TS%                  12730
    3PAr                 12730
    FTr                  12730
    ORB%                 12730
    DRB%                 12730
    TRB%                 12730
    AST%                 12730
    STL%                 12730
    BLK%                 12730
    TOV%                 12730
    USG%                 12730
    OWS                  12730
    DWS                  12730
    WS                   12730
    WS/48                12730
    OBPM                 12730
    DBPM                 12730
    BPM                  12730
    VORP                 12730
    FG                   12730
    FGA                  12730
    FG%                  12730
    3P                   12730
    3PA                  12730
    3P%                  12730
    2P                   12730
    2PA                  12730
    2P%                  12730
    eFG%                 12730
    FT                   12730
    FTA                  12730
    FT%                  12730
    ORB                  12730
    DRB                  12730
    TRB                  12730
    AST                  12730
    STL                  12730
    BLK                  12730
    TOV                  12730
    PF                   12730
    PTS                  12730
    Salary               12730
    Team                 12730
    Team Payroll         12730
    Total NBA Payroll    12730
    Player Leverage      12730
    League Weight        12730
    Team Market Size     12730
    US Region            12730
    dtype: int64




```
data['player_stats'].head(500)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>Tm</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3912</th>
      <td>1991</td>
      <td>Alaa Abdelnaby</td>
      <td>PF</td>
      <td>22.0</td>
      <td>POR</td>
      <td>43.0</td>
      <td>290.0</td>
      <td>13.1</td>
      <td>0.499</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>10.4</td>
      <td>23.4</td>
      <td>17.0</td>
      <td>5.8</td>
      <td>0.7</td>
      <td>2.5</td>
      <td>14.0</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.079</td>
      <td>-4.2</td>
      <td>-0.7</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.474</td>
      <td>25.0</td>
      <td>44.0</td>
      <td>0.568</td>
      <td>27.0</td>
      <td>62.0</td>
      <td>89.0</td>
      <td>12.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>22.0</td>
      <td>39.0</td>
      <td>135.0</td>
      <td>395000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.035221</td>
      <td>0.001346</td>
      <td>0.038203</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3913</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>PG</td>
      <td>21.0</td>
      <td>DEN</td>
      <td>67.0</td>
      <td>1505.0</td>
      <td>12.2</td>
      <td>0.448</td>
      <td>0.099</td>
      <td>0.097</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>3.8</td>
      <td>19.2</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>27.2</td>
      <td>-0.7</td>
      <td>-0.3</td>
      <td>-1.0</td>
      <td>-0.031</td>
      <td>-1.7</td>
      <td>-4.4</td>
      <td>-6.1</td>
      <td>-1.6</td>
      <td>417.0</td>
      <td>1009.0</td>
      <td>0.413</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>0.240</td>
      <td>393.0</td>
      <td>909.0</td>
      <td>0.432</td>
      <td>0.425</td>
      <td>84.0</td>
      <td>98.0</td>
      <td>0.857</td>
      <td>34.0</td>
      <td>87.0</td>
      <td>121.0</td>
      <td>206.0</td>
      <td>55.0</td>
      <td>4.0</td>
      <td>110.0</td>
      <td>149.0</td>
      <td>942.0</td>
      <td>1660000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.160619</td>
      <td>0.005655</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3914</th>
      <td>1991</td>
      <td>Mark Acres</td>
      <td>C</td>
      <td>28.0</td>
      <td>ORL</td>
      <td>68.0</td>
      <td>1313.0</td>
      <td>9.2</td>
      <td>0.551</td>
      <td>0.014</td>
      <td>0.472</td>
      <td>11.3</td>
      <td>18.7</td>
      <td>14.9</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>14.0</td>
      <td>9.3</td>
      <td>1.4</td>
      <td>1.1</td>
      <td>2.5</td>
      <td>0.090</td>
      <td>-2.1</td>
      <td>0.3</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>109.0</td>
      <td>214.0</td>
      <td>0.509</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>108.0</td>
      <td>211.0</td>
      <td>0.512</td>
      <td>0.512</td>
      <td>66.0</td>
      <td>101.0</td>
      <td>0.653</td>
      <td>140.0</td>
      <td>219.0</td>
      <td>359.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>42.0</td>
      <td>218.0</td>
      <td>285.0</td>
      <td>437000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.058019</td>
      <td>0.001489</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3915</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>PG</td>
      <td>28.0</td>
      <td>DEN</td>
      <td>66.0</td>
      <td>2346.0</td>
      <td>22.3</td>
      <td>0.530</td>
      <td>0.397</td>
      <td>0.372</td>
      <td>2.1</td>
      <td>8.8</td>
      <td>5.2</td>
      <td>39.4</td>
      <td>2.6</td>
      <td>0.1</td>
      <td>12.7</td>
      <td>28.5</td>
      <td>5.8</td>
      <td>0.4</td>
      <td>6.3</td>
      <td>0.128</td>
      <td>7.1</td>
      <td>-2.7</td>
      <td>4.4</td>
      <td>3.8</td>
      <td>560.0</td>
      <td>1421.0</td>
      <td>0.394</td>
      <td>167.0</td>
      <td>564.0</td>
      <td>0.296</td>
      <td>393.0</td>
      <td>857.0</td>
      <td>0.459</td>
      <td>0.453</td>
      <td>465.0</td>
      <td>529.0</td>
      <td>0.879</td>
      <td>58.0</td>
      <td>198.0</td>
      <td>256.0</td>
      <td>693.0</td>
      <td>147.0</td>
      <td>6.0</td>
      <td>240.0</td>
      <td>162.0</td>
      <td>1752.0</td>
      <td>825000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.079826</td>
      <td>0.002810</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3916</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>SF</td>
      <td>31.0</td>
      <td>DET</td>
      <td>78.0</td>
      <td>2006.0</td>
      <td>16.7</td>
      <td>0.526</td>
      <td>0.086</td>
      <td>0.349</td>
      <td>7.6</td>
      <td>13.7</td>
      <td>10.7</td>
      <td>11.6</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>10.9</td>
      <td>25.7</td>
      <td>2.8</td>
      <td>2.7</td>
      <td>5.5</td>
      <td>0.132</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>0.7</td>
      <td>1.4</td>
      <td>420.0</td>
      <td>909.0</td>
      <td>0.462</td>
      <td>24.0</td>
      <td>78.0</td>
      <td>0.308</td>
      <td>396.0</td>
      <td>831.0</td>
      <td>0.477</td>
      <td>0.475</td>
      <td>240.0</td>
      <td>317.0</td>
      <td>0.757</td>
      <td>134.0</td>
      <td>240.0</td>
      <td>374.0</td>
      <td>139.0</td>
      <td>47.0</td>
      <td>20.0</td>
      <td>128.0</td>
      <td>209.0</td>
      <td>1104.0</td>
      <td>1115000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.086367</td>
      <td>0.003798</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3917</th>
      <td>1991</td>
      <td>Danny Ainge</td>
      <td>SG</td>
      <td>31.0</td>
      <td>POR</td>
      <td>80.0</td>
      <td>1710.0</td>
      <td>17.0</td>
      <td>0.574</td>
      <td>0.352</td>
      <td>0.193</td>
      <td>2.9</td>
      <td>10.2</td>
      <td>6.6</td>
      <td>23.8</td>
      <td>1.8</td>
      <td>0.5</td>
      <td>11.4</td>
      <td>20.8</td>
      <td>4.1</td>
      <td>2.1</td>
      <td>6.2</td>
      <td>0.175</td>
      <td>3.4</td>
      <td>-0.6</td>
      <td>2.8</td>
      <td>2.1</td>
      <td>337.0</td>
      <td>714.0</td>
      <td>0.472</td>
      <td>102.0</td>
      <td>251.0</td>
      <td>0.406</td>
      <td>235.0</td>
      <td>463.0</td>
      <td>0.508</td>
      <td>0.543</td>
      <td>114.0</td>
      <td>138.0</td>
      <td>0.826</td>
      <td>45.0</td>
      <td>160.0</td>
      <td>205.0</td>
      <td>285.0</td>
      <td>63.0</td>
      <td>13.0</td>
      <td>100.0</td>
      <td>195.0</td>
      <td>890.0</td>
      <td>725000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.064646</td>
      <td>0.002470</td>
      <td>0.038203</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3918</th>
      <td>1991</td>
      <td>Mark Alarie</td>
      <td>PF</td>
      <td>27.0</td>
      <td>WSB</td>
      <td>42.0</td>
      <td>587.0</td>
      <td>11.3</td>
      <td>0.496</td>
      <td>0.093</td>
      <td>0.213</td>
      <td>7.6</td>
      <td>14.2</td>
      <td>10.9</td>
      <td>11.2</td>
      <td>1.3</td>
      <td>0.8</td>
      <td>14.0</td>
      <td>20.3</td>
      <td>0.1</td>
      <td>0.6</td>
      <td>0.6</td>
      <td>0.050</td>
      <td>-2.4</td>
      <td>-1.1</td>
      <td>-3.6</td>
      <td>-0.2</td>
      <td>99.0</td>
      <td>225.0</td>
      <td>0.440</td>
      <td>5.0</td>
      <td>21.0</td>
      <td>0.238</td>
      <td>94.0</td>
      <td>204.0</td>
      <td>0.461</td>
      <td>0.451</td>
      <td>41.0</td>
      <td>48.0</td>
      <td>0.854</td>
      <td>41.0</td>
      <td>76.0</td>
      <td>117.0</td>
      <td>45.0</td>
      <td>15.0</td>
      <td>8.0</td>
      <td>40.0</td>
      <td>88.0</td>
      <td>244.0</td>
      <td>500000.0</td>
      <td>Washington Wizards</td>
      <td>9610000.0</td>
      <td>293563000.0</td>
      <td>0.052029</td>
      <td>0.001703</td>
      <td>0.032736</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3919</th>
      <td>1991</td>
      <td>Steve Alford</td>
      <td>PG</td>
      <td>26.0</td>
      <td>DAL</td>
      <td>34.0</td>
      <td>236.0</td>
      <td>20.5</td>
      <td>0.578</td>
      <td>0.197</td>
      <td>0.265</td>
      <td>4.8</td>
      <td>6.8</td>
      <td>5.8</td>
      <td>16.4</td>
      <td>1.7</td>
      <td>0.3</td>
      <td>10.9</td>
      <td>27.5</td>
      <td>0.5</td>
      <td>0.1</td>
      <td>0.6</td>
      <td>0.121</td>
      <td>2.4</td>
      <td>-3.8</td>
      <td>-1.4</td>
      <td>0.0</td>
      <td>59.0</td>
      <td>117.0</td>
      <td>0.504</td>
      <td>7.0</td>
      <td>23.0</td>
      <td>0.304</td>
      <td>52.0</td>
      <td>94.0</td>
      <td>0.553</td>
      <td>0.534</td>
      <td>26.0</td>
      <td>31.0</td>
      <td>0.839</td>
      <td>10.0</td>
      <td>14.0</td>
      <td>24.0</td>
      <td>22.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>16.0</td>
      <td>11.0</td>
      <td>151.0</td>
      <td>250000.0</td>
      <td>Dallas Mavericks</td>
      <td>11693000.0</td>
      <td>293563000.0</td>
      <td>0.021380</td>
      <td>0.000852</td>
      <td>0.039831</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3920</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>68.0</td>
      <td>924.0</td>
      <td>9.1</td>
      <td>0.455</td>
      <td>0.004</td>
      <td>0.426</td>
      <td>10.0</td>
      <td>25.9</td>
      <td>17.3</td>
      <td>2.1</td>
      <td>1.7</td>
      <td>2.7</td>
      <td>20.8</td>
      <td>16.3</td>
      <td>-1.2</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>-0.017</td>
      <td>-6.8</td>
      <td>-0.8</td>
      <td>-7.5</td>
      <td>-1.3</td>
      <td>116.0</td>
      <td>270.0</td>
      <td>0.430</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>116.0</td>
      <td>269.0</td>
      <td>0.431</td>
      <td>0.430</td>
      <td>60.0</td>
      <td>115.0</td>
      <td>0.522</td>
      <td>97.0</td>
      <td>221.0</td>
      <td>318.0</td>
      <td>16.0</td>
      <td>35.0</td>
      <td>45.0</td>
      <td>84.0</td>
      <td>140.0</td>
      <td>292.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3921</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>MIL</td>
      <td>26.0</td>
      <td>247.0</td>
      <td>7.5</td>
      <td>0.410</td>
      <td>0.014</td>
      <td>0.384</td>
      <td>12.3</td>
      <td>24.1</td>
      <td>18.1</td>
      <td>1.7</td>
      <td>1.6</td>
      <td>2.3</td>
      <td>20.5</td>
      <td>18.6</td>
      <td>-0.5</td>
      <td>0.4</td>
      <td>-0.1</td>
      <td>-0.027</td>
      <td>-7.8</td>
      <td>-1.3</td>
      <td>-9.1</td>
      <td>-0.4</td>
      <td>27.0</td>
      <td>73.0</td>
      <td>0.370</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>27.0</td>
      <td>72.0</td>
      <td>0.375</td>
      <td>0.370</td>
      <td>16.0</td>
      <td>28.0</td>
      <td>0.571</td>
      <td>26.0</td>
      <td>49.0</td>
      <td>75.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>22.0</td>
      <td>29.0</td>
      <td>70.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3922</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>NJN</td>
      <td>1.0</td>
      <td>18.0</td>
      <td>26.4</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>22.4</td>
      <td>12.1</td>
      <td>17.5</td>
      <td>9.1</td>
      <td>5.4</td>
      <td>0.0</td>
      <td>20.0</td>
      <td>11.2</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.300</td>
      <td>8.3</td>
      <td>2.5</td>
      <td>10.9</td>
      <td>0.1</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>8.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3923</th>
      <td>1991</td>
      <td>Greg Anderson</td>
      <td>PF</td>
      <td>26.0</td>
      <td>DEN</td>
      <td>41.0</td>
      <td>659.0</td>
      <td>9.2</td>
      <td>0.463</td>
      <td>0.000</td>
      <td>0.451</td>
      <td>8.7</td>
      <td>27.0</td>
      <td>17.0</td>
      <td>2.1</td>
      <td>1.6</td>
      <td>2.9</td>
      <td>20.9</td>
      <td>15.6</td>
      <td>-0.8</td>
      <td>0.5</td>
      <td>-0.3</td>
      <td>-0.022</td>
      <td>-6.8</td>
      <td>-0.6</td>
      <td>-7.5</td>
      <td>-0.9</td>
      <td>85.0</td>
      <td>193.0</td>
      <td>0.440</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>85.0</td>
      <td>193.0</td>
      <td>0.440</td>
      <td>0.440</td>
      <td>44.0</td>
      <td>87.0</td>
      <td>0.506</td>
      <td>67.0</td>
      <td>170.0</td>
      <td>237.0</td>
      <td>12.0</td>
      <td>25.0</td>
      <td>36.0</td>
      <td>61.0</td>
      <td>107.0</td>
      <td>214.0</td>
      <td>1365000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.132075</td>
      <td>0.004650</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3924</th>
      <td>1991</td>
      <td>Nick Anderson</td>
      <td>SG</td>
      <td>23.0</td>
      <td>ORL</td>
      <td>70.0</td>
      <td>1971.0</td>
      <td>15.1</td>
      <td>0.510</td>
      <td>0.068</td>
      <td>0.302</td>
      <td>4.9</td>
      <td>16.7</td>
      <td>10.7</td>
      <td>8.5</td>
      <td>1.8</td>
      <td>1.3</td>
      <td>10.4</td>
      <td>22.4</td>
      <td>1.2</td>
      <td>1.9</td>
      <td>3.1</td>
      <td>0.075</td>
      <td>-1.1</td>
      <td>0.1</td>
      <td>-1.0</td>
      <td>0.5</td>
      <td>400.0</td>
      <td>857.0</td>
      <td>0.467</td>
      <td>17.0</td>
      <td>58.0</td>
      <td>0.293</td>
      <td>383.0</td>
      <td>799.0</td>
      <td>0.479</td>
      <td>0.477</td>
      <td>173.0</td>
      <td>259.0</td>
      <td>0.668</td>
      <td>92.0</td>
      <td>294.0</td>
      <td>386.0</td>
      <td>106.0</td>
      <td>74.0</td>
      <td>44.0</td>
      <td>113.0</td>
      <td>145.0</td>
      <td>990.0</td>
      <td>725000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.096256</td>
      <td>0.002470</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3925</th>
      <td>1991</td>
      <td>Ron Anderson</td>
      <td>SF</td>
      <td>32.0</td>
      <td>PHI</td>
      <td>82.0</td>
      <td>2340.0</td>
      <td>15.5</td>
      <td>0.524</td>
      <td>0.041</td>
      <td>0.188</td>
      <td>5.0</td>
      <td>12.4</td>
      <td>8.8</td>
      <td>8.2</td>
      <td>1.4</td>
      <td>0.3</td>
      <td>8.1</td>
      <td>23.2</td>
      <td>2.3</td>
      <td>1.8</td>
      <td>4.1</td>
      <td>0.085</td>
      <td>-0.5</td>
      <td>-1.8</td>
      <td>-2.3</td>
      <td>-0.2</td>
      <td>512.0</td>
      <td>1055.0</td>
      <td>0.485</td>
      <td>9.0</td>
      <td>43.0</td>
      <td>0.209</td>
      <td>503.0</td>
      <td>1012.0</td>
      <td>0.497</td>
      <td>0.490</td>
      <td>165.0</td>
      <td>198.0</td>
      <td>0.833</td>
      <td>103.0</td>
      <td>264.0</td>
      <td>367.0</td>
      <td>115.0</td>
      <td>65.0</td>
      <td>13.0</td>
      <td>100.0</td>
      <td>163.0</td>
      <td>1198.0</td>
      <td>425000.0</td>
      <td>Philadelphia 76ers</td>
      <td>11640000.0</td>
      <td>293563000.0</td>
      <td>0.036512</td>
      <td>0.001448</td>
      <td>0.039651</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>3926</th>
      <td>1991</td>
      <td>Willie Anderson</td>
      <td>SG</td>
      <td>24.0</td>
      <td>SAS</td>
      <td>75.0</td>
      <td>2592.0</td>
      <td>13.0</td>
      <td>0.499</td>
      <td>0.035</td>
      <td>0.215</td>
      <td>3.1</td>
      <td>11.5</td>
      <td>7.5</td>
      <td>20.2</td>
      <td>1.5</td>
      <td>1.1</td>
      <td>13.3</td>
      <td>20.1</td>
      <td>1.3</td>
      <td>3.5</td>
      <td>4.8</td>
      <td>0.089</td>
      <td>-0.8</td>
      <td>0.9</td>
      <td>0.1</td>
      <td>1.4</td>
      <td>453.0</td>
      <td>991.0</td>
      <td>0.457</td>
      <td>7.0</td>
      <td>35.0</td>
      <td>0.200</td>
      <td>446.0</td>
      <td>956.0</td>
      <td>0.467</td>
      <td>0.461</td>
      <td>170.0</td>
      <td>213.0</td>
      <td>0.798</td>
      <td>68.0</td>
      <td>283.0</td>
      <td>351.0</td>
      <td>358.0</td>
      <td>79.0</td>
      <td>46.0</td>
      <td>167.0</td>
      <td>226.0</td>
      <td>1083.0</td>
      <td>725000.0</td>
      <td>San Antonio Spurs</td>
      <td>11057000.0</td>
      <td>293563000.0</td>
      <td>0.065569</td>
      <td>0.002470</td>
      <td>0.037665</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3927</th>
      <td>1991</td>
      <td>Michael Ansley</td>
      <td>SF</td>
      <td>23.0</td>
      <td>ORL</td>
      <td>67.0</td>
      <td>877.0</td>
      <td>16.9</td>
      <td>0.594</td>
      <td>0.000</td>
      <td>0.483</td>
      <td>14.7</td>
      <td>16.8</td>
      <td>15.7</td>
      <td>4.3</td>
      <td>1.5</td>
      <td>0.5</td>
      <td>9.1</td>
      <td>16.3</td>
      <td>2.3</td>
      <td>0.7</td>
      <td>3.1</td>
      <td>0.168</td>
      <td>1.2</td>
      <td>-1.2</td>
      <td>0.0</td>
      <td>0.4</td>
      <td>144.0</td>
      <td>263.0</td>
      <td>0.548</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>144.0</td>
      <td>263.0</td>
      <td>0.548</td>
      <td>0.548</td>
      <td>91.0</td>
      <td>127.0</td>
      <td>0.717</td>
      <td>122.0</td>
      <td>131.0</td>
      <td>253.0</td>
      <td>25.0</td>
      <td>27.0</td>
      <td>7.0</td>
      <td>32.0</td>
      <td>125.0</td>
      <td>379.0</td>
      <td>200000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.026553</td>
      <td>0.000681</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3928</th>
      <td>1991</td>
      <td>B.J. Armstrong</td>
      <td>PG</td>
      <td>23.0</td>
      <td>CHI</td>
      <td>82.0</td>
      <td>1731.0</td>
      <td>14.8</td>
      <td>0.529</td>
      <td>0.047</td>
      <td>0.176</td>
      <td>1.7</td>
      <td>8.3</td>
      <td>5.1</td>
      <td>23.4</td>
      <td>2.0</td>
      <td>0.1</td>
      <td>13.6</td>
      <td>19.5</td>
      <td>2.3</td>
      <td>1.8</td>
      <td>4.1</td>
      <td>0.114</td>
      <td>-0.4</td>
      <td>-1.4</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>304.0</td>
      <td>632.0</td>
      <td>0.481</td>
      <td>15.0</td>
      <td>30.0</td>
      <td>0.500</td>
      <td>289.0</td>
      <td>602.0</td>
      <td>0.480</td>
      <td>0.493</td>
      <td>97.0</td>
      <td>111.0</td>
      <td>0.874</td>
      <td>25.0</td>
      <td>124.0</td>
      <td>149.0</td>
      <td>301.0</td>
      <td>70.0</td>
      <td>4.0</td>
      <td>107.0</td>
      <td>118.0</td>
      <td>720.0</td>
      <td>425000.0</td>
      <td>Chicago Bulls</td>
      <td>10040000.0</td>
      <td>293563000.0</td>
      <td>0.042331</td>
      <td>0.001448</td>
      <td>0.034200</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3930</th>
      <td>1991</td>
      <td>Keith Askins</td>
      <td>SF</td>
      <td>23.0</td>
      <td>MIA</td>
      <td>39.0</td>
      <td>266.0</td>
      <td>13.5</td>
      <td>0.467</td>
      <td>0.309</td>
      <td>0.309</td>
      <td>12.4</td>
      <td>16.2</td>
      <td>14.3</td>
      <td>10.2</td>
      <td>2.9</td>
      <td>3.0</td>
      <td>10.7</td>
      <td>15.8</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.6</td>
      <td>0.103</td>
      <td>-0.6</td>
      <td>2.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>34.0</td>
      <td>81.0</td>
      <td>0.420</td>
      <td>6.0</td>
      <td>25.0</td>
      <td>0.240</td>
      <td>28.0</td>
      <td>56.0</td>
      <td>0.500</td>
      <td>0.457</td>
      <td>12.0</td>
      <td>25.0</td>
      <td>0.480</td>
      <td>30.0</td>
      <td>38.0</td>
      <td>68.0</td>
      <td>19.0</td>
      <td>16.0</td>
      <td>13.0</td>
      <td>11.0</td>
      <td>46.0</td>
      <td>86.0</td>
      <td>150000.0</td>
      <td>Miami Heat</td>
      <td>8510000.0</td>
      <td>293563000.0</td>
      <td>0.017626</td>
      <td>0.000511</td>
      <td>0.028989</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3931</th>
      <td>1991</td>
      <td>Milos Babic</td>
      <td>PF</td>
      <td>22.0</td>
      <td>CLE</td>
      <td>12.0</td>
      <td>52.0</td>
      <td>6.0</td>
      <td>0.391</td>
      <td>0.000</td>
      <td>0.632</td>
      <td>13.4</td>
      <td>6.7</td>
      <td>10.0</td>
      <td>10.9</td>
      <td>1.0</td>
      <td>1.2</td>
      <td>17.1</td>
      <td>24.6</td>
      <td>-0.1</td>
      <td>0.0</td>
      <td>-0.1</td>
      <td>-0.085</td>
      <td>-6.0</td>
      <td>-2.6</td>
      <td>-8.6</td>
      <td>-0.1</td>
      <td>6.0</td>
      <td>19.0</td>
      <td>0.316</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>6.0</td>
      <td>19.0</td>
      <td>0.316</td>
      <td>0.316</td>
      <td>7.0</td>
      <td>12.0</td>
      <td>0.583</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>9.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>19.0</td>
      <td>200000.0</td>
      <td>Cleveland Caveliers</td>
      <td>14403000.0</td>
      <td>293563000.0</td>
      <td>0.013886</td>
      <td>0.000681</td>
      <td>0.049063</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3932</th>
      <td>1991</td>
      <td>Thurl Bailey</td>
      <td>PF</td>
      <td>29.0</td>
      <td>UTA</td>
      <td>82.0</td>
      <td>2486.0</td>
      <td>12.5</td>
      <td>0.513</td>
      <td>0.003</td>
      <td>0.311</td>
      <td>5.1</td>
      <td>13.6</td>
      <td>9.6</td>
      <td>7.7</td>
      <td>1.1</td>
      <td>2.3</td>
      <td>11.6</td>
      <td>20.0</td>
      <td>0.6</td>
      <td>3.1</td>
      <td>3.7</td>
      <td>0.072</td>
      <td>-2.4</td>
      <td>1.2</td>
      <td>-1.2</td>
      <td>0.5</td>
      <td>399.0</td>
      <td>872.0</td>
      <td>0.458</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.000</td>
      <td>399.0</td>
      <td>869.0</td>
      <td>0.459</td>
      <td>0.458</td>
      <td>219.0</td>
      <td>271.0</td>
      <td>0.808</td>
      <td>101.0</td>
      <td>306.0</td>
      <td>407.0</td>
      <td>124.0</td>
      <td>53.0</td>
      <td>91.0</td>
      <td>130.0</td>
      <td>160.0</td>
      <td>1017.0</td>
      <td>1000000.0</td>
      <td>Utah Jazz</td>
      <td>10695000.0</td>
      <td>293563000.0</td>
      <td>0.093502</td>
      <td>0.003406</td>
      <td>0.036432</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3934</th>
      <td>1991</td>
      <td>Ken Bannister</td>
      <td>PF</td>
      <td>30.0</td>
      <td>LAC</td>
      <td>47.0</td>
      <td>339.0</td>
      <td>7.8</td>
      <td>0.506</td>
      <td>0.012</td>
      <td>0.802</td>
      <td>10.6</td>
      <td>20.0</td>
      <td>15.3</td>
      <td>3.6</td>
      <td>0.7</td>
      <td>1.2</td>
      <td>18.6</td>
      <td>16.1</td>
      <td>-0.2</td>
      <td>0.4</td>
      <td>0.2</td>
      <td>0.028</td>
      <td>-5.0</td>
      <td>-1.2</td>
      <td>-6.3</td>
      <td>-0.4</td>
      <td>43.0</td>
      <td>81.0</td>
      <td>0.531</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>43.0</td>
      <td>80.0</td>
      <td>0.538</td>
      <td>0.531</td>
      <td>25.0</td>
      <td>65.0</td>
      <td>0.385</td>
      <td>34.0</td>
      <td>62.0</td>
      <td>96.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>25.0</td>
      <td>73.0</td>
      <td>111.0</td>
      <td>400000.0</td>
      <td>Los Angeles Clippers</td>
      <td>10245000.0</td>
      <td>293563000.0</td>
      <td>0.039043</td>
      <td>0.001363</td>
      <td>0.034899</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3935</th>
      <td>1991</td>
      <td>Ken Bannister</td>
      <td>PF</td>
      <td>30.0</td>
      <td>LAC</td>
      <td>47.0</td>
      <td>339.0</td>
      <td>7.8</td>
      <td>0.506</td>
      <td>0.012</td>
      <td>0.802</td>
      <td>10.6</td>
      <td>20.0</td>
      <td>15.3</td>
      <td>3.6</td>
      <td>0.7</td>
      <td>1.2</td>
      <td>18.6</td>
      <td>16.1</td>
      <td>-0.2</td>
      <td>0.4</td>
      <td>0.2</td>
      <td>0.028</td>
      <td>-5.0</td>
      <td>-1.2</td>
      <td>-6.3</td>
      <td>-0.4</td>
      <td>43.0</td>
      <td>81.0</td>
      <td>0.531</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>43.0</td>
      <td>80.0</td>
      <td>0.538</td>
      <td>0.531</td>
      <td>25.0</td>
      <td>65.0</td>
      <td>0.385</td>
      <td>34.0</td>
      <td>62.0</td>
      <td>96.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>25.0</td>
      <td>73.0</td>
      <td>111.0</td>
      <td>50000.0</td>
      <td>Utah Jazz</td>
      <td>10695000.0</td>
      <td>293563000.0</td>
      <td>0.004675</td>
      <td>0.000170</td>
      <td>0.036432</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3937</th>
      <td>1991</td>
      <td>Dana Barros</td>
      <td>PG</td>
      <td>23.0</td>
      <td>SEA</td>
      <td>66.0</td>
      <td>750.0</td>
      <td>18.8</td>
      <td>0.600</td>
      <td>0.260</td>
      <td>0.273</td>
      <td>2.7</td>
      <td>8.7</td>
      <td>5.7</td>
      <td>21.9</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>13.4</td>
      <td>22.5</td>
      <td>2.1</td>
      <td>0.4</td>
      <td>2.5</td>
      <td>0.158</td>
      <td>3.0</td>
      <td>-3.2</td>
      <td>-0.2</td>
      <td>0.3</td>
      <td>154.0</td>
      <td>311.0</td>
      <td>0.495</td>
      <td>32.0</td>
      <td>81.0</td>
      <td>0.395</td>
      <td>122.0</td>
      <td>230.0</td>
      <td>0.530</td>
      <td>0.547</td>
      <td>78.0</td>
      <td>85.0</td>
      <td>0.918</td>
      <td>17.0</td>
      <td>54.0</td>
      <td>71.0</td>
      <td>111.0</td>
      <td>23.0</td>
      <td>1.0</td>
      <td>54.0</td>
      <td>40.0</td>
      <td>418.0</td>
      <td>500000.0</td>
      <td>Oklahoma City Thunder</td>
      <td>10590000.0</td>
      <td>293563000.0</td>
      <td>0.047214</td>
      <td>0.001703</td>
      <td>0.036074</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3938</th>
      <td>1991</td>
      <td>John Battle</td>
      <td>SG</td>
      <td>28.0</td>
      <td>ATL</td>
      <td>79.0</td>
      <td>1863.0</td>
      <td>16.4</td>
      <td>0.538</td>
      <td>0.057</td>
      <td>0.367</td>
      <td>1.9</td>
      <td>7.6</td>
      <td>4.7</td>
      <td>18.4</td>
      <td>1.2</td>
      <td>0.2</td>
      <td>10.1</td>
      <td>24.7</td>
      <td>3.4</td>
      <td>0.6</td>
      <td>4.0</td>
      <td>0.102</td>
      <td>0.7</td>
      <td>-3.1</td>
      <td>-2.4</td>
      <td>-0.2</td>
      <td>397.0</td>
      <td>862.0</td>
      <td>0.461</td>
      <td>14.0</td>
      <td>49.0</td>
      <td>0.286</td>
      <td>383.0</td>
      <td>813.0</td>
      <td>0.471</td>
      <td>0.469</td>
      <td>270.0</td>
      <td>316.0</td>
      <td>0.854</td>
      <td>34.0</td>
      <td>125.0</td>
      <td>159.0</td>
      <td>217.0</td>
      <td>45.0</td>
      <td>6.0</td>
      <td>113.0</td>
      <td>145.0</td>
      <td>1078.0</td>
      <td>590000.0</td>
      <td>Atlanta Hawks</td>
      <td>11761000.0</td>
      <td>293563000.0</td>
      <td>0.050166</td>
      <td>0.002010</td>
      <td>0.040063</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3939</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>SG</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>56.0</td>
      <td>945.0</td>
      <td>12.4</td>
      <td>0.525</td>
      <td>0.085</td>
      <td>0.330</td>
      <td>8.2</td>
      <td>10.3</td>
      <td>9.2</td>
      <td>7.9</td>
      <td>2.8</td>
      <td>1.0</td>
      <td>14.1</td>
      <td>14.7</td>
      <td>0.7</td>
      <td>0.6</td>
      <td>1.3</td>
      <td>0.068</td>
      <td>-1.1</td>
      <td>0.4</td>
      <td>-0.7</td>
      <td>0.3</td>
      <td>133.0</td>
      <td>282.0</td>
      <td>0.472</td>
      <td>3.0</td>
      <td>24.0</td>
      <td>0.125</td>
      <td>130.0</td>
      <td>258.0</td>
      <td>0.504</td>
      <td>0.477</td>
      <td>70.0</td>
      <td>93.0</td>
      <td>0.753</td>
      <td>83.0</td>
      <td>93.0</td>
      <td>176.0</td>
      <td>62.0</td>
      <td>60.0</td>
      <td>18.0</td>
      <td>53.0</td>
      <td>108.0</td>
      <td>339.0</td>
      <td>350000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3940</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>SG</td>
      <td>26.0</td>
      <td>PHO</td>
      <td>16.0</td>
      <td>263.0</td>
      <td>12.8</td>
      <td>0.486</td>
      <td>0.023</td>
      <td>0.337</td>
      <td>9.2</td>
      <td>12.7</td>
      <td>11.0</td>
      <td>7.5</td>
      <td>3.4</td>
      <td>1.3</td>
      <td>14.7</td>
      <td>17.9</td>
      <td>0.0</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.081</td>
      <td>-1.7</td>
      <td>1.8</td>
      <td>0.2</td>
      <td>0.1</td>
      <td>38.0</td>
      <td>86.0</td>
      <td>0.442</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>38.0</td>
      <td>84.0</td>
      <td>0.452</td>
      <td>0.442</td>
      <td>20.0</td>
      <td>29.0</td>
      <td>0.690</td>
      <td>21.0</td>
      <td>32.0</td>
      <td>53.0</td>
      <td>15.0</td>
      <td>19.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>25.0</td>
      <td>96.0</td>
      <td>350000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3941</th>
      <td>1991</td>
      <td>Kenny Battle</td>
      <td>SG</td>
      <td>26.0</td>
      <td>DEN</td>
      <td>40.0</td>
      <td>682.0</td>
      <td>12.3</td>
      <td>0.542</td>
      <td>0.112</td>
      <td>0.327</td>
      <td>7.8</td>
      <td>9.4</td>
      <td>8.5</td>
      <td>8.1</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>13.8</td>
      <td>13.4</td>
      <td>0.7</td>
      <td>0.2</td>
      <td>0.9</td>
      <td>0.063</td>
      <td>-0.8</td>
      <td>-0.2</td>
      <td>-1.0</td>
      <td>0.2</td>
      <td>95.0</td>
      <td>196.0</td>
      <td>0.485</td>
      <td>3.0</td>
      <td>22.0</td>
      <td>0.136</td>
      <td>92.0</td>
      <td>174.0</td>
      <td>0.529</td>
      <td>0.492</td>
      <td>50.0</td>
      <td>64.0</td>
      <td>0.781</td>
      <td>62.0</td>
      <td>61.0</td>
      <td>123.0</td>
      <td>47.0</td>
      <td>41.0</td>
      <td>12.0</td>
      <td>36.0</td>
      <td>83.0</td>
      <td>243.0</td>
      <td>350000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.033866</td>
      <td>0.001192</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3942</th>
      <td>1991</td>
      <td>William Bedford</td>
      <td>C</td>
      <td>27.0</td>
      <td>DET</td>
      <td>60.0</td>
      <td>562.0</td>
      <td>15.5</td>
      <td>0.492</td>
      <td>0.054</td>
      <td>0.322</td>
      <td>11.1</td>
      <td>15.5</td>
      <td>13.3</td>
      <td>9.2</td>
      <td>0.2</td>
      <td>4.1</td>
      <td>10.4</td>
      <td>24.1</td>
      <td>0.5</td>
      <td>0.8</td>
      <td>1.3</td>
      <td>0.113</td>
      <td>-1.6</td>
      <td>0.2</td>
      <td>-1.4</td>
      <td>0.1</td>
      <td>106.0</td>
      <td>242.0</td>
      <td>0.438</td>
      <td>5.0</td>
      <td>13.0</td>
      <td>0.385</td>
      <td>101.0</td>
      <td>229.0</td>
      <td>0.441</td>
      <td>0.448</td>
      <td>55.0</td>
      <td>78.0</td>
      <td>0.705</td>
      <td>55.0</td>
      <td>76.0</td>
      <td>131.0</td>
      <td>32.0</td>
      <td>2.0</td>
      <td>36.0</td>
      <td>32.0</td>
      <td>76.0</td>
      <td>272.0</td>
      <td>850000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.065840</td>
      <td>0.002895</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>3943</th>
      <td>1991</td>
      <td>Benoit Benjamin</td>
      <td>C</td>
      <td>26.0</td>
      <td>TOT</td>
      <td>70.0</td>
      <td>2236.0</td>
      <td>15.1</td>
      <td>0.541</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>7.8</td>
      <td>28.7</td>
      <td>18.1</td>
      <td>7.7</td>
      <td>1.2</td>
      <td>4.0</td>
      <td>20.6</td>
      <td>21.0</td>
      <td>-0.7</td>
      <td>3.7</td>
      <td>3.0</td>
      <td>0.064</td>
      <td>-3.5</td>
      <td>2.3</td>
      <td>-1.1</td>
      <td>0.5</td>
      <td>386.0</td>
      <td>778.0</td>
      <td>0.496</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>386.0</td>
      <td>778.0</td>
      <td>0.496</td>
      <td>0.496</td>
      <td>210.0</td>
      <td>295.0</td>
      <td>0.712</td>
      <td>157.0</td>
      <td>566.0</td>
      <td>723.0</td>
      <td>119.0</td>
      <td>54.0</td>
      <td>145.0</td>
      <td>235.0</td>
      <td>184.0</td>
      <td>982.0</td>
      <td>1750000.0</td>
      <td>Oklahoma City Thunder</td>
      <td>10590000.0</td>
      <td>293563000.0</td>
      <td>0.165250</td>
      <td>0.005961</td>
      <td>0.036074</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3944</th>
      <td>1991</td>
      <td>Benoit Benjamin</td>
      <td>C</td>
      <td>26.0</td>
      <td>LAC</td>
      <td>39.0</td>
      <td>1337.0</td>
      <td>15.2</td>
      <td>0.539</td>
      <td>0.000</td>
      <td>0.363</td>
      <td>7.5</td>
      <td>30.7</td>
      <td>18.9</td>
      <td>8.1</td>
      <td>0.9</td>
      <td>4.1</td>
      <td>20.4</td>
      <td>20.6</td>
      <td>-0.5</td>
      <td>2.4</td>
      <td>1.9</td>
      <td>0.068</td>
      <td>-3.8</td>
      <td>2.7</td>
      <td>-1.1</td>
      <td>0.3</td>
      <td>229.0</td>
      <td>465.0</td>
      <td>0.492</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>229.0</td>
      <td>465.0</td>
      <td>0.492</td>
      <td>0.492</td>
      <td>123.0</td>
      <td>169.0</td>
      <td>0.728</td>
      <td>95.0</td>
      <td>374.0</td>
      <td>469.0</td>
      <td>74.0</td>
      <td>26.0</td>
      <td>91.0</td>
      <td>138.0</td>
      <td>110.0</td>
      <td>581.0</td>
      <td>1750000.0</td>
      <td>Oklahoma City Thunder</td>
      <td>10590000.0</td>
      <td>293563000.0</td>
      <td>0.165250</td>
      <td>0.005961</td>
      <td>0.036074</td>
      <td>South</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4491</th>
      <td>1992</td>
      <td>Dave Feitl</td>
      <td>C</td>
      <td>29.0</td>
      <td>NJN</td>
      <td>34.0</td>
      <td>175.0</td>
      <td>11.9</td>
      <td>0.480</td>
      <td>0.000</td>
      <td>0.247</td>
      <td>12.3</td>
      <td>25.1</td>
      <td>18.4</td>
      <td>5.0</td>
      <td>0.6</td>
      <td>1.0</td>
      <td>18.2</td>
      <td>23.9</td>
      <td>-0.1</td>
      <td>0.2</td>
      <td>0.1</td>
      <td>0.019</td>
      <td>-5.8</td>
      <td>-3.5</td>
      <td>-9.3</td>
      <td>-0.3</td>
      <td>33.0</td>
      <td>77.0</td>
      <td>0.429</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>33.0</td>
      <td>77.0</td>
      <td>0.429</td>
      <td>0.429</td>
      <td>16.0</td>
      <td>19.0</td>
      <td>0.842</td>
      <td>21.0</td>
      <td>40.0</td>
      <td>61.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>19.0</td>
      <td>22.0</td>
      <td>82.0</td>
      <td>135000.0</td>
      <td>Brooklyn Nets</td>
      <td>12598000.0</td>
      <td>354154000.0</td>
      <td>0.010716</td>
      <td>0.000381</td>
      <td>0.035572</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4492</th>
      <td>1992</td>
      <td>Duane Ferrell</td>
      <td>SF</td>
      <td>26.0</td>
      <td>ATL</td>
      <td>66.0</td>
      <td>1598.0</td>
      <td>15.6</td>
      <td>0.576</td>
      <td>0.052</td>
      <td>0.345</td>
      <td>6.8</td>
      <td>7.2</td>
      <td>7.0</td>
      <td>8.6</td>
      <td>1.5</td>
      <td>0.6</td>
      <td>12.0</td>
      <td>21.4</td>
      <td>2.7</td>
      <td>0.9</td>
      <td>3.5</td>
      <td>0.107</td>
      <td>0.7</td>
      <td>-1.5</td>
      <td>-0.8</td>
      <td>0.5</td>
      <td>331.0</td>
      <td>632.0</td>
      <td>0.524</td>
      <td>11.0</td>
      <td>33.0</td>
      <td>0.333</td>
      <td>320.0</td>
      <td>599.0</td>
      <td>0.534</td>
      <td>0.532</td>
      <td>166.0</td>
      <td>218.0</td>
      <td>0.761</td>
      <td>105.0</td>
      <td>105.0</td>
      <td>210.0</td>
      <td>92.0</td>
      <td>49.0</td>
      <td>17.0</td>
      <td>99.0</td>
      <td>134.0</td>
      <td>839.0</td>
      <td>500000.0</td>
      <td>Atlanta Hawks</td>
      <td>12930000.0</td>
      <td>354154000.0</td>
      <td>0.038670</td>
      <td>0.001412</td>
      <td>0.036510</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4493</th>
      <td>1992</td>
      <td>Danny Ferry</td>
      <td>PF</td>
      <td>25.0</td>
      <td>CLE</td>
      <td>68.0</td>
      <td>937.0</td>
      <td>11.2</td>
      <td>0.480</td>
      <td>0.146</td>
      <td>0.223</td>
      <td>6.6</td>
      <td>18.3</td>
      <td>12.7</td>
      <td>11.1</td>
      <td>1.2</td>
      <td>0.9</td>
      <td>11.3</td>
      <td>18.9</td>
      <td>0.3</td>
      <td>1.0</td>
      <td>1.3</td>
      <td>0.067</td>
      <td>-2.2</td>
      <td>-0.8</td>
      <td>-3.0</td>
      <td>-0.2</td>
      <td>134.0</td>
      <td>328.0</td>
      <td>0.409</td>
      <td>17.0</td>
      <td>48.0</td>
      <td>0.354</td>
      <td>117.0</td>
      <td>280.0</td>
      <td>0.418</td>
      <td>0.434</td>
      <td>61.0</td>
      <td>73.0</td>
      <td>0.836</td>
      <td>53.0</td>
      <td>160.0</td>
      <td>213.0</td>
      <td>75.0</td>
      <td>22.0</td>
      <td>15.0</td>
      <td>46.0</td>
      <td>135.0</td>
      <td>346.0</td>
      <td>2843000.0</td>
      <td>Cleveland Caveliers</td>
      <td>16882000.0</td>
      <td>354154000.0</td>
      <td>0.168404</td>
      <td>0.008028</td>
      <td>0.047669</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4494</th>
      <td>1992</td>
      <td>Vern Fleming</td>
      <td>PG</td>
      <td>29.0</td>
      <td>IND</td>
      <td>82.0</td>
      <td>1737.0</td>
      <td>12.7</td>
      <td>0.527</td>
      <td>0.044</td>
      <td>0.293</td>
      <td>4.7</td>
      <td>8.5</td>
      <td>6.7</td>
      <td>21.7</td>
      <td>1.6</td>
      <td>0.2</td>
      <td>16.9</td>
      <td>20.0</td>
      <td>1.1</td>
      <td>0.9</td>
      <td>2.0</td>
      <td>0.055</td>
      <td>-1.2</td>
      <td>-1.6</td>
      <td>-2.8</td>
      <td>-0.3</td>
      <td>294.0</td>
      <td>610.0</td>
      <td>0.482</td>
      <td>6.0</td>
      <td>27.0</td>
      <td>0.222</td>
      <td>288.0</td>
      <td>583.0</td>
      <td>0.494</td>
      <td>0.487</td>
      <td>132.0</td>
      <td>179.0</td>
      <td>0.737</td>
      <td>69.0</td>
      <td>140.0</td>
      <td>209.0</td>
      <td>266.0</td>
      <td>56.0</td>
      <td>7.0</td>
      <td>140.0</td>
      <td>134.0</td>
      <td>726.0</td>
      <td>760000.0</td>
      <td>Indiana Pacers</td>
      <td>15090000.0</td>
      <td>354154000.0</td>
      <td>0.050364</td>
      <td>0.002146</td>
      <td>0.042609</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4495</th>
      <td>1992</td>
      <td>Sleepy Floyd</td>
      <td>PG</td>
      <td>31.0</td>
      <td>HOU</td>
      <td>82.0</td>
      <td>1662.0</td>
      <td>12.1</td>
      <td>0.478</td>
      <td>0.175</td>
      <td>0.241</td>
      <td>2.3</td>
      <td>7.6</td>
      <td>5.0</td>
      <td>22.1</td>
      <td>1.7</td>
      <td>0.7</td>
      <td>14.1</td>
      <td>23.7</td>
      <td>-0.3</td>
      <td>1.2</td>
      <td>0.9</td>
      <td>0.025</td>
      <td>-2.1</td>
      <td>-2.0</td>
      <td>-4.2</td>
      <td>-0.9</td>
      <td>286.0</td>
      <td>704.0</td>
      <td>0.406</td>
      <td>37.0</td>
      <td>123.0</td>
      <td>0.301</td>
      <td>249.0</td>
      <td>581.0</td>
      <td>0.429</td>
      <td>0.433</td>
      <td>135.0</td>
      <td>170.0</td>
      <td>0.794</td>
      <td>34.0</td>
      <td>116.0</td>
      <td>150.0</td>
      <td>239.0</td>
      <td>57.0</td>
      <td>21.0</td>
      <td>128.0</td>
      <td>128.0</td>
      <td>744.0</td>
      <td>1500000.0</td>
      <td>Houston Rockets</td>
      <td>13274000.0</td>
      <td>354154000.0</td>
      <td>0.113003</td>
      <td>0.004235</td>
      <td>0.037481</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4496</th>
      <td>1992</td>
      <td>Greg Foster</td>
      <td>PF</td>
      <td>23.0</td>
      <td>WSB</td>
      <td>49.0</td>
      <td>548.0</td>
      <td>11.2</td>
      <td>0.496</td>
      <td>0.005</td>
      <td>0.254</td>
      <td>8.4</td>
      <td>20.9</td>
      <td>14.5</td>
      <td>9.3</td>
      <td>0.5</td>
      <td>1.3</td>
      <td>14.4</td>
      <td>19.3</td>
      <td>-0.1</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.043</td>
      <td>-4.2</td>
      <td>-1.2</td>
      <td>-5.4</td>
      <td>-0.5</td>
      <td>89.0</td>
      <td>193.0</td>
      <td>0.461</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>89.0</td>
      <td>192.0</td>
      <td>0.464</td>
      <td>0.461</td>
      <td>35.0</td>
      <td>49.0</td>
      <td>0.714</td>
      <td>43.0</td>
      <td>102.0</td>
      <td>145.0</td>
      <td>35.0</td>
      <td>6.0</td>
      <td>12.0</td>
      <td>36.0</td>
      <td>83.0</td>
      <td>213.0</td>
      <td>339000.0</td>
      <td>Washington Wizards</td>
      <td>12633000.0</td>
      <td>354154000.0</td>
      <td>0.026834</td>
      <td>0.000957</td>
      <td>0.035671</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4497</th>
      <td>1992</td>
      <td>Rick Fox</td>
      <td>SF</td>
      <td>22.0</td>
      <td>BOS</td>
      <td>81.0</td>
      <td>1535.0</td>
      <td>12.2</td>
      <td>0.531</td>
      <td>0.133</td>
      <td>0.350</td>
      <td>5.5</td>
      <td>10.1</td>
      <td>7.9</td>
      <td>11.1</td>
      <td>2.5</td>
      <td>1.2</td>
      <td>16.9</td>
      <td>20.4</td>
      <td>0.4</td>
      <td>1.9</td>
      <td>2.3</td>
      <td>0.073</td>
      <td>-0.8</td>
      <td>0.8</td>
      <td>0.0</td>
      <td>0.8</td>
      <td>241.0</td>
      <td>525.0</td>
      <td>0.459</td>
      <td>23.0</td>
      <td>70.0</td>
      <td>0.329</td>
      <td>218.0</td>
      <td>455.0</td>
      <td>0.479</td>
      <td>0.481</td>
      <td>139.0</td>
      <td>184.0</td>
      <td>0.755</td>
      <td>73.0</td>
      <td>147.0</td>
      <td>220.0</td>
      <td>126.0</td>
      <td>78.0</td>
      <td>30.0</td>
      <td>123.0</td>
      <td>230.0</td>
      <td>644.0</td>
      <td>525000.0</td>
      <td>Boston Celtic</td>
      <td>25343000.0</td>
      <td>354154000.0</td>
      <td>0.020716</td>
      <td>0.001482</td>
      <td>0.071559</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4499</th>
      <td>1992</td>
      <td>Anthony Frederick</td>
      <td>SF</td>
      <td>27.0</td>
      <td>CHH</td>
      <td>66.0</td>
      <td>852.0</td>
      <td>13.0</td>
      <td>0.474</td>
      <td>0.046</td>
      <td>0.249</td>
      <td>9.3</td>
      <td>8.9</td>
      <td>9.1</td>
      <td>11.5</td>
      <td>2.2</td>
      <td>1.8</td>
      <td>12.4</td>
      <td>22.2</td>
      <td>-0.1</td>
      <td>0.6</td>
      <td>0.5</td>
      <td>0.028</td>
      <td>-1.9</td>
      <td>-0.8</td>
      <td>-2.7</td>
      <td>-0.2</td>
      <td>161.0</td>
      <td>370.0</td>
      <td>0.435</td>
      <td>4.0</td>
      <td>17.0</td>
      <td>0.235</td>
      <td>157.0</td>
      <td>353.0</td>
      <td>0.445</td>
      <td>0.441</td>
      <td>63.0</td>
      <td>92.0</td>
      <td>0.685</td>
      <td>75.0</td>
      <td>69.0</td>
      <td>144.0</td>
      <td>71.0</td>
      <td>40.0</td>
      <td>26.0</td>
      <td>58.0</td>
      <td>91.0</td>
      <td>389.0</td>
      <td>130000.0</td>
      <td>Charlotte Hornets</td>
      <td>12625000.0</td>
      <td>354154000.0</td>
      <td>0.010297</td>
      <td>0.000367</td>
      <td>0.035648</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4500</th>
      <td>1992</td>
      <td>Kevin Gamble</td>
      <td>SF</td>
      <td>26.0</td>
      <td>BOS</td>
      <td>82.0</td>
      <td>2496.0</td>
      <td>15.2</td>
      <td>0.567</td>
      <td>0.034</td>
      <td>0.173</td>
      <td>3.7</td>
      <td>8.7</td>
      <td>6.3</td>
      <td>12.5</td>
      <td>1.5</td>
      <td>0.9</td>
      <td>9.0</td>
      <td>18.5</td>
      <td>4.5</td>
      <td>2.2</td>
      <td>6.7</td>
      <td>0.129</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>1.7</td>
      <td>2.3</td>
      <td>480.0</td>
      <td>908.0</td>
      <td>0.529</td>
      <td>9.0</td>
      <td>31.0</td>
      <td>0.290</td>
      <td>471.0</td>
      <td>877.0</td>
      <td>0.537</td>
      <td>0.534</td>
      <td>139.0</td>
      <td>157.0</td>
      <td>0.885</td>
      <td>80.0</td>
      <td>206.0</td>
      <td>286.0</td>
      <td>219.0</td>
      <td>75.0</td>
      <td>37.0</td>
      <td>97.0</td>
      <td>200.0</td>
      <td>1108.0</td>
      <td>1400000.0</td>
      <td>Boston Celtic</td>
      <td>25343000.0</td>
      <td>354154000.0</td>
      <td>0.055242</td>
      <td>0.003953</td>
      <td>0.071559</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4501</th>
      <td>1992</td>
      <td>Winston Garland</td>
      <td>PG</td>
      <td>27.0</td>
      <td>DEN</td>
      <td>78.0</td>
      <td>2209.0</td>
      <td>13.3</td>
      <td>0.505</td>
      <td>0.037</td>
      <td>0.265</td>
      <td>3.1</td>
      <td>6.4</td>
      <td>4.6</td>
      <td>27.6</td>
      <td>2.2</td>
      <td>0.6</td>
      <td>17.3</td>
      <td>18.6</td>
      <td>1.3</td>
      <td>1.6</td>
      <td>2.9</td>
      <td>0.063</td>
      <td>-0.9</td>
      <td>-1.1</td>
      <td>-2.0</td>
      <td>0.0</td>
      <td>333.0</td>
      <td>750.0</td>
      <td>0.444</td>
      <td>9.0</td>
      <td>28.0</td>
      <td>0.321</td>
      <td>324.0</td>
      <td>722.0</td>
      <td>0.449</td>
      <td>0.450</td>
      <td>171.0</td>
      <td>199.0</td>
      <td>0.859</td>
      <td>67.0</td>
      <td>123.0</td>
      <td>190.0</td>
      <td>411.0</td>
      <td>98.0</td>
      <td>22.0</td>
      <td>175.0</td>
      <td>206.0</td>
      <td>846.0</td>
      <td>450000.0</td>
      <td>Denver Nuggets</td>
      <td>11879000.0</td>
      <td>354154000.0</td>
      <td>0.037882</td>
      <td>0.001271</td>
      <td>0.033542</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4502</th>
      <td>1992</td>
      <td>Tom Garrick</td>
      <td>SG</td>
      <td>25.0</td>
      <td>TOT</td>
      <td>40.0</td>
      <td>549.0</td>
      <td>9.8</td>
      <td>0.444</td>
      <td>0.028</td>
      <td>0.182</td>
      <td>2.4</td>
      <td>8.8</td>
      <td>5.6</td>
      <td>24.2</td>
      <td>3.3</td>
      <td>0.4</td>
      <td>22.2</td>
      <td>15.3</td>
      <td>-0.5</td>
      <td>0.7</td>
      <td>0.2</td>
      <td>0.022</td>
      <td>-3.9</td>
      <td>0.5</td>
      <td>-3.4</td>
      <td>-0.2</td>
      <td>59.0</td>
      <td>143.0</td>
      <td>0.413</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.250</td>
      <td>58.0</td>
      <td>139.0</td>
      <td>0.417</td>
      <td>0.416</td>
      <td>18.0</td>
      <td>26.0</td>
      <td>0.692</td>
      <td>12.0</td>
      <td>44.0</td>
      <td>56.0</td>
      <td>98.0</td>
      <td>36.0</td>
      <td>4.0</td>
      <td>44.0</td>
      <td>54.0</td>
      <td>137.0</td>
      <td>200000.0</td>
      <td>Minnesota Timberwolves</td>
      <td>10763000.0</td>
      <td>354154000.0</td>
      <td>0.018582</td>
      <td>0.000565</td>
      <td>0.030391</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4503</th>
      <td>1992</td>
      <td>Tom Garrick</td>
      <td>SG</td>
      <td>25.0</td>
      <td>SAS</td>
      <td>19.0</td>
      <td>374.0</td>
      <td>10.2</td>
      <td>0.490</td>
      <td>0.011</td>
      <td>0.172</td>
      <td>2.4</td>
      <td>9.6</td>
      <td>6.0</td>
      <td>22.9</td>
      <td>2.8</td>
      <td>0.2</td>
      <td>23.1</td>
      <td>14.7</td>
      <td>-0.1</td>
      <td>0.5</td>
      <td>0.4</td>
      <td>0.050</td>
      <td>-3.1</td>
      <td>0.7</td>
      <td>-2.4</td>
      <td>0.0</td>
      <td>44.0</td>
      <td>93.0</td>
      <td>0.473</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>44.0</td>
      <td>92.0</td>
      <td>0.478</td>
      <td>0.473</td>
      <td>10.0</td>
      <td>16.0</td>
      <td>0.625</td>
      <td>8.0</td>
      <td>33.0</td>
      <td>41.0</td>
      <td>63.0</td>
      <td>21.0</td>
      <td>1.0</td>
      <td>30.0</td>
      <td>33.0</td>
      <td>98.0</td>
      <td>200000.0</td>
      <td>Minnesota Timberwolves</td>
      <td>10763000.0</td>
      <td>354154000.0</td>
      <td>0.018582</td>
      <td>0.000565</td>
      <td>0.030391</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4504</th>
      <td>1992</td>
      <td>Tom Garrick</td>
      <td>SG</td>
      <td>25.0</td>
      <td>MIN</td>
      <td>15.0</td>
      <td>112.0</td>
      <td>7.4</td>
      <td>0.397</td>
      <td>0.061</td>
      <td>0.242</td>
      <td>1.9</td>
      <td>7.1</td>
      <td>4.4</td>
      <td>21.4</td>
      <td>3.2</td>
      <td>1.6</td>
      <td>21.5</td>
      <td>17.7</td>
      <td>-0.2</td>
      <td>0.1</td>
      <td>-0.1</td>
      <td>-0.059</td>
      <td>-5.9</td>
      <td>-0.8</td>
      <td>-6.7</td>
      <td>-0.1</td>
      <td>11.0</td>
      <td>33.0</td>
      <td>0.333</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.500</td>
      <td>10.0</td>
      <td>31.0</td>
      <td>0.323</td>
      <td>0.348</td>
      <td>6.0</td>
      <td>8.0</td>
      <td>0.750</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>18.0</td>
      <td>7.0</td>
      <td>3.0</td>
      <td>10.0</td>
      <td>14.0</td>
      <td>29.0</td>
      <td>200000.0</td>
      <td>Minnesota Timberwolves</td>
      <td>10763000.0</td>
      <td>354154000.0</td>
      <td>0.018582</td>
      <td>0.000565</td>
      <td>0.030391</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4505</th>
      <td>1992</td>
      <td>Tom Garrick</td>
      <td>SG</td>
      <td>25.0</td>
      <td>DAL</td>
      <td>6.0</td>
      <td>63.0</td>
      <td>11.3</td>
      <td>0.280</td>
      <td>0.059</td>
      <td>0.118</td>
      <td>3.3</td>
      <td>7.0</td>
      <td>5.1</td>
      <td>37.2</td>
      <td>6.5</td>
      <td>0.0</td>
      <td>18.3</td>
      <td>14.9</td>
      <td>-0.1</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>-0.004</td>
      <td>-4.4</td>
      <td>1.4</td>
      <td>-3.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>17.0</td>
      <td>0.235</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.000</td>
      <td>4.0</td>
      <td>16.0</td>
      <td>0.250</td>
      <td>0.235</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1.000</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
      <td>200000.0</td>
      <td>Minnesota Timberwolves</td>
      <td>10763000.0</td>
      <td>354154000.0</td>
      <td>0.018582</td>
      <td>0.000565</td>
      <td>0.030391</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4506</th>
      <td>1992</td>
      <td>Chris Gatling</td>
      <td>PF</td>
      <td>24.0</td>
      <td>GSW</td>
      <td>54.0</td>
      <td>612.0</td>
      <td>18.2</td>
      <td>0.602</td>
      <td>0.019</td>
      <td>0.529</td>
      <td>13.6</td>
      <td>18.8</td>
      <td>16.3</td>
      <td>3.5</td>
      <td>2.4</td>
      <td>3.5</td>
      <td>14.8</td>
      <td>19.5</td>
      <td>1.2</td>
      <td>0.8</td>
      <td>2.0</td>
      <td>0.156</td>
      <td>0.0</td>
      <td>-0.1</td>
      <td>-0.1</td>
      <td>0.3</td>
      <td>117.0</td>
      <td>206.0</td>
      <td>0.568</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.000</td>
      <td>117.0</td>
      <td>202.0</td>
      <td>0.579</td>
      <td>0.568</td>
      <td>72.0</td>
      <td>109.0</td>
      <td>0.661</td>
      <td>75.0</td>
      <td>107.0</td>
      <td>182.0</td>
      <td>16.0</td>
      <td>31.0</td>
      <td>36.0</td>
      <td>44.0</td>
      <td>101.0</td>
      <td>306.0</td>
      <td>700000.0</td>
      <td>Golden State Warriors</td>
      <td>12500000.0</td>
      <td>354154000.0</td>
      <td>0.056000</td>
      <td>0.001977</td>
      <td>0.035295</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4507</th>
      <td>1992</td>
      <td>Kenny Gattison</td>
      <td>PF</td>
      <td>27.0</td>
      <td>CHH</td>
      <td>82.0</td>
      <td>2223.0</td>
      <td>15.3</td>
      <td>0.564</td>
      <td>0.003</td>
      <td>0.357</td>
      <td>8.4</td>
      <td>19.8</td>
      <td>14.0</td>
      <td>8.2</td>
      <td>1.3</td>
      <td>1.8</td>
      <td>13.2</td>
      <td>19.4</td>
      <td>2.5</td>
      <td>1.9</td>
      <td>4.5</td>
      <td>0.097</td>
      <td>-0.6</td>
      <td>0.0</td>
      <td>-0.6</td>
      <td>0.8</td>
      <td>423.0</td>
      <td>799.0</td>
      <td>0.529</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>423.0</td>
      <td>797.0</td>
      <td>0.531</td>
      <td>0.529</td>
      <td>196.0</td>
      <td>285.0</td>
      <td>0.688</td>
      <td>177.0</td>
      <td>403.0</td>
      <td>580.0</td>
      <td>131.0</td>
      <td>59.0</td>
      <td>69.0</td>
      <td>140.0</td>
      <td>273.0</td>
      <td>1042.0</td>
      <td>456000.0</td>
      <td>Charlotte Hornets</td>
      <td>12625000.0</td>
      <td>354154000.0</td>
      <td>0.036119</td>
      <td>0.001288</td>
      <td>0.035648</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4508</th>
      <td>1992</td>
      <td>Tate George</td>
      <td>SG</td>
      <td>23.0</td>
      <td>NJN</td>
      <td>70.0</td>
      <td>1037.0</td>
      <td>11.8</td>
      <td>0.483</td>
      <td>0.016</td>
      <td>0.275</td>
      <td>3.6</td>
      <td>7.3</td>
      <td>5.4</td>
      <td>21.7</td>
      <td>1.9</td>
      <td>0.2</td>
      <td>15.9</td>
      <td>19.9</td>
      <td>0.4</td>
      <td>0.7</td>
      <td>1.1</td>
      <td>0.050</td>
      <td>-2.4</td>
      <td>-2.1</td>
      <td>-4.5</td>
      <td>-0.7</td>
      <td>165.0</td>
      <td>386.0</td>
      <td>0.427</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>0.167</td>
      <td>164.0</td>
      <td>380.0</td>
      <td>0.432</td>
      <td>0.429</td>
      <td>87.0</td>
      <td>106.0</td>
      <td>0.821</td>
      <td>36.0</td>
      <td>69.0</td>
      <td>105.0</td>
      <td>162.0</td>
      <td>41.0</td>
      <td>3.0</td>
      <td>82.0</td>
      <td>98.0</td>
      <td>418.0</td>
      <td>481000.0</td>
      <td>Brooklyn Nets</td>
      <td>12598000.0</td>
      <td>354154000.0</td>
      <td>0.038181</td>
      <td>0.001358</td>
      <td>0.035572</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4509</th>
      <td>1992</td>
      <td>Kendall Gill</td>
      <td>SG</td>
      <td>23.0</td>
      <td>CHH</td>
      <td>79.0</td>
      <td>2906.0</td>
      <td>16.5</td>
      <td>0.509</td>
      <td>0.018</td>
      <td>0.267</td>
      <td>6.0</td>
      <td>8.9</td>
      <td>7.4</td>
      <td>16.6</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>10.1</td>
      <td>24.7</td>
      <td>2.7</td>
      <td>2.0</td>
      <td>4.7</td>
      <td>0.077</td>
      <td>1.1</td>
      <td>-0.2</td>
      <td>0.9</td>
      <td>2.1</td>
      <td>666.0</td>
      <td>1427.0</td>
      <td>0.467</td>
      <td>6.0</td>
      <td>25.0</td>
      <td>0.240</td>
      <td>660.0</td>
      <td>1402.0</td>
      <td>0.471</td>
      <td>0.469</td>
      <td>284.0</td>
      <td>381.0</td>
      <td>0.745</td>
      <td>165.0</td>
      <td>237.0</td>
      <td>402.0</td>
      <td>329.0</td>
      <td>154.0</td>
      <td>46.0</td>
      <td>180.0</td>
      <td>237.0</td>
      <td>1622.0</td>
      <td>1908000.0</td>
      <td>Charlotte Hornets</td>
      <td>12625000.0</td>
      <td>354154000.0</td>
      <td>0.151129</td>
      <td>0.005387</td>
      <td>0.035648</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4510</th>
      <td>1992</td>
      <td>Armen Gilliam</td>
      <td>PF</td>
      <td>27.0</td>
      <td>PHI</td>
      <td>81.0</td>
      <td>2771.0</td>
      <td>18.0</td>
      <td>0.575</td>
      <td>0.002</td>
      <td>0.425</td>
      <td>9.7</td>
      <td>17.5</td>
      <td>13.6</td>
      <td>6.8</td>
      <td>0.9</td>
      <td>1.9</td>
      <td>12.3</td>
      <td>21.5</td>
      <td>5.3</td>
      <td>2.3</td>
      <td>7.6</td>
      <td>0.131</td>
      <td>0.7</td>
      <td>-0.6</td>
      <td>0.0</td>
      <td>1.4</td>
      <td>512.0</td>
      <td>1001.0</td>
      <td>0.511</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>512.0</td>
      <td>999.0</td>
      <td>0.513</td>
      <td>0.511</td>
      <td>343.0</td>
      <td>425.0</td>
      <td>0.807</td>
      <td>234.0</td>
      <td>426.0</td>
      <td>660.0</td>
      <td>118.0</td>
      <td>51.0</td>
      <td>85.0</td>
      <td>166.0</td>
      <td>176.0</td>
      <td>1367.0</td>
      <td>1350000.0</td>
      <td>Philadelphia 76ers</td>
      <td>14058000.0</td>
      <td>354154000.0</td>
      <td>0.096031</td>
      <td>0.003812</td>
      <td>0.039695</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4511</th>
      <td>1992</td>
      <td>Gerald Glass</td>
      <td>SF</td>
      <td>24.0</td>
      <td>MIN</td>
      <td>75.0</td>
      <td>1822.0</td>
      <td>13.2</td>
      <td>0.464</td>
      <td>0.062</td>
      <td>0.144</td>
      <td>6.3</td>
      <td>9.6</td>
      <td>7.9</td>
      <td>15.0</td>
      <td>1.8</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>24.0</td>
      <td>-0.4</td>
      <td>0.7</td>
      <td>0.3</td>
      <td>0.008</td>
      <td>-0.9</td>
      <td>-1.7</td>
      <td>-2.6</td>
      <td>-0.3</td>
      <td>383.0</td>
      <td>871.0</td>
      <td>0.440</td>
      <td>16.0</td>
      <td>54.0</td>
      <td>0.296</td>
      <td>367.0</td>
      <td>817.0</td>
      <td>0.449</td>
      <td>0.449</td>
      <td>77.0</td>
      <td>125.0</td>
      <td>0.616</td>
      <td>107.0</td>
      <td>153.0</td>
      <td>260.0</td>
      <td>175.0</td>
      <td>66.0</td>
      <td>30.0</td>
      <td>103.0</td>
      <td>171.0</td>
      <td>859.0</td>
      <td>705000.0</td>
      <td>Minnesota Timberwolves</td>
      <td>10763000.0</td>
      <td>354154000.0</td>
      <td>0.065502</td>
      <td>0.001991</td>
      <td>0.030391</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4512</th>
      <td>1992</td>
      <td>Mike Gminski</td>
      <td>C</td>
      <td>32.0</td>
      <td>CHH</td>
      <td>35.0</td>
      <td>499.0</td>
      <td>13.0</td>
      <td>0.478</td>
      <td>0.015</td>
      <td>0.141</td>
      <td>7.8</td>
      <td>17.8</td>
      <td>12.7</td>
      <td>8.5</td>
      <td>1.0</td>
      <td>1.9</td>
      <td>8.6</td>
      <td>18.7</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.5</td>
      <td>0.051</td>
      <td>-2.9</td>
      <td>-1.0</td>
      <td>-3.9</td>
      <td>-0.2</td>
      <td>90.0</td>
      <td>199.0</td>
      <td>0.452</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>89.0</td>
      <td>196.0</td>
      <td>0.454</td>
      <td>0.455</td>
      <td>21.0</td>
      <td>28.0</td>
      <td>0.750</td>
      <td>37.0</td>
      <td>81.0</td>
      <td>118.0</td>
      <td>31.0</td>
      <td>11.0</td>
      <td>16.0</td>
      <td>20.0</td>
      <td>37.0</td>
      <td>202.0</td>
      <td>1850000.0</td>
      <td>Charlotte Hornets</td>
      <td>12625000.0</td>
      <td>354154000.0</td>
      <td>0.146535</td>
      <td>0.005224</td>
      <td>0.035648</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4513</th>
      <td>1992</td>
      <td>Dan Godfread</td>
      <td>C</td>
      <td>24.0</td>
      <td>HOU</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-0.005</td>
      <td>-6.8</td>
      <td>0.1</td>
      <td>-6.7</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>100000.0</td>
      <td>Minnesota Timberwolves</td>
      <td>10763000.0</td>
      <td>354154000.0</td>
      <td>0.009291</td>
      <td>0.000282</td>
      <td>0.030391</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4514</th>
      <td>1992</td>
      <td>Paul Graham</td>
      <td>SG</td>
      <td>24.0</td>
      <td>ATL</td>
      <td>78.0</td>
      <td>1718.0</td>
      <td>14.7</td>
      <td>0.523</td>
      <td>0.207</td>
      <td>0.249</td>
      <td>4.4</td>
      <td>10.1</td>
      <td>7.1</td>
      <td>14.6</td>
      <td>2.7</td>
      <td>0.7</td>
      <td>10.7</td>
      <td>20.4</td>
      <td>1.9</td>
      <td>1.7</td>
      <td>3.6</td>
      <td>0.100</td>
      <td>0.6</td>
      <td>-0.1</td>
      <td>0.5</td>
      <td>1.1</td>
      <td>305.0</td>
      <td>682.0</td>
      <td>0.447</td>
      <td>55.0</td>
      <td>141.0</td>
      <td>0.390</td>
      <td>250.0</td>
      <td>541.0</td>
      <td>0.462</td>
      <td>0.488</td>
      <td>126.0</td>
      <td>170.0</td>
      <td>0.741</td>
      <td>72.0</td>
      <td>159.0</td>
      <td>231.0</td>
      <td>175.0</td>
      <td>96.0</td>
      <td>21.0</td>
      <td>91.0</td>
      <td>193.0</td>
      <td>791.0</td>
      <td>125000.0</td>
      <td>Atlanta Hawks</td>
      <td>12930000.0</td>
      <td>354154000.0</td>
      <td>0.009667</td>
      <td>0.000353</td>
      <td>0.036510</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4516</th>
      <td>1992</td>
      <td>Gary Grant</td>
      <td>PG</td>
      <td>26.0</td>
      <td>LAC</td>
      <td>78.0</td>
      <td>2049.0</td>
      <td>13.4</td>
      <td>0.492</td>
      <td>0.086</td>
      <td>0.091</td>
      <td>1.8</td>
      <td>8.2</td>
      <td>5.0</td>
      <td>37.0</td>
      <td>3.3</td>
      <td>0.4</td>
      <td>23.2</td>
      <td>16.7</td>
      <td>0.0</td>
      <td>2.9</td>
      <td>3.0</td>
      <td>0.069</td>
      <td>-1.7</td>
      <td>0.1</td>
      <td>-1.6</td>
      <td>0.2</td>
      <td>275.0</td>
      <td>595.0</td>
      <td>0.462</td>
      <td>15.0</td>
      <td>51.0</td>
      <td>0.294</td>
      <td>260.0</td>
      <td>544.0</td>
      <td>0.478</td>
      <td>0.475</td>
      <td>44.0</td>
      <td>54.0</td>
      <td>0.815</td>
      <td>34.0</td>
      <td>150.0</td>
      <td>184.0</td>
      <td>538.0</td>
      <td>138.0</td>
      <td>14.0</td>
      <td>187.0</td>
      <td>181.0</td>
      <td>609.0</td>
      <td>928000.0</td>
      <td>Los Angeles Clippers</td>
      <td>12976000.0</td>
      <td>354154000.0</td>
      <td>0.071517</td>
      <td>0.002620</td>
      <td>0.036639</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4520</th>
      <td>1992</td>
      <td>Harvey Grant</td>
      <td>SF</td>
      <td>26.0</td>
      <td>WSB</td>
      <td>64.0</td>
      <td>2388.0</td>
      <td>15.3</td>
      <td>0.516</td>
      <td>0.008</td>
      <td>0.215</td>
      <td>7.0</td>
      <td>12.9</td>
      <td>9.9</td>
      <td>11.1</td>
      <td>1.5</td>
      <td>0.7</td>
      <td>8.9</td>
      <td>21.7</td>
      <td>2.3</td>
      <td>2.0</td>
      <td>4.3</td>
      <td>0.087</td>
      <td>0.0</td>
      <td>-0.3</td>
      <td>-0.2</td>
      <td>1.1</td>
      <td>489.0</td>
      <td>1022.0</td>
      <td>0.478</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>0.125</td>
      <td>488.0</td>
      <td>1014.0</td>
      <td>0.481</td>
      <td>0.479</td>
      <td>176.0</td>
      <td>220.0</td>
      <td>0.800</td>
      <td>157.0</td>
      <td>275.0</td>
      <td>432.0</td>
      <td>170.0</td>
      <td>74.0</td>
      <td>27.0</td>
      <td>109.0</td>
      <td>178.0</td>
      <td>1155.0</td>
      <td>450000.0</td>
      <td>Washington Wizards</td>
      <td>12633000.0</td>
      <td>354154000.0</td>
      <td>0.035621</td>
      <td>0.001271</td>
      <td>0.035671</td>
      <td>South</td>
    </tr>
    <tr>
      <th>4521</th>
      <td>1992</td>
      <td>Horace Grant</td>
      <td>PF</td>
      <td>26.0</td>
      <td>CHI</td>
      <td>81.0</td>
      <td>2859.0</td>
      <td>20.6</td>
      <td>0.618</td>
      <td>0.003</td>
      <td>0.401</td>
      <td>14.3</td>
      <td>18.2</td>
      <td>16.3</td>
      <td>10.0</td>
      <td>1.8</td>
      <td>2.9</td>
      <td>9.5</td>
      <td>15.5</td>
      <td>9.1</td>
      <td>5.0</td>
      <td>14.1</td>
      <td>0.237</td>
      <td>4.3</td>
      <td>3.0</td>
      <td>7.3</td>
      <td>6.7</td>
      <td>457.0</td>
      <td>790.0</td>
      <td>0.578</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.000</td>
      <td>457.0</td>
      <td>788.0</td>
      <td>0.580</td>
      <td>0.578</td>
      <td>235.0</td>
      <td>317.0</td>
      <td>0.741</td>
      <td>344.0</td>
      <td>463.0</td>
      <td>807.0</td>
      <td>217.0</td>
      <td>100.0</td>
      <td>131.0</td>
      <td>98.0</td>
      <td>196.0</td>
      <td>1149.0</td>
      <td>1750000.0</td>
      <td>Chicago Bulls</td>
      <td>16829000.0</td>
      <td>354154000.0</td>
      <td>0.103987</td>
      <td>0.004941</td>
      <td>0.047519</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4522</th>
      <td>1992</td>
      <td>Jeff Grayer</td>
      <td>SG</td>
      <td>26.0</td>
      <td>MIL</td>
      <td>82.0</td>
      <td>1659.0</td>
      <td>13.3</td>
      <td>0.489</td>
      <td>0.096</td>
      <td>0.222</td>
      <td>8.5</td>
      <td>9.2</td>
      <td>8.9</td>
      <td>13.9</td>
      <td>1.9</td>
      <td>0.5</td>
      <td>12.2</td>
      <td>21.7</td>
      <td>1.0</td>
      <td>0.8</td>
      <td>1.8</td>
      <td>0.052</td>
      <td>-0.4</td>
      <td>-1.3</td>
      <td>-1.7</td>
      <td>0.1</td>
      <td>309.0</td>
      <td>689.0</td>
      <td>0.448</td>
      <td>19.0</td>
      <td>66.0</td>
      <td>0.288</td>
      <td>290.0</td>
      <td>623.0</td>
      <td>0.465</td>
      <td>0.462</td>
      <td>102.0</td>
      <td>153.0</td>
      <td>0.667</td>
      <td>129.0</td>
      <td>128.0</td>
      <td>257.0</td>
      <td>150.0</td>
      <td>64.0</td>
      <td>13.0</td>
      <td>105.0</td>
      <td>142.0</td>
      <td>739.0</td>
      <td>950000.0</td>
      <td>Milwaukee Bucks</td>
      <td>12451000.0</td>
      <td>354154000.0</td>
      <td>0.076299</td>
      <td>0.002682</td>
      <td>0.035157</td>
      <td>Midwest</td>
    </tr>
    <tr>
      <th>4523</th>
      <td>1992</td>
      <td>A.C. Green</td>
      <td>PF</td>
      <td>28.0</td>
      <td>LAL</td>
      <td>82.0</td>
      <td>2902.0</td>
      <td>16.7</td>
      <td>0.556</td>
      <td>0.070</td>
      <td>0.569</td>
      <td>11.8</td>
      <td>18.2</td>
      <td>14.9</td>
      <td>6.0</td>
      <td>1.6</td>
      <td>0.8</td>
      <td>10.0</td>
      <td>16.8</td>
      <td>5.8</td>
      <td>2.9</td>
      <td>8.8</td>
      <td>0.145</td>
      <td>1.2</td>
      <td>0.2</td>
      <td>1.4</td>
      <td>2.5</td>
      <td>382.0</td>
      <td>803.0</td>
      <td>0.476</td>
      <td>12.0</td>
      <td>56.0</td>
      <td>0.214</td>
      <td>370.0</td>
      <td>747.0</td>
      <td>0.495</td>
      <td>0.483</td>
      <td>340.0</td>
      <td>457.0</td>
      <td>0.744</td>
      <td>306.0</td>
      <td>456.0</td>
      <td>762.0</td>
      <td>117.0</td>
      <td>91.0</td>
      <td>36.0</td>
      <td>111.0</td>
      <td>141.0</td>
      <td>1116.0</td>
      <td>1750000.0</td>
      <td>Los Angeles Lakers</td>
      <td>15673000.0</td>
      <td>354154000.0</td>
      <td>0.111657</td>
      <td>0.004941</td>
      <td>0.044255</td>
      <td>West</td>
    </tr>
    <tr>
      <th>4524</th>
      <td>1992</td>
      <td>Rickey Green</td>
      <td>PG</td>
      <td>37.0</td>
      <td>BOS</td>
      <td>26.0</td>
      <td>367.0</td>
      <td>11.4</td>
      <td>0.478</td>
      <td>0.039</td>
      <td>0.175</td>
      <td>0.9</td>
      <td>6.0</td>
      <td>3.6</td>
      <td>24.1</td>
      <td>2.3</td>
      <td>0.2</td>
      <td>14.0</td>
      <td>15.1</td>
      <td>0.2</td>
      <td>0.3</td>
      <td>0.5</td>
      <td>0.068</td>
      <td>-2.1</td>
      <td>-1.2</td>
      <td>-3.3</td>
      <td>-0.1</td>
      <td>46.0</td>
      <td>103.0</td>
      <td>0.447</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.250</td>
      <td>45.0</td>
      <td>99.0</td>
      <td>0.455</td>
      <td>0.451</td>
      <td>13.0</td>
      <td>18.0</td>
      <td>0.722</td>
      <td>3.0</td>
      <td>21.0</td>
      <td>24.0</td>
      <td>68.0</td>
      <td>17.0</td>
      <td>1.0</td>
      <td>18.0</td>
      <td>28.0</td>
      <td>106.0</td>
      <td>315000.0</td>
      <td>Boston Celtic</td>
      <td>25343000.0</td>
      <td>354154000.0</td>
      <td>0.012429</td>
      <td>0.000889</td>
      <td>0.071559</td>
      <td>Northeast</td>
    </tr>
    <tr>
      <th>4525</th>
      <td>1992</td>
      <td>Rickey Green</td>
      <td>PG</td>
      <td>37.0</td>
      <td>BOS</td>
      <td>26.0</td>
      <td>367.0</td>
      <td>11.4</td>
      <td>0.478</td>
      <td>0.039</td>
      <td>0.175</td>
      <td>0.9</td>
      <td>6.0</td>
      <td>3.6</td>
      <td>24.1</td>
      <td>2.3</td>
      <td>0.2</td>
      <td>14.0</td>
      <td>15.1</td>
      <td>0.2</td>
      <td>0.3</td>
      <td>0.5</td>
      <td>0.068</td>
      <td>-2.1</td>
      <td>-1.2</td>
      <td>-3.3</td>
      <td>-0.1</td>
      <td>46.0</td>
      <td>103.0</td>
      <td>0.447</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.250</td>
      <td>45.0</td>
      <td>99.0</td>
      <td>0.455</td>
      <td>0.451</td>
      <td>13.0</td>
      <td>18.0</td>
      <td>0.722</td>
      <td>3.0</td>
      <td>21.0</td>
      <td>24.0</td>
      <td>68.0</td>
      <td>17.0</td>
      <td>1.0</td>
      <td>18.0</td>
      <td>28.0</td>
      <td>106.0</td>
      <td>130000.0</td>
      <td>Philadelphia 76ers</td>
      <td>14058000.0</td>
      <td>354154000.0</td>
      <td>0.009247</td>
      <td>0.000367</td>
      <td>0.039695</td>
      <td>Northeast</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 57 columns</p>
</div>




```
# Small nitpicking to just remove the extra team column.

data['player_stats'] = data['player_stats'].drop(columns=['Tm'])
```


```
data['player_stats'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3912</th>
      <td>1991</td>
      <td>Alaa Abdelnaby</td>
      <td>PF</td>
      <td>22.0</td>
      <td>43.0</td>
      <td>290.0</td>
      <td>13.1</td>
      <td>0.499</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>10.4</td>
      <td>23.4</td>
      <td>17.0</td>
      <td>5.8</td>
      <td>0.7</td>
      <td>2.5</td>
      <td>14.0</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.079</td>
      <td>-4.2</td>
      <td>-0.7</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.474</td>
      <td>25.0</td>
      <td>44.0</td>
      <td>0.568</td>
      <td>27.0</td>
      <td>62.0</td>
      <td>89.0</td>
      <td>12.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>22.0</td>
      <td>39.0</td>
      <td>135.0</td>
      <td>395000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.035221</td>
      <td>0.001346</td>
      <td>0.038203</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3913</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>PG</td>
      <td>21.0</td>
      <td>67.0</td>
      <td>1505.0</td>
      <td>12.2</td>
      <td>0.448</td>
      <td>0.099</td>
      <td>0.097</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>3.8</td>
      <td>19.2</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>27.2</td>
      <td>-0.7</td>
      <td>-0.3</td>
      <td>-1.0</td>
      <td>-0.031</td>
      <td>-1.7</td>
      <td>-4.4</td>
      <td>-6.1</td>
      <td>-1.6</td>
      <td>417.0</td>
      <td>1009.0</td>
      <td>0.413</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>0.240</td>
      <td>393.0</td>
      <td>909.0</td>
      <td>0.432</td>
      <td>0.425</td>
      <td>84.0</td>
      <td>98.0</td>
      <td>0.857</td>
      <td>34.0</td>
      <td>87.0</td>
      <td>121.0</td>
      <td>206.0</td>
      <td>55.0</td>
      <td>4.0</td>
      <td>110.0</td>
      <td>149.0</td>
      <td>942.0</td>
      <td>1660000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.160619</td>
      <td>0.005655</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3914</th>
      <td>1991</td>
      <td>Mark Acres</td>
      <td>C</td>
      <td>28.0</td>
      <td>68.0</td>
      <td>1313.0</td>
      <td>9.2</td>
      <td>0.551</td>
      <td>0.014</td>
      <td>0.472</td>
      <td>11.3</td>
      <td>18.7</td>
      <td>14.9</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>14.0</td>
      <td>9.3</td>
      <td>1.4</td>
      <td>1.1</td>
      <td>2.5</td>
      <td>0.090</td>
      <td>-2.1</td>
      <td>0.3</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>109.0</td>
      <td>214.0</td>
      <td>0.509</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>108.0</td>
      <td>211.0</td>
      <td>0.512</td>
      <td>0.512</td>
      <td>66.0</td>
      <td>101.0</td>
      <td>0.653</td>
      <td>140.0</td>
      <td>219.0</td>
      <td>359.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>42.0</td>
      <td>218.0</td>
      <td>285.0</td>
      <td>437000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.058019</td>
      <td>0.001489</td>
      <td>0.025657</td>
      <td>South</td>
    </tr>
    <tr>
      <th>3915</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>PG</td>
      <td>28.0</td>
      <td>66.0</td>
      <td>2346.0</td>
      <td>22.3</td>
      <td>0.530</td>
      <td>0.397</td>
      <td>0.372</td>
      <td>2.1</td>
      <td>8.8</td>
      <td>5.2</td>
      <td>39.4</td>
      <td>2.6</td>
      <td>0.1</td>
      <td>12.7</td>
      <td>28.5</td>
      <td>5.8</td>
      <td>0.4</td>
      <td>6.3</td>
      <td>0.128</td>
      <td>7.1</td>
      <td>-2.7</td>
      <td>4.4</td>
      <td>3.8</td>
      <td>560.0</td>
      <td>1421.0</td>
      <td>0.394</td>
      <td>167.0</td>
      <td>564.0</td>
      <td>0.296</td>
      <td>393.0</td>
      <td>857.0</td>
      <td>0.459</td>
      <td>0.453</td>
      <td>465.0</td>
      <td>529.0</td>
      <td>0.879</td>
      <td>58.0</td>
      <td>198.0</td>
      <td>256.0</td>
      <td>693.0</td>
      <td>147.0</td>
      <td>6.0</td>
      <td>240.0</td>
      <td>162.0</td>
      <td>1752.0</td>
      <td>825000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.079826</td>
      <td>0.002810</td>
      <td>0.035205</td>
      <td>West</td>
    </tr>
    <tr>
      <th>3916</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>SF</td>
      <td>31.0</td>
      <td>78.0</td>
      <td>2006.0</td>
      <td>16.7</td>
      <td>0.526</td>
      <td>0.086</td>
      <td>0.349</td>
      <td>7.6</td>
      <td>13.7</td>
      <td>10.7</td>
      <td>11.6</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>10.9</td>
      <td>25.7</td>
      <td>2.8</td>
      <td>2.7</td>
      <td>5.5</td>
      <td>0.132</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>0.7</td>
      <td>1.4</td>
      <td>420.0</td>
      <td>909.0</td>
      <td>0.462</td>
      <td>24.0</td>
      <td>78.0</td>
      <td>0.308</td>
      <td>396.0</td>
      <td>831.0</td>
      <td>0.477</td>
      <td>0.475</td>
      <td>240.0</td>
      <td>317.0</td>
      <td>0.757</td>
      <td>134.0</td>
      <td>240.0</td>
      <td>374.0</td>
      <td>139.0</td>
      <td>47.0</td>
      <td>20.0</td>
      <td>128.0</td>
      <td>209.0</td>
      <td>1104.0</td>
      <td>1115000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.086367</td>
      <td>0.003798</td>
      <td>0.043977</td>
      <td>Midwest</td>
    </tr>
  </tbody>
</table>
</div>



## Dealing with Categorical Variables

We've got a few variables that are not numerical and thus cannot be directly worked with in any machine learning algorithm that works with continuous data. 

The variables we have to turn into dummy variables are Position, US Region, and Team




```
# Creating dummy variables for Position first
positions = pd.get_dummies(data['player_stats']['Pos'])
data['player_stats'] = pd.concat([data['player_stats'], positions], axis=1)
```


```
data['player_stats'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
      <th>C</th>
      <th>C-PF</th>
      <th>C-SF</th>
      <th>PF</th>
      <th>PF-C</th>
      <th>PF-SF</th>
      <th>PG</th>
      <th>PG-SF</th>
      <th>PG-SG</th>
      <th>SF</th>
      <th>SF-PF</th>
      <th>SF-SG</th>
      <th>SG</th>
      <th>SG-PF</th>
      <th>SG-PG</th>
      <th>SG-SF</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3912</th>
      <td>1991</td>
      <td>Alaa Abdelnaby</td>
      <td>PF</td>
      <td>22.0</td>
      <td>43.0</td>
      <td>290.0</td>
      <td>13.1</td>
      <td>0.499</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>10.4</td>
      <td>23.4</td>
      <td>17.0</td>
      <td>5.8</td>
      <td>0.7</td>
      <td>2.5</td>
      <td>14.0</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.079</td>
      <td>-4.2</td>
      <td>-0.7</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.474</td>
      <td>25.0</td>
      <td>44.0</td>
      <td>0.568</td>
      <td>27.0</td>
      <td>62.0</td>
      <td>89.0</td>
      <td>12.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>22.0</td>
      <td>39.0</td>
      <td>135.0</td>
      <td>395000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.035221</td>
      <td>0.001346</td>
      <td>0.038203</td>
      <td>West</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3913</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>PG</td>
      <td>21.0</td>
      <td>67.0</td>
      <td>1505.0</td>
      <td>12.2</td>
      <td>0.448</td>
      <td>0.099</td>
      <td>0.097</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>3.8</td>
      <td>19.2</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>27.2</td>
      <td>-0.7</td>
      <td>-0.3</td>
      <td>-1.0</td>
      <td>-0.031</td>
      <td>-1.7</td>
      <td>-4.4</td>
      <td>-6.1</td>
      <td>-1.6</td>
      <td>417.0</td>
      <td>1009.0</td>
      <td>0.413</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>0.240</td>
      <td>393.0</td>
      <td>909.0</td>
      <td>0.432</td>
      <td>0.425</td>
      <td>84.0</td>
      <td>98.0</td>
      <td>0.857</td>
      <td>34.0</td>
      <td>87.0</td>
      <td>121.0</td>
      <td>206.0</td>
      <td>55.0</td>
      <td>4.0</td>
      <td>110.0</td>
      <td>149.0</td>
      <td>942.0</td>
      <td>1660000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.160619</td>
      <td>0.005655</td>
      <td>0.035205</td>
      <td>West</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3914</th>
      <td>1991</td>
      <td>Mark Acres</td>
      <td>C</td>
      <td>28.0</td>
      <td>68.0</td>
      <td>1313.0</td>
      <td>9.2</td>
      <td>0.551</td>
      <td>0.014</td>
      <td>0.472</td>
      <td>11.3</td>
      <td>18.7</td>
      <td>14.9</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>14.0</td>
      <td>9.3</td>
      <td>1.4</td>
      <td>1.1</td>
      <td>2.5</td>
      <td>0.090</td>
      <td>-2.1</td>
      <td>0.3</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>109.0</td>
      <td>214.0</td>
      <td>0.509</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>108.0</td>
      <td>211.0</td>
      <td>0.512</td>
      <td>0.512</td>
      <td>66.0</td>
      <td>101.0</td>
      <td>0.653</td>
      <td>140.0</td>
      <td>219.0</td>
      <td>359.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>42.0</td>
      <td>218.0</td>
      <td>285.0</td>
      <td>437000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.058019</td>
      <td>0.001489</td>
      <td>0.025657</td>
      <td>South</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3915</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>PG</td>
      <td>28.0</td>
      <td>66.0</td>
      <td>2346.0</td>
      <td>22.3</td>
      <td>0.530</td>
      <td>0.397</td>
      <td>0.372</td>
      <td>2.1</td>
      <td>8.8</td>
      <td>5.2</td>
      <td>39.4</td>
      <td>2.6</td>
      <td>0.1</td>
      <td>12.7</td>
      <td>28.5</td>
      <td>5.8</td>
      <td>0.4</td>
      <td>6.3</td>
      <td>0.128</td>
      <td>7.1</td>
      <td>-2.7</td>
      <td>4.4</td>
      <td>3.8</td>
      <td>560.0</td>
      <td>1421.0</td>
      <td>0.394</td>
      <td>167.0</td>
      <td>564.0</td>
      <td>0.296</td>
      <td>393.0</td>
      <td>857.0</td>
      <td>0.459</td>
      <td>0.453</td>
      <td>465.0</td>
      <td>529.0</td>
      <td>0.879</td>
      <td>58.0</td>
      <td>198.0</td>
      <td>256.0</td>
      <td>693.0</td>
      <td>147.0</td>
      <td>6.0</td>
      <td>240.0</td>
      <td>162.0</td>
      <td>1752.0</td>
      <td>825000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.079826</td>
      <td>0.002810</td>
      <td>0.035205</td>
      <td>West</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3916</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>SF</td>
      <td>31.0</td>
      <td>78.0</td>
      <td>2006.0</td>
      <td>16.7</td>
      <td>0.526</td>
      <td>0.086</td>
      <td>0.349</td>
      <td>7.6</td>
      <td>13.7</td>
      <td>10.7</td>
      <td>11.6</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>10.9</td>
      <td>25.7</td>
      <td>2.8</td>
      <td>2.7</td>
      <td>5.5</td>
      <td>0.132</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>0.7</td>
      <td>1.4</td>
      <td>420.0</td>
      <td>909.0</td>
      <td>0.462</td>
      <td>24.0</td>
      <td>78.0</td>
      <td>0.308</td>
      <td>396.0</td>
      <td>831.0</td>
      <td>0.477</td>
      <td>0.475</td>
      <td>240.0</td>
      <td>317.0</td>
      <td>0.757</td>
      <td>134.0</td>
      <td>240.0</td>
      <td>374.0</td>
      <td>139.0</td>
      <td>47.0</td>
      <td>20.0</td>
      <td>128.0</td>
      <td>209.0</td>
      <td>1104.0</td>
      <td>1115000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.086367</td>
      <td>0.003798</td>
      <td>0.043977</td>
      <td>Midwest</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```
data['player_stats']['Pos'].unique()
```




    array(['PF', 'PG', 'C', 'SF', 'SG', 'SG-SF', 'SF-SG', 'PG-SG', 'SG-PG',
           'PF-SF', 'PF-C', 'SF-PF', 'C-PF', 'PG-SF', 'C-SF', 'SG-PF'],
          dtype=object)



Something very interesting that comes up for positions is that some players play multiple positions in their careers, so the dummy variables reflected that. Perhaps that might actually make a difference in the analysis?


```
# Creating dummy variables for US Region
regions = pd.get_dummies(data['player_stats']['US Region'])
data['player_stats'] = pd.concat([data['player_stats'], regions], axis=1)
```


```
data['player_stats'].head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
      <th>C</th>
      <th>C-PF</th>
      <th>C-SF</th>
      <th>PF</th>
      <th>PF-C</th>
      <th>PF-SF</th>
      <th>PG</th>
      <th>PG-SF</th>
      <th>PG-SG</th>
      <th>SF</th>
      <th>SF-PF</th>
      <th>SF-SG</th>
      <th>SG</th>
      <th>SG-PF</th>
      <th>SG-PG</th>
      <th>SG-SF</th>
      <th>Midwest</th>
      <th>Northeast</th>
      <th>South</th>
      <th>West</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3912</th>
      <td>1991</td>
      <td>Alaa Abdelnaby</td>
      <td>PF</td>
      <td>22.0</td>
      <td>43.0</td>
      <td>290.0</td>
      <td>13.1</td>
      <td>0.499</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>10.4</td>
      <td>23.4</td>
      <td>17.0</td>
      <td>5.8</td>
      <td>0.7</td>
      <td>2.5</td>
      <td>14.0</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.079</td>
      <td>-4.2</td>
      <td>-0.7</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.474</td>
      <td>25.0</td>
      <td>44.0</td>
      <td>0.568</td>
      <td>27.0</td>
      <td>62.0</td>
      <td>89.0</td>
      <td>12.0</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>22.0</td>
      <td>39.0</td>
      <td>135.0</td>
      <td>395000.0</td>
      <td>Portland Trail Blazers</td>
      <td>11215000.0</td>
      <td>293563000.0</td>
      <td>0.035221</td>
      <td>0.001346</td>
      <td>0.038203</td>
      <td>West</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3913</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>PG</td>
      <td>21.0</td>
      <td>67.0</td>
      <td>1505.0</td>
      <td>12.2</td>
      <td>0.448</td>
      <td>0.099</td>
      <td>0.097</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>3.8</td>
      <td>19.2</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>27.2</td>
      <td>-0.7</td>
      <td>-0.3</td>
      <td>-1.0</td>
      <td>-0.031</td>
      <td>-1.7</td>
      <td>-4.4</td>
      <td>-6.1</td>
      <td>-1.6</td>
      <td>417.0</td>
      <td>1009.0</td>
      <td>0.413</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>0.240</td>
      <td>393.0</td>
      <td>909.0</td>
      <td>0.432</td>
      <td>0.425</td>
      <td>84.0</td>
      <td>98.0</td>
      <td>0.857</td>
      <td>34.0</td>
      <td>87.0</td>
      <td>121.0</td>
      <td>206.0</td>
      <td>55.0</td>
      <td>4.0</td>
      <td>110.0</td>
      <td>149.0</td>
      <td>942.0</td>
      <td>1660000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.160619</td>
      <td>0.005655</td>
      <td>0.035205</td>
      <td>West</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3914</th>
      <td>1991</td>
      <td>Mark Acres</td>
      <td>C</td>
      <td>28.0</td>
      <td>68.0</td>
      <td>1313.0</td>
      <td>9.2</td>
      <td>0.551</td>
      <td>0.014</td>
      <td>0.472</td>
      <td>11.3</td>
      <td>18.7</td>
      <td>14.9</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>14.0</td>
      <td>9.3</td>
      <td>1.4</td>
      <td>1.1</td>
      <td>2.5</td>
      <td>0.090</td>
      <td>-2.1</td>
      <td>0.3</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>109.0</td>
      <td>214.0</td>
      <td>0.509</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>108.0</td>
      <td>211.0</td>
      <td>0.512</td>
      <td>0.512</td>
      <td>66.0</td>
      <td>101.0</td>
      <td>0.653</td>
      <td>140.0</td>
      <td>219.0</td>
      <td>359.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>42.0</td>
      <td>218.0</td>
      <td>285.0</td>
      <td>437000.0</td>
      <td>Orlando Magic</td>
      <td>7532000.0</td>
      <td>293563000.0</td>
      <td>0.058019</td>
      <td>0.001489</td>
      <td>0.025657</td>
      <td>South</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3915</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>PG</td>
      <td>28.0</td>
      <td>66.0</td>
      <td>2346.0</td>
      <td>22.3</td>
      <td>0.530</td>
      <td>0.397</td>
      <td>0.372</td>
      <td>2.1</td>
      <td>8.8</td>
      <td>5.2</td>
      <td>39.4</td>
      <td>2.6</td>
      <td>0.1</td>
      <td>12.7</td>
      <td>28.5</td>
      <td>5.8</td>
      <td>0.4</td>
      <td>6.3</td>
      <td>0.128</td>
      <td>7.1</td>
      <td>-2.7</td>
      <td>4.4</td>
      <td>3.8</td>
      <td>560.0</td>
      <td>1421.0</td>
      <td>0.394</td>
      <td>167.0</td>
      <td>564.0</td>
      <td>0.296</td>
      <td>393.0</td>
      <td>857.0</td>
      <td>0.459</td>
      <td>0.453</td>
      <td>465.0</td>
      <td>529.0</td>
      <td>0.879</td>
      <td>58.0</td>
      <td>198.0</td>
      <td>256.0</td>
      <td>693.0</td>
      <td>147.0</td>
      <td>6.0</td>
      <td>240.0</td>
      <td>162.0</td>
      <td>1752.0</td>
      <td>825000.0</td>
      <td>Denver Nuggets</td>
      <td>10335000.0</td>
      <td>293563000.0</td>
      <td>0.079826</td>
      <td>0.002810</td>
      <td>0.035205</td>
      <td>West</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3916</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>SF</td>
      <td>31.0</td>
      <td>78.0</td>
      <td>2006.0</td>
      <td>16.7</td>
      <td>0.526</td>
      <td>0.086</td>
      <td>0.349</td>
      <td>7.6</td>
      <td>13.7</td>
      <td>10.7</td>
      <td>11.6</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>10.9</td>
      <td>25.7</td>
      <td>2.8</td>
      <td>2.7</td>
      <td>5.5</td>
      <td>0.132</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>0.7</td>
      <td>1.4</td>
      <td>420.0</td>
      <td>909.0</td>
      <td>0.462</td>
      <td>24.0</td>
      <td>78.0</td>
      <td>0.308</td>
      <td>396.0</td>
      <td>831.0</td>
      <td>0.477</td>
      <td>0.475</td>
      <td>240.0</td>
      <td>317.0</td>
      <td>0.757</td>
      <td>134.0</td>
      <td>240.0</td>
      <td>374.0</td>
      <td>139.0</td>
      <td>47.0</td>
      <td>20.0</td>
      <td>128.0</td>
      <td>209.0</td>
      <td>1104.0</td>
      <td>1115000.0</td>
      <td>Detroit Pistons</td>
      <td>12910000.0</td>
      <td>293563000.0</td>
      <td>0.086367</td>
      <td>0.003798</td>
      <td>0.043977</td>
      <td>Midwest</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```
# Creating dummy variables for Team
positions = pd.get_dummies(data['player_stats']['Team'])
data['player_stats'] = pd.concat([data['player_stats'], positions], axis=1)
```


```
data['player_stats'].head()

# There are a LOT of dummy variables for 'Team'
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>...</th>
      <th>SF-PF</th>
      <th>SF-SG</th>
      <th>SG</th>
      <th>SG-PF</th>
      <th>SG-PG</th>
      <th>SG-SF</th>
      <th>Midwest</th>
      <th>Northeast</th>
      <th>South</th>
      <th>West</th>
      <th>Atlanta Hawks</th>
      <th>Boston Celtic</th>
      <th>Brooklyn Nets</th>
      <th>Charlotte Hornets</th>
      <th>Chicago Bulls</th>
      <th>Cleveland Caveliers</th>
      <th>Dallas Mavericks</th>
      <th>Denver Nuggets</th>
      <th>Detroit Pistons</th>
      <th>Golden State Warriors</th>
      <th>Houston Rockets</th>
      <th>Indiana Pacers</th>
      <th>Los Angeles Clippers</th>
      <th>Los Angeles Lakers</th>
      <th>Memphis Grizzlies</th>
      <th>Miami Heat</th>
      <th>Milwaukee Bucks</th>
      <th>Minnesota Timberwolves</th>
      <th>New Orleans Pelicans</th>
      <th>New York Knicks</th>
      <th>Oklahoma City Thunder</th>
      <th>Orlando Magic</th>
      <th>Philadelphia 76ers</th>
      <th>Phoenix Suns</th>
      <th>Portland Trail Blazers</th>
      <th>Sacramento Kings</th>
      <th>San Antonio Spurs</th>
      <th>Toronto Raptors</th>
      <th>Utah Jazz</th>
      <th>Washington Wizards</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3912</th>
      <td>1991</td>
      <td>Alaa Abdelnaby</td>
      <td>PF</td>
      <td>22.0</td>
      <td>43.0</td>
      <td>290.0</td>
      <td>13.1</td>
      <td>0.499</td>
      <td>0.000</td>
      <td>0.379</td>
      <td>10.4</td>
      <td>23.4</td>
      <td>17.0</td>
      <td>5.8</td>
      <td>0.7</td>
      <td>2.5</td>
      <td>14.0</td>
      <td>22.1</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.079</td>
      <td>-4.2</td>
      <td>-0.7</td>
      <td>-5.0</td>
      <td>-0.2</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>55.0</td>
      <td>116.0</td>
      <td>0.474</td>
      <td>0.474</td>
      <td>25.0</td>
      <td>44.0</td>
      <td>0.568</td>
      <td>27.0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3913</th>
      <td>1991</td>
      <td>Mahmoud Abdul-Rauf</td>
      <td>PG</td>
      <td>21.0</td>
      <td>67.0</td>
      <td>1505.0</td>
      <td>12.2</td>
      <td>0.448</td>
      <td>0.099</td>
      <td>0.097</td>
      <td>1.9</td>
      <td>6.0</td>
      <td>3.8</td>
      <td>19.2</td>
      <td>1.5</td>
      <td>0.1</td>
      <td>9.5</td>
      <td>27.2</td>
      <td>-0.7</td>
      <td>-0.3</td>
      <td>-1.0</td>
      <td>-0.031</td>
      <td>-1.7</td>
      <td>-4.4</td>
      <td>-6.1</td>
      <td>-1.6</td>
      <td>417.0</td>
      <td>1009.0</td>
      <td>0.413</td>
      <td>24.0</td>
      <td>100.0</td>
      <td>0.240</td>
      <td>393.0</td>
      <td>909.0</td>
      <td>0.432</td>
      <td>0.425</td>
      <td>84.0</td>
      <td>98.0</td>
      <td>0.857</td>
      <td>34.0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3914</th>
      <td>1991</td>
      <td>Mark Acres</td>
      <td>C</td>
      <td>28.0</td>
      <td>68.0</td>
      <td>1313.0</td>
      <td>9.2</td>
      <td>0.551</td>
      <td>0.014</td>
      <td>0.472</td>
      <td>11.3</td>
      <td>18.7</td>
      <td>14.9</td>
      <td>2.5</td>
      <td>0.9</td>
      <td>1.1</td>
      <td>14.0</td>
      <td>9.3</td>
      <td>1.4</td>
      <td>1.1</td>
      <td>2.5</td>
      <td>0.090</td>
      <td>-2.1</td>
      <td>0.3</td>
      <td>-1.8</td>
      <td>0.1</td>
      <td>109.0</td>
      <td>214.0</td>
      <td>0.509</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.333</td>
      <td>108.0</td>
      <td>211.0</td>
      <td>0.512</td>
      <td>0.512</td>
      <td>66.0</td>
      <td>101.0</td>
      <td>0.653</td>
      <td>140.0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3915</th>
      <td>1991</td>
      <td>Michael Adams</td>
      <td>PG</td>
      <td>28.0</td>
      <td>66.0</td>
      <td>2346.0</td>
      <td>22.3</td>
      <td>0.530</td>
      <td>0.397</td>
      <td>0.372</td>
      <td>2.1</td>
      <td>8.8</td>
      <td>5.2</td>
      <td>39.4</td>
      <td>2.6</td>
      <td>0.1</td>
      <td>12.7</td>
      <td>28.5</td>
      <td>5.8</td>
      <td>0.4</td>
      <td>6.3</td>
      <td>0.128</td>
      <td>7.1</td>
      <td>-2.7</td>
      <td>4.4</td>
      <td>3.8</td>
      <td>560.0</td>
      <td>1421.0</td>
      <td>0.394</td>
      <td>167.0</td>
      <td>564.0</td>
      <td>0.296</td>
      <td>393.0</td>
      <td>857.0</td>
      <td>0.459</td>
      <td>0.453</td>
      <td>465.0</td>
      <td>529.0</td>
      <td>0.879</td>
      <td>58.0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3916</th>
      <td>1991</td>
      <td>Mark Aguirre</td>
      <td>SF</td>
      <td>31.0</td>
      <td>78.0</td>
      <td>2006.0</td>
      <td>16.7</td>
      <td>0.526</td>
      <td>0.086</td>
      <td>0.349</td>
      <td>7.6</td>
      <td>13.7</td>
      <td>10.7</td>
      <td>11.6</td>
      <td>1.2</td>
      <td>0.6</td>
      <td>10.9</td>
      <td>25.7</td>
      <td>2.8</td>
      <td>2.7</td>
      <td>5.5</td>
      <td>0.132</td>
      <td>0.9</td>
      <td>-0.3</td>
      <td>0.7</td>
      <td>1.4</td>
      <td>420.0</td>
      <td>909.0</td>
      <td>0.462</td>
      <td>24.0</td>
      <td>78.0</td>
      <td>0.308</td>
      <td>396.0</td>
      <td>831.0</td>
      <td>0.477</td>
      <td>0.475</td>
      <td>240.0</td>
      <td>317.0</td>
      <td>0.757</td>
      <td>134.0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 106 columns</p>
</div>



## Exploratory Data Analysis

Finally, FINALLY we can explore this data and find some juicy relationships!

*NOTE: Because there are SO many variables to account for, my EDA could have been more extensive. Feel free to play around with the data in the .ipynb notebook or in the Python file!*


```
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
sns.set_palette('Spectral')
sns.set_style('whitegrid')
```

As a quick refresher, here are all of the features and what they mean.


*   'Pos' - position
*   'G' - games played
*   'MP' - minutes played
*   'PER' - player efficiency rating
*   'TS%' - true shooting percentage (weights 3-pointers higher)
*   '3PAr' - 3-point attempt rate
*   'Ftr' - free throw attempt rate
*   'ORB%' - offensive rebound percentage
*   'DRB%' - defensive rebound percentage
*   'TRB%' - total rebound percentage
*   'AST%' - assist percentage
*   'STL%' - steal percentage
*   'BLK%' - block percentage
*   'TOV%' - turnover percentage
*   'USG%' - usage rate
*   'OWS' - offensive win shares
*   'DWS' - defensive win shares
*   'WS' - win shares
*   'WS/48' - win shares over 48 minutes
*   'OBPM' - offensive box plus/minus
*   'DBPM' - defensive box plus/minus
*   'BPM' - box plus/minus
*   'VORP' - value over replacement player
*   'FG' - field goals made
*   'FGA' - field goals attempted
*   'FG%' - field goal percentage'
*   '3P' - 3-pointers made
*   '3PA' - 3-pointers attempted
*   '3P%' - 3-point percentage
*   '2P' - 2-pointers made
*   '2PA' - 2-pointers attempted
*   '2P%' - 2-point percentage'
*   'eFG%' - effective field goal percentage
*   'FT' - free throws made
*   'FTA' - free throws attempted
*   'FT%' - free throw percentage
*   'ORB' - offensive rebounds
*   'DRB' - defensive rebounds
*   'TRB' - total rebounds
*   'AST' - assists
*   'STL' - steals
*   'BLK' - blocks
*   'TOV' - turnovers
*   'PF' - personal fouls
*   'PTS' - points
*   'Salary' - the annual salary for the particular player in that particular year
*   'Team' - the team that the particular player played in for that particular year
*  'Team Payroll' - the total amount of money spent on player salaries for that particular team for that particular year
*   'Total NBA Payroll' - the total amount of money spent on player salaries for the entire NBA in that particular year
*   'Player Leverage' - the proportion of the total team payroll that particular player had in that particular year
*   'League Weight' - the proportion of the total NBA payroll that particular player had in that particular year
*   'Team Market Size' - the proportion of the total NBA payroll that particular team had in that particular year
*   'US Region' - the region in which that team is located











Let's explore some relationships between the variables


```
sns.jointplot(x='Salary', y='League Weight', data=data['player_stats'])
```




    <seaborn.axisgrid.JointGrid at 0x7f3a565069b0>




![png](NBA%20Salary%20Prediction_files/NBA%20Salary%20Prediction_77_1.png)


This may seem silly at first to plot league weight with salary because they basically are explaining the same thing, mainly, the amount of money a player is getting relative to the rest of the league. However, looking at this plot, we can see why the Salary variable might not be the best predictor variable. The near straight lines that can be inferred from the plot show that League Weight and Salary are almost perfectly correlated, but because of inflation and other time-related factors, the salary amount that would give a player a league weight of 0.01 in one year might give another player (or the same player) a completely different league weight in another year.

The time value of money is powerful and could really mess up our analysis if we're not careful.

**For now, we should focus on predicting the League Weight variable.**


```
# Comparing win shares per 48 minutes with league weight

sns.jointplot(x='WS/48', y='League Weight', data=data['player_stats'])
```




    <seaborn.axisgrid.JointGrid at 0x7f3a53343710>




![png](NBA%20Salary%20Prediction_files/NBA%20Salary%20Prediction_79_1.png)



```
# Comparing total points with league weight

sns.jointplot(x='PTS', y='League Weight', data=data['player_stats'])
```




    <seaborn.axisgrid.JointGrid at 0x7f3a530674a8>




![png](NBA%20Salary%20Prediction_files/NBA%20Salary%20Prediction_80_1.png)



```
# Comparing VORP with League Weight

sns.jointplot(x='VORP', y='League Weight', data=data['player_stats'])
```




    <seaborn.axisgrid.JointGrid at 0x7f3a53339fd0>




![png](NBA%20Salary%20Prediction_files/NBA%20Salary%20Prediction_81_1.png)



```
# Comparing USG% with League Weight

sns.jointplot(x='USG%', y='League Weight', data=data['player_stats'])
```




    <seaborn.axisgrid.JointGrid at 0x7f3a52d86780>




![png](NBA%20Salary%20Prediction_files/NBA%20Salary%20Prediction_82_1.png)



```
# Here is a strange outlier

data['player_stats'][data['player_stats']['USG%'] == data['player_stats']['USG%'].max()]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Player</th>
      <th>Pos</th>
      <th>Age</th>
      <th>G</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>3PAr</th>
      <th>FTr</th>
      <th>ORB%</th>
      <th>DRB%</th>
      <th>TRB%</th>
      <th>AST%</th>
      <th>STL%</th>
      <th>BLK%</th>
      <th>TOV%</th>
      <th>USG%</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>eFG%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Salary</th>
      <th>Team</th>
      <th>Team Payroll</th>
      <th>Total NBA Payroll</th>
      <th>Player Leverage</th>
      <th>League Weight</th>
      <th>Team Market Size</th>
      <th>US Region</th>
      <th>C</th>
      <th>C-PF</th>
      <th>C-SF</th>
      <th>PF</th>
      <th>PF-C</th>
      <th>PF-SF</th>
      <th>PG</th>
      <th>PG-SF</th>
      <th>PG-SG</th>
      <th>SF</th>
      <th>SF-PF</th>
      <th>SF-SG</th>
      <th>SG</th>
      <th>SG-PF</th>
      <th>SG-PG</th>
      <th>SG-SF</th>
      <th>Midwest</th>
      <th>Northeast</th>
      <th>South</th>
      <th>West</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8227</th>
      <td>1999</td>
      <td>Gheorghe Muresan</td>
      <td>C</td>
      <td>27.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>-90.6</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>50.0</td>
      <td>88.3</td>
      <td>-0.1</td>
      <td>0.0</td>
      <td>-0.1</td>
      <td>-2.519</td>
      <td>-73.8</td>
      <td>-12.9</td>
      <td>-86.7</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>475000.0</td>
      <td>Brooklyn Nets</td>
      <td>43735000.0</td>
      <td>884280346.0</td>
      <td>0.010861</td>
      <td>0.000537</td>
      <td>0.049458</td>
      <td>Northeast</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```
# Getting rid of this outlier
data['player_stats'] = data['player_stats'].drop(8227)
```

## Predictive Model Building

Let's get to the best part, the machine learning algorithms!

For this set of data, trying to predict a continuous variable, it might be best to use multiple regression, though we could also use decision trees or random forest.

### Train Test Split

Let's split the data into training and testing data


```
from sklearn.model_selection import train_test_split
```


```
# The predictor variable is 'League Weight', and since League Weight is calculated from salary and the related variables, I'll remove the salary-related variables

colnames = list(data['player_stats']) 

colnames.remove('Player')
colnames.remove('Pos')
colnames.remove('US Region')
colnames.remove('Salary')
colnames.remove('League Weight')
colnames.remove('Total NBA Payroll')
colnames.remove('Team Payroll')
colnames.remove('Team')
colnames.remove('Player Leverage')
print(colnames)
```

    ['Year', 'Age', 'G', 'MP', 'PER', 'TS%', '3PAr', 'FTr', 'ORB%', 'DRB%', 'TRB%', 'AST%', 'STL%', 'BLK%', 'TOV%', 'USG%', 'OWS', 'DWS', 'WS', 'WS/48', 'OBPM', 'DBPM', 'BPM', 'VORP', 'FG', 'FGA', 'FG%', '3P', '3PA', '3P%', '2P', '2PA', '2P%', 'eFG%', 'FT', 'FTA', 'FT%', 'ORB', 'DRB', 'TRB', 'AST', 'STL', 'BLK', 'TOV', 'PF', 'PTS', 'Team Market Size', 'C', 'C-PF', 'C-SF', 'PF', 'PF-C', 'PF-SF', 'PG', 'PG-SF', 'PG-SG', 'SF', 'SF-PF', 'SF-SG', 'SG', 'SG-PF', 'SG-PG', 'SG-SF', 'Midwest', 'Northeast', 'South', 'West', 'Atlanta Hawks', 'Boston Celtic', 'Brooklyn Nets', 'Charlotte Hornets', 'Chicago Bulls', 'Cleveland Caveliers', 'Dallas Mavericks', 'Denver Nuggets', 'Detroit Pistons', 'Golden State Warriors', 'Houston Rockets', 'Indiana Pacers', 'Los Angeles Clippers', 'Los Angeles Lakers', 'Memphis Grizzlies', 'Miami Heat', 'Milwaukee Bucks', 'Minnesota Timberwolves', 'New Orleans Pelicans', 'New York Knicks', 'Oklahoma City Thunder', 'Orlando Magic', 'Philadelphia 76ers', 'Phoenix Suns', 'Portland Trail Blazers', 'Sacramento Kings', 'San Antonio Spurs', 'Toronto Raptors', 'Utah Jazz', 'Washington Wizards']



```
# Here we are trying to predict the league weight of the players

y = data['player_stats']['League Weight']
X = data['player_stats'][colnames]
```


```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=101)
```

### Regression

Let's start simple for our predictive model and just use linear regression. It may be the most basic algorithm, but sometimes simple yields the best results.




```
from sklearn.linear_model import LinearRegression
```


```
lm = LinearRegression()
```


```
lm.fit(X_train, y_train) 
```




    LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)




```
# Let's take a look at our coefficients 
# WARNING: There will be a LOT

print('Coefficients: \n', lm.coef_)
```

    Coefficients: 
     [-9.76196723e-06  1.43166403e-04 -2.64390026e-05  4.19292331e-07
      2.12258418e-05 -1.88291841e-03 -9.55093847e-04 -8.77880545e-05
     -1.59768298e-04 -6.93171104e-05  1.99109563e-04 -1.17344177e-06
     -8.17010630e-05 -1.80942460e-06  1.17897051e-05  4.37056348e-05
      1.96517774e-05  1.10987216e-04  1.44281082e-04 -6.16291358e-03
      6.42692933e-06 -1.42960208e-04  2.53857634e-04 -1.08495475e-04
     -6.50561141e-07  3.83614624e-06  1.57296898e-03  1.02855293e-06
      1.47232144e-06  1.43869664e-04 -1.67911392e-06  2.36382517e-06
      2.46382150e-04 -1.88369418e-03 -3.64164256e-06  5.96844272e-06
      5.58805374e-04 -2.01733987e-06  2.09068974e-06  7.33498099e-08
      5.06220100e-07 -7.55834650e-06  5.40796711e-06  4.28476785e-06
     -1.51730935e-06  1.41398721e-04 -3.91421179e-06  3.37786450e-02
      4.51545419e-04  5.39689181e-04 -1.25295136e-04 -1.51730936e-06
      1.41398721e-04  4.23909759e-04  4.74776774e-04 -4.96687322e-04
      5.47522097e-18 -1.77256372e-04 -4.36823580e-05 -4.40738770e-05
     -3.53845311e-04 -1.47132699e-04  6.39865722e-04 -9.07915058e-04
     -3.75297443e-04 -2.73753052e-05  9.31710322e-05 -9.29485674e-05
      2.71528404e-05  4.64079868e-05  1.04502421e-04  2.30506691e-05
      1.99588603e-04 -9.70905447e-06 -8.09256539e-05 -5.75941572e-05
     -1.42690429e-04 -6.67851935e-05 -9.91325972e-05 -1.57873990e-04
     -6.20839220e-05 -7.11275310e-05  1.56133685e-04  1.44799163e-04
     -2.43816268e-04  3.92810257e-05  1.52847493e-04 -1.85563874e-05
      8.10320585e-05  1.61109423e-05  1.51430143e-04  1.54419530e-05
     -1.36069769e-04  1.56577876e-04  1.27056024e-04 -3.92155337e-04
     -1.30856069e-04  3.64055819e-05  2.18710734e-04]



```
# Which variable had the biggest impact on our model?
import numpy as np

highest = np.argmax(lm.coef_)
value = np.amax(lm.coef_)
print(value)


var = colnames[highest]
print(var)
```

    0.033778644988993016
    C


Now this is interesting! Something seemingly unimportant like what position a player plays can actually have a pretty big impact! 

Just the position of center has a coefficient of more than 3%! (Of course, this is based on the linear regression model so it might not be fully accurate)

### Predicting Test Data for Regression

Let's see how our model performed!




```
predictions = lm.predict(X_test)
```


```
# Here we'll plot our predictions versus the actual values

plt.scatter(y_test, predictions)
plt.xlabel('Y Test')
plt.ylabel('Predicted Y')
```




    Text(0, 0.5, 'Predicted Y')




![png](NBA%20Salary%20Prediction_files/NBA%20Salary%20Prediction_101_1.png)


### Evaluation of Regression Model

Let's get some concrete numbers on the performance of our model. We'll use the metrics: mean absolute error, mean squared error, and root mean squared error. 

Later we'll compare these numbers between all of the algorithms we use in this project.


```
from sklearn import metrics


print('MAE:', metrics.mean_absolute_error(y_test, predictions))
print('MSE:', metrics.mean_squared_error(y_test, predictions))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, predictions)))
```

    MAE: 0.001208361245470979
    MSE: 2.773862781586042e-06
    RMSE: 0.0016654917536829902


### Decision Tree Model

Though decision trees are mainly used for categorical data because the structure of it allows for either distinctions for non-continuous data, it can still be used here. Let's see how it stacks up!


```
from sklearn.tree import DecisionTreeRegressor
```


```
dtree = DecisionTreeRegressor()
```


```
dtree.fit(X_train, y_train)
```




    DecisionTreeRegressor(criterion='mse', max_depth=None, max_features=None,
                          max_leaf_nodes=None, min_impurity_decrease=0.0,
                          min_impurity_split=None, min_samples_leaf=1,
                          min_samples_split=2, min_weight_fraction_leaf=0.0,
                          presort=False, random_state=None, splitter='best')



### Evaluation of Decision Tree Model


```
dt_predictions = dtree.predict(X_test)

print('MAE:', metrics.mean_absolute_error(y_test, dt_predictions))
print('MSE:', metrics.mean_squared_error(y_test, dt_predictions))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, dt_predictions)))
```

    MAE: 0.001450260763597658
    MSE: 5.0184572230247925e-06
    RMSE: 0.0022401913362533996


### Random Forest Model


```
from sklearn.ensemble import RandomForestRegressor

rfr = RandomForestRegressor()
```


```
rfr.fit(X_train, y_train)
```

    /usr/local/lib/python3.6/dist-packages/sklearn/ensemble/forest.py:245: FutureWarning: The default value of n_estimators will change from 10 in version 0.20 to 100 in 0.22.
      "10 in version 0.20 to 100 in 0.22.", FutureWarning)





    RandomForestRegressor(bootstrap=True, criterion='mse', max_depth=None,
                          max_features='auto', max_leaf_nodes=None,
                          min_impurity_decrease=0.0, min_impurity_split=None,
                          min_samples_leaf=1, min_samples_split=2,
                          min_weight_fraction_leaf=0.0, n_estimators=10,
                          n_jobs=None, oob_score=False, random_state=None,
                          verbose=0, warm_start=False)



### Evaluation of Random Forest Model


```
rfr_predictions = rfr.predict(X_test)
print('MAE:', metrics.mean_absolute_error(y_test, rfr_predictions))
print('MSE:', metrics.mean_squared_error(y_test, rfr_predictions))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, rfr_predictions)))
```

    MAE: 0.0011275359959120699
    MSE: 2.6443290638954636e-06
    RMSE: 0.001626139312573023


## Comparison of Models

We have three models: linear regression, decision trees, and random forests. Let's compare all of the numbers to see which ones are most accurate.


```
print('Linear Regression: ' )
print('MAE:', metrics.mean_absolute_error(y_test, predictions))
print('MSE:', metrics.mean_squared_error(y_test, predictions))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, predictions)))
print('\n')

print('Decision Tree Model: ' )
print('MAE:', metrics.mean_absolute_error(y_test, dt_predictions))
print('MSE:', metrics.mean_squared_error(y_test, dt_predictions))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, dt_predictions)))
print('\n')

print('Random Forest Model: ')
print('MAE:', metrics.mean_absolute_error(y_test, rfr_predictions))
print('MSE:', metrics.mean_squared_error(y_test, rfr_predictions))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, rfr_predictions)))
```

    Linear Regression: 
    MAE: 0.0011196114995213812
    MSE: 2.5965861294595025e-06
    RMSE: 0.0016113926056239374
    
    
    Decision Tree Model: 
    MAE: 0.001450260763597658
    MSE: 5.0184572230247925e-06
    RMSE: 0.0022401913362533996
    
    
    Random Forest Model: 
    MAE: 0.0011275359959120699
    MSE: 2.6443290638954636e-06
    RMSE: 0.001626139312573023


Based on these numbers, it looks like linear regression actually did the best! Like I mentioned before, sometimes the simplest method leads to the best results. 

*NOTE: I didn't do any tuning of the hyperparameters for the decision tree and random forest models so perhaps I didn't get the optimal models for them. Feel free to tune the models to improve them!*

# Potential Improvements

Finally, we've covered the entire basic data science workflow by simply hunting down an answer for a simple yet difficult question to answer: what factors determine who gets paid a higher NBA salary? 

This process was quite extensive, and thus, there is much room for improvement. With this project, I've prepared a template for any of you to improve upon and yield even better results than I did. Here are a few things (not comprehensive) that could be improved:


### 1. Dealing with the time value of money more effectively

Because of the fact that the value of money changed over time, I knew that I couldn't compare salaries from the 1990s to the salaries of players today. Instead of converting the numbers to a single adjusted number, I opted to created a metric called League Weight that would make all of the player salaries relative to one another in terms of who has a greater proportion of the total payroll. Maybe this wasn't the best way to deal with the problem. Feel free to find a better way to adjust the salary numbers.

### 2. Getting more over-arching data

I only used data from 1990 to the present because any salary data and advanced statistics were sparse and difficult to find for the years earlier. Before 1979 there wasn't even a 3-point line! Nonetheless, for people who are less impatient than me, don't be afraid to look for more historic data that might allow for a more comprehensive analysis of the salary data of NBA history.

### 3. Adding more features

I used a LOT of variables in this project, but the significance of the variables to the final outcome definitely differed a lot. There are many other advanced stats in the NBA that I didn't use that others could add. 

### 4. Using more machine learning algorithms

I only used three machine learning algorithms, and all of them were pretty simple. I considered using neural networks but that would have required a lot of hyperparameter tuning. 

### 5. Fine-tuning the algorithms already used

Even amongst the few algorithms I used in this project, for decision trees and random forests, I could definitely have tuned the models to make them more effective. 


```

```
