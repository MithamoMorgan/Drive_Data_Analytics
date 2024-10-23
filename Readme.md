![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/header_img.jpg)
## Project Overview:

This project aims to scrape car data from the [Kai and Karo](https://www.kaiandkaro.com/vehicles?model__make__vehicle_type=Automobile)  website, store the data in a database, and analyze pricing trends in Kenya's car market. The dashboard will provide an interactive overview of key metrics, enabling users to perform exploratory data analysis (EDA) on various factors influencing car prices.

## Tools:

* **Python:** For web scraping, data processing, and machine learning.
* **BeautifulSoup:** To scrape data from Kai and Karo websites.
* **MySQL:** For database management and storage.
* **Pandas:** For data cleaning and manipulation.
* **Matplotlib/Seaborn:** For data visualization.
* **Tableau:** For creating an interactive dashboard.

## Scraping and Data Storage

Here is the [link](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/Scraping.ipynb) to the file with the scraping code.

## Dataset

The dataset has 8 columns:
* Name - This is the name of the car
* Transimission_Type - Either Automatic or Manual
* Engine_Size - The size of the engine in cubic centimeters(CC)
* Usage_Origin - Either Kenyan used or Foreign used
* Year - The year of manufacture
* Price - The price of the car
* Make - The make of the car eg Toyota
* Model - Model of the car eg C200 for mercedes benz
* Car_Age - The age of the car
* Price_Range - Price categories

## Problem Statement

The Kenyan car market is characterized by fluctuating prices and a wide variety of options, making it challenging for buyers and sellers to understand current trends. There is a lack of accessible, comprehensive data that highlights pricing dynamics and vehicle availability. This project seeks to address this gap by scraping car data from the Kai and Karo website, enabling a deeper understanding of pricing trends and market behavior through a user-friendly dashboard that facilitates exploratory data analysis (EDA).

## Data Processing

The data had 64 duplicates which were droped and all the data inconsistencies were corrected. Here is the [link](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/preprocessing.ipynb) to the preprocessing  file with the code used to clean the data.

## Feature Engineering

Only 4 features were added from the original features. Make and Model were both extracted from name column using the following code;

```python
# Split column 'Name' into 'Make' and 'Model'

car_df['Name'] = car_df['Name'].str.title()
unique_names = ['Alfa Romeo', 'Aston Martin', 'Land Rover', 'Mercedes Benz', 'Rolls Royce', 'Range Rover']

def split_name(Name):
    for unique_name in unique_names:
        if Name.startswith(unique_name):
            Make = unique_name
            Model = Name[len(Make):].strip()
            return Make, Model

    single_name_make = Name.split(' ', 1)
    if len(single_name_make) > 1:
        Make, Model = single_name_make[0], single_name_make[1]
    else:
        Make = Name
        Model = ''
    return Make, Model

car_df[['Make', 'Model']] = car_df['Name'].apply(lambda x: pd.Series(split_name(x)))
```
Car_Age was extracted from Year colum;

```python
# Get the current year
date = date.today()
year = date.year

# Get the car age
car_df['Car_Age'] = car_df['Year'].apply(lambda x: year - x)
```

Finally Price_Range from Price colum;

```python
def price_range(Price):
    if Price <= 1000000:
        return '0-1M'
    elif Price <= 2000000:
        return '1M-2M'
    elif Price <= 3000000:
        return '2M-3M'
    elif Price <= 5000000:
        return '3M-5M'
    elif Price <= 10000000:
        return '5M-10M'
    elif Price <= 20000000:
        return '10M-20M'
    elif Price <= 30000000:
        return '20M-30M'
    else:
        return 'Above 30M'

car_df['Price_Range'] = car_df['Price'].apply(lambda x: price_range(x))
```

## EDA

Exploratory data analysis was conducted to better understand the data and the relationship between variables. Here is [link](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/EDA_Descriptive.ipynb) to the EDA file.

## Dashboard:

To interact with the dashboard, click the following [link](https://public.tableau.com/app/profile/morgan.murimi/viz/Book1_17295698513280/Dashboard?publish=yes).

## Template Dashboard Screenshot

![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/img1.jpg)
![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/Img2.jpg)
