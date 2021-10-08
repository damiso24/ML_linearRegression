# Machine Learning - Linear Regression Model

![enter image description here](200m_viz_snapshot.png)

## What will be the time that wins the gold?
Using historic olympic data as far back as 1800s-2016, I used a linear regression model to predict what time will win the gold medal for two seperate events in the 2020 (2021) olympics.  I chose Mens 200m and 100m to predict.

To accomplish this I used the following technologies:
 - Python Environmnet
 - Jupyter Notebook
 - Pandas 
 - ScikitLearn (Sklearn)
 - Linear Regression Algorithm 
 - Numpy, scipy.stats 
 - Tableau 

See visualization here:  https://public.tableau.com/shared/RQKKQFHRP?:display_count=n&:origin=viz_share_link)

### Enjoy :)

{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "%matplotlib inline\n",
    "import matplotlib.pyplot as plt\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "from scipy.stats import linregress"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Raw Data Import"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Gender</th>\n",
       "      <th>Event</th>\n",
       "      <th>Location</th>\n",
       "      <th>Year</th>\n",
       "      <th>Medal</th>\n",
       "      <th>Name</th>\n",
       "      <th>Nationality</th>\n",
       "      <th>Result</th>\n",
       "      <th>Unnamed: 8</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>G</td>\n",
       "      <td>Mohamed FARAH</td>\n",
       "      <td>USA</td>\n",
       "      <td>25:05.2</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>S</td>\n",
       "      <td>Paul Kipngetich TANUI</td>\n",
       "      <td>KEN</td>\n",
       "      <td>27:05.6</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>B</td>\n",
       "      <td>Tamirat TOLA</td>\n",
       "      <td>ETH</td>\n",
       "      <td>27:06.3</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>G</td>\n",
       "      <td>Kenenisa BEKELE</td>\n",
       "      <td>ETH</td>\n",
       "      <td>27:01.2</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>S</td>\n",
       "      <td>Sileshi SIHINE</td>\n",
       "      <td>ETH</td>\n",
       "      <td>27:02.8</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2389</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Athens</td>\n",
       "      <td>2004</td>\n",
       "      <td>S</td>\n",
       "      <td>Hrysopiyi DEVETZI</td>\n",
       "      <td>GRE</td>\n",
       "      <td>15.25</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2390</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Athens</td>\n",
       "      <td>2004</td>\n",
       "      <td>B</td>\n",
       "      <td>Tatyana LEBEDEVA</td>\n",
       "      <td>RUS</td>\n",
       "      <td>15.14</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2391</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Atlanta</td>\n",
       "      <td>1996</td>\n",
       "      <td>G</td>\n",
       "      <td>Inessa KRAVETS</td>\n",
       "      <td>UKR</td>\n",
       "      <td>15.33</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2392</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Atlanta</td>\n",
       "      <td>1996</td>\n",
       "      <td>S</td>\n",
       "      <td>Inna LASOVSKAYA</td>\n",
       "      <td>RUS</td>\n",
       "      <td>14.98</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2393</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Atlanta</td>\n",
       "      <td>1996</td>\n",
       "      <td>B</td>\n",
       "      <td>Sarka KASPARKOVA</td>\n",
       "      <td>CZE</td>\n",
       "      <td>14.98</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>2394 rows × 9 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     Gender              Event Location  Year Medal                   Name  \\\n",
       "0         M         10000M Men      Rio  2016     G          Mohamed FARAH   \n",
       "1         M         10000M Men      Rio  2016     S  Paul Kipngetich TANUI   \n",
       "2         M         10000M Men      Rio  2016     B           Tamirat TOLA   \n",
       "3         M         10000M Men  Beijing  2008     G        Kenenisa BEKELE   \n",
       "4         M         10000M Men  Beijing  2008     S         Sileshi SIHINE   \n",
       "...     ...                ...      ...   ...   ...                    ...   \n",
       "2389      W  Triple Jump Women   Athens  2004     S      Hrysopiyi DEVETZI   \n",
       "2390      W  Triple Jump Women   Athens  2004     B       Tatyana LEBEDEVA   \n",
       "2391      W  Triple Jump Women  Atlanta  1996     G         Inessa KRAVETS   \n",
       "2392      W  Triple Jump Women  Atlanta  1996     S        Inna LASOVSKAYA   \n",
       "2393      W  Triple Jump Women  Atlanta  1996     B       Sarka KASPARKOVA   \n",
       "\n",
       "     Nationality   Result  Unnamed: 8  \n",
       "0            USA  25:05.2         NaN  \n",
       "1            KEN  27:05.6         NaN  \n",
       "2            ETH  27:06.3         NaN  \n",
       "3            ETH  27:01.2         NaN  \n",
       "4            ETH  27:02.8         NaN  \n",
       "...          ...      ...         ...  \n",
       "2389         GRE    15.25         NaN  \n",
       "2390         RUS    15.14         NaN  \n",
       "2391         UKR    15.33         NaN  \n",
       "2392         RUS    14.98         NaN  \n",
       "2393         CZE    14.98         NaN  \n",
       "\n",
       "[2394 rows x 9 columns]"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "alldata = pd.read_csv(\"olympic_results_history.csv\", parse_dates=True)\n",
    "alldata"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 2394 entries, 0 to 2393\n",
      "Data columns (total 9 columns):\n",
      " #   Column       Non-Null Count  Dtype  \n",
      "---  ------       --------------  -----  \n",
      " 0   Gender       2394 non-null   object \n",
      " 1   Event        2394 non-null   object \n",
      " 2   Location     2394 non-null   object \n",
      " 3   Year         2394 non-null   int64  \n",
      " 4   Medal        2394 non-null   object \n",
      " 5   Name         2164 non-null   object \n",
      " 6   Nationality  2394 non-null   object \n",
      " 7   Result       2394 non-null   object \n",
      " 8   Unnamed: 8   12 non-null     float64\n",
      "dtypes: float64(1), int64(1), object(7)\n",
      "memory usage: 168.5+ KB\n"
     ]
    }
   ],
   "source": [
    "alldata.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Data Cleaning"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Gender</th>\n",
       "      <th>Event</th>\n",
       "      <th>Location</th>\n",
       "      <th>Year</th>\n",
       "      <th>Medal</th>\n",
       "      <th>Name</th>\n",
       "      <th>Nationality</th>\n",
       "      <th>Result</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>G</td>\n",
       "      <td>Mohamed FARAH</td>\n",
       "      <td>USA</td>\n",
       "      <td>25:05.2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>S</td>\n",
       "      <td>Paul Kipngetich TANUI</td>\n",
       "      <td>KEN</td>\n",
       "      <td>27:05.6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>B</td>\n",
       "      <td>Tamirat TOLA</td>\n",
       "      <td>ETH</td>\n",
       "      <td>27:06.3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>G</td>\n",
       "      <td>Kenenisa BEKELE</td>\n",
       "      <td>ETH</td>\n",
       "      <td>27:01.2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>M</td>\n",
       "      <td>10000M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>S</td>\n",
       "      <td>Sileshi SIHINE</td>\n",
       "      <td>ETH</td>\n",
       "      <td>27:02.8</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2389</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Athens</td>\n",
       "      <td>2004</td>\n",
       "      <td>S</td>\n",
       "      <td>Hrysopiyi DEVETZI</td>\n",
       "      <td>GRE</td>\n",
       "      <td>15.25</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2390</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Athens</td>\n",
       "      <td>2004</td>\n",
       "      <td>B</td>\n",
       "      <td>Tatyana LEBEDEVA</td>\n",
       "      <td>RUS</td>\n",
       "      <td>15.14</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2391</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Atlanta</td>\n",
       "      <td>1996</td>\n",
       "      <td>G</td>\n",
       "      <td>Inessa KRAVETS</td>\n",
       "      <td>UKR</td>\n",
       "      <td>15.33</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2392</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Atlanta</td>\n",
       "      <td>1996</td>\n",
       "      <td>S</td>\n",
       "      <td>Inna LASOVSKAYA</td>\n",
       "      <td>RUS</td>\n",
       "      <td>14.98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2393</th>\n",
       "      <td>W</td>\n",
       "      <td>Triple Jump Women</td>\n",
       "      <td>Atlanta</td>\n",
       "      <td>1996</td>\n",
       "      <td>B</td>\n",
       "      <td>Sarka KASPARKOVA</td>\n",
       "      <td>CZE</td>\n",
       "      <td>14.98</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>2394 rows × 8 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     Gender              Event Location  Year Medal                   Name  \\\n",
       "0         M         10000M Men      Rio  2016     G          Mohamed FARAH   \n",
       "1         M         10000M Men      Rio  2016     S  Paul Kipngetich TANUI   \n",
       "2         M         10000M Men      Rio  2016     B           Tamirat TOLA   \n",
       "3         M         10000M Men  Beijing  2008     G        Kenenisa BEKELE   \n",
       "4         M         10000M Men  Beijing  2008     S         Sileshi SIHINE   \n",
       "...     ...                ...      ...   ...   ...                    ...   \n",
       "2389      W  Triple Jump Women   Athens  2004     S      Hrysopiyi DEVETZI   \n",
       "2390      W  Triple Jump Women   Athens  2004     B       Tatyana LEBEDEVA   \n",
       "2391      W  Triple Jump Women  Atlanta  1996     G         Inessa KRAVETS   \n",
       "2392      W  Triple Jump Women  Atlanta  1996     S        Inna LASOVSKAYA   \n",
       "2393      W  Triple Jump Women  Atlanta  1996     B       Sarka KASPARKOVA   \n",
       "\n",
       "     Nationality   Result  \n",
       "0            USA  25:05.2  \n",
       "1            KEN  27:05.6  \n",
       "2            ETH  27:06.3  \n",
       "3            ETH  27:01.2  \n",
       "4            ETH  27:02.8  \n",
       "...          ...      ...  \n",
       "2389         GRE    15.25  \n",
       "2390         RUS    15.14  \n",
       "2391         UKR    15.33  \n",
       "2392         RUS    14.98  \n",
       "2393         CZE    14.98  \n",
       "\n",
       "[2394 rows x 8 columns]"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Dropping columns\n",
    "\n",
    "alldata.drop(['Unnamed: 8'], axis=1, inplace=True)\n",
    "alldata"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Filter for 100m Men"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "82\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Gender</th>\n",
       "      <th>Event</th>\n",
       "      <th>Location</th>\n",
       "      <th>Year</th>\n",
       "      <th>Medal</th>\n",
       "      <th>Name</th>\n",
       "      <th>Nationality</th>\n",
       "      <th>Result</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>69</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>9.81</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>S</td>\n",
       "      <td>Justin GATLIN</td>\n",
       "      <td>USA</td>\n",
       "      <td>9.89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>71</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>B</td>\n",
       "      <td>Andre DE GRASSE</td>\n",
       "      <td>CAN</td>\n",
       "      <td>9.91</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>72</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>9.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>73</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>S</td>\n",
       "      <td>Richard THOMPSON</td>\n",
       "      <td>TTO</td>\n",
       "      <td>9.89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>146</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>St Louis</td>\n",
       "      <td>1904</td>\n",
       "      <td>B</td>\n",
       "      <td>William HOGENSON</td>\n",
       "      <td>USA</td>\n",
       "      <td>11.2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>147</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>G</td>\n",
       "      <td>Thomas BURKE</td>\n",
       "      <td>USA</td>\n",
       "      <td>12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>148</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>S</td>\n",
       "      <td>Fritz HOFMANN</td>\n",
       "      <td>GER</td>\n",
       "      <td>12.2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>149</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>B</td>\n",
       "      <td>Alajos SZOKOLYI</td>\n",
       "      <td>HUN</td>\n",
       "      <td>12.6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>150</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>B</td>\n",
       "      <td>Francis LANE</td>\n",
       "      <td>USA</td>\n",
       "      <td>12.6</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>82 rows × 8 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "    Gender     Event  Location  Year Medal              Name Nationality  \\\n",
       "69       M  100M Men       Rio  2016     G        Usain BOLT         JAM   \n",
       "70       M  100M Men       Rio  2016     S     Justin GATLIN         USA   \n",
       "71       M  100M Men       Rio  2016     B   Andre DE GRASSE         CAN   \n",
       "72       M  100M Men   Beijing  2008     G        Usain BOLT         JAM   \n",
       "73       M  100M Men   Beijing  2008     S  Richard THOMPSON         TTO   \n",
       "..     ...       ...       ...   ...   ...               ...         ...   \n",
       "146      M  100M Men  St Louis  1904     B  William HOGENSON         USA   \n",
       "147      M  100M Men    Athens  1896     G      Thomas BURKE         USA   \n",
       "148      M  100M Men    Athens  1896     S     Fritz HOFMANN         GER   \n",
       "149      M  100M Men    Athens  1896     B   Alajos SZOKOLYI         HUN   \n",
       "150      M  100M Men    Athens  1896     B      Francis LANE         USA   \n",
       "\n",
       "    Result  \n",
       "69    9.81  \n",
       "70    9.89  \n",
       "71    9.91  \n",
       "72    9.69  \n",
       "73    9.89  \n",
       "..     ...  \n",
       "146   11.2  \n",
       "147     12  \n",
       "148   12.2  \n",
       "149   12.6  \n",
       "150   12.6  \n",
       "\n",
       "[82 rows x 8 columns]"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "men100m = alldata.loc[(alldata['Event'] == '100M Men')]\n",
    "print(len(men100m))\n",
    "men100m"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Creating copy to overwrite.\n",
    "\n",
    "men100m_clean=men100m.copy()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Gender</th>\n",
       "      <th>Event</th>\n",
       "      <th>Location</th>\n",
       "      <th>Year</th>\n",
       "      <th>Medal</th>\n",
       "      <th>Name</th>\n",
       "      <th>Nationality</th>\n",
       "      <th>Result</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>69</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>9.81</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>S</td>\n",
       "      <td>Justin GATLIN</td>\n",
       "      <td>USA</td>\n",
       "      <td>9.89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>71</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>B</td>\n",
       "      <td>Andre DE GRASSE</td>\n",
       "      <td>CAN</td>\n",
       "      <td>9.91</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>72</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>9.69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>73</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>S</td>\n",
       "      <td>Richard THOMPSON</td>\n",
       "      <td>TTO</td>\n",
       "      <td>9.89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>146</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>St Louis</td>\n",
       "      <td>1904</td>\n",
       "      <td>B</td>\n",
       "      <td>William HOGENSON</td>\n",
       "      <td>USA</td>\n",
       "      <td>11.2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>147</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>G</td>\n",
       "      <td>Thomas BURKE</td>\n",
       "      <td>USA</td>\n",
       "      <td>12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>148</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>S</td>\n",
       "      <td>Fritz HOFMANN</td>\n",
       "      <td>GER</td>\n",
       "      <td>12.2</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>149</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>B</td>\n",
       "      <td>Alajos SZOKOLYI</td>\n",
       "      <td>HUN</td>\n",
       "      <td>12.6</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>150</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>B</td>\n",
       "      <td>Francis LANE</td>\n",
       "      <td>USA</td>\n",
       "      <td>12.6</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>80 rows × 8 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "    Gender     Event  Location  Year Medal              Name Nationality  \\\n",
       "69       M  100M Men       Rio  2016     G        Usain BOLT         JAM   \n",
       "70       M  100M Men       Rio  2016     S     Justin GATLIN         USA   \n",
       "71       M  100M Men       Rio  2016     B   Andre DE GRASSE         CAN   \n",
       "72       M  100M Men   Beijing  2008     G        Usain BOLT         JAM   \n",
       "73       M  100M Men   Beijing  2008     S  Richard THOMPSON         TTO   \n",
       "..     ...       ...       ...   ...   ...               ...         ...   \n",
       "146      M  100M Men  St Louis  1904     B  William HOGENSON         USA   \n",
       "147      M  100M Men    Athens  1896     G      Thomas BURKE         USA   \n",
       "148      M  100M Men    Athens  1896     S     Fritz HOFMANN         GER   \n",
       "149      M  100M Men    Athens  1896     B   Alajos SZOKOLYI         HUN   \n",
       "150      M  100M Men    Athens  1896     B      Francis LANE         USA   \n",
       "\n",
       "    Result  \n",
       "69    9.81  \n",
       "70    9.89  \n",
       "71    9.91  \n",
       "72    9.69  \n",
       "73    9.89  \n",
       "..     ...  \n",
       "146   11.2  \n",
       "147     12  \n",
       "148   12.2  \n",
       "149   12.6  \n",
       "150   12.6  \n",
       "\n",
       "[80 rows x 8 columns]"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Removing null values.  2 Result values were \"None\" so I removed them.\n",
    "\n",
    "remove_null = men100m_clean.loc[men100m_clean['Result'] == 'None'].index\n",
    "men100m_clean.drop(remove_null , inplace=True)\n",
    "men100m_clean"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "# men100m_clean.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Change the Results column to a float\n",
    "\n",
    "men100m_clean['Result'] = men100m_clean['Result'].astype(float)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "80"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(men100m_clean)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Gender</th>\n",
       "      <th>Event</th>\n",
       "      <th>Location</th>\n",
       "      <th>Year</th>\n",
       "      <th>Medal</th>\n",
       "      <th>Name</th>\n",
       "      <th>Nationality</th>\n",
       "      <th>Result</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>69</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>9.81</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>S</td>\n",
       "      <td>Justin GATLIN</td>\n",
       "      <td>USA</td>\n",
       "      <td>9.89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>71</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>B</td>\n",
       "      <td>Andre DE GRASSE</td>\n",
       "      <td>CAN</td>\n",
       "      <td>9.91</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>113</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>London</td>\n",
       "      <td>2012</td>\n",
       "      <td>B</td>\n",
       "      <td>Justin GATLIN</td>\n",
       "      <td>USA</td>\n",
       "      <td>9.79</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>112</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>London</td>\n",
       "      <td>2012</td>\n",
       "      <td>S</td>\n",
       "      <td>Yohan BLAKE</td>\n",
       "      <td>JAM</td>\n",
       "      <td>9.75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>108</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Paris</td>\n",
       "      <td>1900</td>\n",
       "      <td>G</td>\n",
       "      <td>Frank JARVIS</td>\n",
       "      <td>USA</td>\n",
       "      <td>11.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>147</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>G</td>\n",
       "      <td>Thomas BURKE</td>\n",
       "      <td>USA</td>\n",
       "      <td>12.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>148</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>S</td>\n",
       "      <td>Fritz HOFMANN</td>\n",
       "      <td>GER</td>\n",
       "      <td>12.20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>149</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>B</td>\n",
       "      <td>Alajos SZOKOLYI</td>\n",
       "      <td>HUN</td>\n",
       "      <td>12.60</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>150</th>\n",
       "      <td>M</td>\n",
       "      <td>100M Men</td>\n",
       "      <td>Athens</td>\n",
       "      <td>1896</td>\n",
       "      <td>B</td>\n",
       "      <td>Francis LANE</td>\n",
       "      <td>USA</td>\n",
       "      <td>12.60</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>80 rows × 8 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "    Gender     Event Location  Year Medal             Name Nationality  Result\n",
       "69       M  100M Men      Rio  2016     G       Usain BOLT         JAM    9.81\n",
       "70       M  100M Men      Rio  2016     S    Justin GATLIN         USA    9.89\n",
       "71       M  100M Men      Rio  2016     B  Andre DE GRASSE         CAN    9.91\n",
       "113      M  100M Men   London  2012     B    Justin GATLIN         USA    9.79\n",
       "112      M  100M Men   London  2012     S      Yohan BLAKE         JAM    9.75\n",
       "..     ...       ...      ...   ...   ...              ...         ...     ...\n",
       "108      M  100M Men    Paris  1900     G     Frank JARVIS         USA   11.00\n",
       "147      M  100M Men   Athens  1896     G     Thomas BURKE         USA   12.00\n",
       "148      M  100M Men   Athens  1896     S    Fritz HOFMANN         GER   12.20\n",
       "149      M  100M Men   Athens  1896     B  Alajos SZOKOLYI         HUN   12.60\n",
       "150      M  100M Men   Athens  1896     B     Francis LANE         USA   12.60\n",
       "\n",
       "[80 rows x 8 columns]"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Sorting by Year\n",
    "\n",
    "men100m_clean.sort_values('Year', ascending=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Output to CSV\n",
    "\n",
    "men100m_clean.to_csv(\"./damiso_men_100m_cleaned.csv\", index=False, header=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Create dataframe with my target columns for my Model  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Year</th>\n",
       "      <th>Result</th>\n",
       "      <th>Medal</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>69</th>\n",
       "      <td>2016</td>\n",
       "      <td>9.81</td>\n",
       "      <td>G</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70</th>\n",
       "      <td>2016</td>\n",
       "      <td>9.89</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>71</th>\n",
       "      <td>2016</td>\n",
       "      <td>9.91</td>\n",
       "      <td>B</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>72</th>\n",
       "      <td>2008</td>\n",
       "      <td>9.69</td>\n",
       "      <td>G</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>73</th>\n",
       "      <td>2008</td>\n",
       "      <td>9.89</td>\n",
       "      <td>S</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    Year  Result Medal\n",
       "69  2016    9.81     G\n",
       "70  2016    9.89     S\n",
       "71  2016    9.91     B\n",
       "72  2008    9.69     G\n",
       "73  2008    9.89     S"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Select my target columns\n",
    "\n",
    "men100m_df = men100m_clean[['Year', 'Result', 'Medal']]\n",
    "men100m_df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Output to CSV\n",
    "\n",
    "men100m_df.to_csv(\"./damiso_men_100m_target.csv\", index=False, header=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Shape:  (80, 1) (80, 1)\n"
     ]
    }
   ],
   "source": [
    "# Reshaping my target array\n",
    "\n",
    "X = men100m_df[\"Year\"].values.reshape(-1, 1)\n",
    "y = men100m_df[\"Result\"].values.reshape(-1, 1)\n",
    "\n",
    "print(\"Shape: \", X.shape, y.shape)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Train Test Split\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Train Test Split\n",
    "\n",
    "from sklearn.model_selection import train_test_split\n",
    "\n",
    "X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42, train_size = .85)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(68, 1)"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X_train.shape\n",
    "# y_train.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(12, 1)"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X_test.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Adding 2020 to the Test"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [],
   "source": [
    "# creating new X_test\n",
    "\n",
    "new_X_test = X_test\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(13, 1)"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Adding 2020 to the X_test array and reshaping it.\n",
    "\n",
    "X3 = np.append([new_X_test],[2020])\n",
    "X3 = X3.reshape(-1, 1)\n",
    "X3.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [],
   "source": [
    "# creating new y_test\n",
    "\n",
    "new_y_test = y_test"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(13, 1)"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Adding 2020 results to the y_test array and reshaping it.\n",
    "\n",
    "y3 = np.append([new_y_test], [9.80])\n",
    "y3 = y3.reshape(-1,1)\n",
    "y3.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Create my Model"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Create the model\n",
    "\n",
    "from sklearn.linear_model import LinearRegression\n",
    "\n",
    "model = LinearRegression()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "LinearRegression()"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Fit the model to the training data. \n",
    "\n",
    "model.fit(X_train, y_train)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Mean Squared Error (MSE): 0.04646579833710119\n",
      "R-squared (R2 ): 0.7145710991941663\n"
     ]
    }
   ],
   "source": [
    "# Calculate the mean_squared_error and the r-squared value for the testing data\n",
    "\n",
    "from sklearn.metrics import mean_squared_error, r2_score\n",
    "\n",
    "# Use our model to make predictions\n",
    "predicted = model.predict(X3)\n",
    "\n",
    "# Score the predictions with mse and r2\n",
    "mse = mean_squared_error(y3, predicted)\n",
    "r2 = r2_score(y3, predicted)\n",
    "\n",
    "\n",
    "print(f\"Mean Squared Error (MSE): {mse}\")\n",
    "print(f\"R-squared (R2 ): {r2}\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.7145710991941663"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Call the `score` method on the model to show the r2 score\n",
    "\n",
    "model.score(X3, y3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Creating final DataFrame for export\n",
    "\n",
    "new_df = pd.DataFrame(X3)\n",
    "new_df['Actual'] = pd.DataFrame(y3)\n",
    "new_df['Predicted'] = pd.DataFrame(predicted).round(2)\n",
    "new_df.columns = ['Year', 'Actual', 'Predicted']\n",
    "new_df.sort_values('Year', ascending=False, inplace = True)\n",
    "new_df.drop_duplicates(subset='Year', ignore_index=True, inplace = True)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Output to CSV\n",
    "\n",
    "new_df.to_csv(\"./damiso_men_100m_linear_regression_output.csv\", index=False, header=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# New Model for Mens 200m"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Cleaning data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "78"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "### Filter by 200M\n",
    "\n",
    "men200m=alldata.loc[(alldata['Event'] == '200M Men')]\n",
    "len(men200m)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Creating a copy to overwrite.\n",
    "\n",
    "men200_clean = men200m.copy()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Removing null values.  2 Result values were \"None\" so I removed them.\n",
    "\n",
    "remove_null = men200_clean.loc[men200_clean['Result'] == 'None'].index\n",
    "men200_clean.drop(remove_null , inplace=True)\n",
    "# men200_clean.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "75"
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(men200_clean)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Change the Result column to a float.\n",
    "\n",
    "men200_clean['Result'] = men200_clean['Result'].astype(float)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [],
   "source": [
    "# men200_clean.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Gender</th>\n",
       "      <th>Event</th>\n",
       "      <th>Location</th>\n",
       "      <th>Year</th>\n",
       "      <th>Medal</th>\n",
       "      <th>Name</th>\n",
       "      <th>Nationality</th>\n",
       "      <th>Result</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>312</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>19.78</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>313</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>S</td>\n",
       "      <td>Andre DE GRASSE</td>\n",
       "      <td>CAN</td>\n",
       "      <td>20.02</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>314</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Rio</td>\n",
       "      <td>2016</td>\n",
       "      <td>B</td>\n",
       "      <td>Christophe LEMAITRE</td>\n",
       "      <td>FRA</td>\n",
       "      <td>20.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>315</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>G</td>\n",
       "      <td>Usain BOLT</td>\n",
       "      <td>JAM</td>\n",
       "      <td>19.30</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>316</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Beijing</td>\n",
       "      <td>2008</td>\n",
       "      <td>S</td>\n",
       "      <td>Shawn CRAWFORD</td>\n",
       "      <td>USA</td>\n",
       "      <td>19.96</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>384</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Stockholm</td>\n",
       "      <td>1912</td>\n",
       "      <td>G</td>\n",
       "      <td>Ralph CRAIG</td>\n",
       "      <td>USA</td>\n",
       "      <td>21.70</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>385</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Stockholm</td>\n",
       "      <td>1912</td>\n",
       "      <td>S</td>\n",
       "      <td>Donald LIPPINCOTT</td>\n",
       "      <td>USA</td>\n",
       "      <td>21.80</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>386</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>Stockholm</td>\n",
       "      <td>1912</td>\n",
       "      <td>B</td>\n",
       "      <td>William APPLEGARTH</td>\n",
       "      <td>GBR</td>\n",
       "      <td>22.00</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>387</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>St Louis</td>\n",
       "      <td>1904</td>\n",
       "      <td>G</td>\n",
       "      <td>Archie HAHN</td>\n",
       "      <td>USA</td>\n",
       "      <td>21.60</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>388</th>\n",
       "      <td>M</td>\n",
       "      <td>200M Men</td>\n",
       "      <td>St Louis</td>\n",
       "      <td>1904</td>\n",
       "      <td>S</td>\n",
       "      <td>Nate CARTMELL</td>\n",
       "      <td>USA</td>\n",
       "      <td>21.90</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>75 rows × 8 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "    Gender     Event   Location  Year Medal                 Name Nationality  \\\n",
       "312      M  200M Men        Rio  2016     G           Usain BOLT         JAM   \n",
       "313      M  200M Men        Rio  2016     S      Andre DE GRASSE         CAN   \n",
       "314      M  200M Men        Rio  2016     B  Christophe LEMAITRE         FRA   \n",
       "315      M  200M Men    Beijing  2008     G           Usain BOLT         JAM   \n",
       "316      M  200M Men    Beijing  2008     S       Shawn CRAWFORD         USA   \n",
       "..     ...       ...        ...   ...   ...                  ...         ...   \n",
       "384      M  200M Men  Stockholm  1912     G          Ralph CRAIG         USA   \n",
       "385      M  200M Men  Stockholm  1912     S    Donald LIPPINCOTT         USA   \n",
       "386      M  200M Men  Stockholm  1912     B   William APPLEGARTH         GBR   \n",
       "387      M  200M Men   St Louis  1904     G          Archie HAHN         USA   \n",
       "388      M  200M Men   St Louis  1904     S        Nate CARTMELL         USA   \n",
       "\n",
       "     Result  \n",
       "312   19.78  \n",
       "313   20.02  \n",
       "314   20.12  \n",
       "315   19.30  \n",
       "316   19.96  \n",
       "..      ...  \n",
       "384   21.70  \n",
       "385   21.80  \n",
       "386   22.00  \n",
       "387   21.60  \n",
       "388   21.90  \n",
       "\n",
       "[75 rows x 8 columns]"
      ]
     },
     "execution_count": 35,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "men200_clean"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Output to CSV\n",
    "\n",
    "men200_clean.to_csv(\"./damiso_men_200m_cleaned.csv\", index=False, header=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Year</th>\n",
       "      <th>Result</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>312</th>\n",
       "      <td>2016</td>\n",
       "      <td>19.78</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>313</th>\n",
       "      <td>2016</td>\n",
       "      <td>20.02</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>314</th>\n",
       "      <td>2016</td>\n",
       "      <td>20.12</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>315</th>\n",
       "      <td>2008</td>\n",
       "      <td>19.30</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>316</th>\n",
       "      <td>2008</td>\n",
       "      <td>19.96</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "     Year  Result\n",
       "312  2016   19.78\n",
       "313  2016   20.02\n",
       "314  2016   20.12\n",
       "315  2008   19.30\n",
       "316  2008   19.96"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Select my target columns\n",
    "\n",
    "men200m_df = men200_clean[['Year', 'Result']]\n",
    "men200m_df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Output to CSV\n",
    "men200m_df.to_csv(\"./damiso_men_200m_target.csv\", index=False, header=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [],
   "source": [
    "# men200m_df.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Shape:  (75, 1) (75, 1)\n"
     ]
    }
   ],
   "source": [
    "X2 = men200m_df[\"Year\"].values.reshape(-1, 1)\n",
    "y2 = men200m_df[\"Result\"].values.reshape(-1, 1)\n",
    "\n",
    "print(\"Shape: \", X2.shape, y2.shape)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Train Test Split"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Train Test Split\n",
    "\n",
    "from sklearn.model_selection import train_test_split\n",
    "\n",
    "X2_train, X2_test, y2_train, y2_test = train_test_split(X2, y2, random_state=42, train_size = .85)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(12, 1)"
      ]
     },
     "execution_count": 42,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X2_test.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(12, 1)"
      ]
     },
     "execution_count": 43,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "y2_test.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Adding 2020 to the Test"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(13, 1)"
      ]
     },
     "execution_count": 44,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Creating new X2_test, appending the 2020 Year and Results and reshaping.\n",
    "\n",
    "new_X2_test = X2_test\n",
    "X6 = np.append([new_X2_test],[2020])\n",
    "X6 = X6.reshape(-1, 1)\n",
    "X6.shape\n",
    "# X6"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(13, 1)"
      ]
     },
     "execution_count": 45,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Creating new y2_test, appending the 2020 Year and Results and reshaping.\n",
    "\n",
    "new_y2_test = y2_test\n",
    "y6 = np.append([new_y2_test],[19.62])\n",
    "y6 = y6.reshape(-1, 1)\n",
    "y6.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Create my Model"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Create the model\n",
    "\n",
    "from sklearn.linear_model import LinearRegression\n",
    "\n",
    "model = LinearRegression()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Fit the Model"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "LinearRegression()"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Fit the model to the training data. \n",
    "\n",
    "model.fit(X2_train, y2_train)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Mean Squared Error (MSE): 0.09920912392936837\n",
      "R-squared (R2 ): 0.881152821453135\n"
     ]
    }
   ],
   "source": [
    "# Calculate the mean_squared_error and the r-squared value for the testing data\n",
    "\n",
    "from sklearn.metrics import mean_squared_error, r2_score\n",
    "\n",
    "# Use our model to make predictions\n",
    "predicted2 = model.predict(X6)\n",
    "\n",
    "# Score the predictions with mse and r2\n",
    "mse = mean_squared_error(y6, predicted2)\n",
    "r2 = r2_score(y6, predicted2)\n",
    "\n",
    "\n",
    "print(f\"Mean Squared Error (MSE): {mse}\")\n",
    "print(f\"R-squared (R2 ): {r2}\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.881152821453135"
      ]
     },
     "execution_count": 49,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Call the `score` method on the model to show the r2 score\n",
    "\n",
    "model.score(X6, y6)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Creating final DataFrame for export\n",
    "\n",
    "last_df = pd.DataFrame(X6)\n",
    "last_df['Actual'] = pd.DataFrame(y6)\n",
    "last_df['Predicted'] = pd.DataFrame(predicted2).round(2)\n",
    "last_df.columns = ['Year', 'Actual', 'Predicted']\n",
    "last_df.sort_values('Year', ascending=False, inplace = True)\n",
    "last_df.drop_duplicates(subset='Year', ignore_index=True, inplace = True)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Output to CSV\n",
    "\n",
    "last_df.to_csv(\"./damiso_men_200m_linear_regression_output.csv\", index=False, header=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "PythonAdv",
   "language": "python",
   "name": "pythonadv"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.13"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}

