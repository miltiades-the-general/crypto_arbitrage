# Crypto Arbitrage Calculator
---
Trading in a market comes with high levels of uncertainty, insecurity, and risk in regards to a traders position. This makes the allure of trades that guarantee a profit for a trader that much stronger. Arbitrage presents the trader with an opportunity to ensure a trade with a high certainty of profit given that the trader acts quickly. Arbitrage can only take place when the difference between their buy position and sell position are disparate enough to ensure a profit after trading fees. These trades can be tricky to identify in real time especially in the context of fluctuating trading fees. However, they are much simpler to analyze retrospectively which is what we attempted to do in this application. We looked at trading prices between two exchanges: Bitstamp and Coinbase, during the first quarter of 2018 to see if we could have taken advantage of arbitrage opportunities at dates during the time period. 

## Technologies
* Jupyter Lab
* Jupyter Notebook
* Python 3.7.0
* Pandas Dataframe Library
* Matplot Lib Inline
* Anaconda Development Environment

## Installation Guide

___

## *Environment Setup*

* For best usage create an anaconda development environment using python version 3.7
* Refer to [Anaconda's Website](https://www.anaconda.com/) to download the most recent version of conda
* In the terminal run the following command to set up a conda environment 
```terminal
$ conda create dev python=3.7
```
* To initialize the environment, first navigate to your working directory, then in the terminal:
```terminal
$ conda activate dev
```
## *Jupyter Lab*

* This application is optimally run in Jupyter lab notebook as it produces many outputs that would be hard to keep track of in a single document. 

* To run Jupyter lab notebook ensure that Jupyter lab is downloaded in your development environment via the terminal:

```terminal
$ pip install jupyterlab
```
* To activate, first navigate to your desired directory, then:
```terminal
$ jupyter-lab
```
* This process will launch Jupyter lab on your local host. The application will be granted read/write permission to the files on your hard drive in the present working directory. Understand that changes made on Jupyter notebook apply to your local files. 

## *Dependencies*

* This application requires the *pandas*, *path_lib* and *matplot_inline* libraries.

* To import these libraries in your Jupyter notebook file

```python
import pandas as pd
from path_lib import Path
import matplotlib_inline
```

---
## Usage 
___
## *File Structure*
* The *crypto_arbitrage.ipynb* file contains the Jupyter notebook file for the application. Each component of the application is run inline using the active python kernel.
* The Resources folder contains the data in .csv files that will be used for the arbitrage analysis. The *bitsamp.csv* file contains data from January, 2018 to March, 2018 for various price statistics for Bitcoin on the Bitstamp exchange. Similarly, the *coinbase.csv* file contains data from January, 2018 to March, 2018 for various price statistics for Bitcoin on the Coinbase exchange.

## *Running the Application*
* The application can be run cellwise from top to bottom in the jupyter notebook. 
### *Phase 1: Data Preprocessing*
* The first process of the application is to import the data from the resources folder. The data will be saved into two Pandas dataframes corresponding to each exchange: bitstamp and coinbase.
* These dataframes house the raw unprocess data in appropriate row and column format. The columns correspond to the categories of data that are found in the header row of the respective .csv files and the rows correspond to an individual data at a timestamp in a datetime format. 
* To work with the data effectively it must be cleaned and processed first. While assigning the data to a Pandas dataframe the "Timestamp" column is used to set the index for the data so that it can be easily analyzed by time rather than by numerical index. 
* For example:
```python
bitstamp = pd.read_csv(
    csvpath,
    index_col="Timestamp",
    parse_dates=True,
    infer_datetime_format=True)
```
* The data is properly inserted into a Pandas dataframe, but it still contains erroneous data that may impact the results of our analysis later on. It contains null values for certain datapoints throughout our set and the 'Close' column contains '$' signs in front of the values of interest. This causes the column to be read as a series of strings rather than integers which is the format that we require for data manipulation. 
* We first drop the null values, then we drop the dollar signs in the 'Close' column by replacing '$' with ''. We are then free to change the type of the 'Close' column to a float using the pandas.df.astype("float") method. 
* This process is repeated for both Pandas dataframes.
* We then check to see if there is any duplicated values within our data using the df.duplicated() method. In our data we find that there is no duplicated datapoints for either of the dataframes, so we can move on. 

### *Phase 2: Preliminary Data Analysis*
* Now that the data is cleaned we move onto the preliminary data analysis phase. 

* We select the column that we want to use for our analysis as the 'Close' column, we save the sliced dataframe in a new variable that we use to proceed to find the summary statistics for and plot on a line-graph using the df.describe(), and df.plot() methods respectively.
* We use this sliced data to analyze an early and late time period in our data set. We plot the two dataframes with one overlaying the other so we can identify any discrepancies between the prices of the different exchanges that would indicate arbitrage opportunities. For our data we plot the price action of the two exchanges for January and March. 

### *Phase 3: Magnified Data Analysis*
* We now select three dates to analyze from an early point, middle point, and late point of our data set. The date we select will provide a range of datapoints that will describe the price action for the two exchanges throughout that day. For our data we selected a date in January, February and March. 
* Magnifying an individual day will allow us to filter the differences between the two exchanges in order to identify arbitrage opportunities for a given day. 
* For each of the three data points we plot the two exchanges relative to each other and identify any patterns that we see amongst the three dates.
* We calculate the spread of the two exchanges by subtracting the exchange we generally identify as cheaper, or which has a lower mean, from the exchange with the generally higher price, or higher mean value. 

### *Phase 4: Calculating Arbitrage*
* To begin the process of calculating the potential arbitrage profits, we define a function that will help to identify the spreads that are positive and eliminating the spreads that are negative. This is defined in the positive_spread() function which takes the dataframe of the spread values as the parameter. For every less than 0 value in the dataframe, we set the spread to 0 so that it is not calculated as a negative profit later on. 
* We can check how many 0 values are present in the dataframe using the df.sum() method for values equal to 0. In our data we find that the early dataframe has no 0 values, but the middle and late dataframes have many 0 values. This is reflected in the profit analysis.

* We then calculate the spread returns or the net revenue by dividing the spread by the price of the cheaper exchange value (since we are buying from the cheaper exchange and selling on the more expensive one).

* Finally we can find the profitable trade opportunities where the spread returns are greater than the trading fee in order to cover them. To do this we select the trade opportunities that are greater than 1%, and we subtract the 1% trading fee from them. 

* The results of this process will expose the retrospective trading opportunities that would have given profit by employing arbitrage. 