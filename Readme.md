![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/header_img.jpg)

## Table of Contents

1. [Overview](#Overview)
2. [Tools](#Tools)
3. [Scraping and Data Storage](#Scraping-and-Data-Storage)
4. [Dataset](#Dataset)
5. [Problem Statement](#Problem-Statement)
6. [Data Processing](#Data-Processing)
7. [Feature Engineering](#Feature-Engineering)
8. [EDA](#EDA)
9. [Findings](#Findings)
10. [Tableau Dashboard](#Tableau-Dashboard)
11. [Template Dashboard Screenshot](#Template-Dashboard-Screenshot)
12. [Conclusion](#Conclusion)

## Overview

This project aims to scrape car data from [Kai and Karo](https://www.kaiandkaro.com/vehicles?model__make__vehicle_type=Automobile)  website, store the data in a database, and analyze pricing trends in Kenya's car market. The dashboard will provide an interactive overview of key metrics, enabling users to perform exploratory data analysis (EDA) on various factors influencing car prices.

## Tools

* **Python:** For web scraping, data processing.
* **BeautifulSoup:** To scrape data from Kai and Karo website.
* **MySQL:** For database management and storage.
* **DBeaver:** For managing and interacting with databases, facilitating SQL queries.
* **Pandas:** For data cleaning and manipulation.
* **Matplotlib/Seaborn:** For data visualization.
* **Tableau:** For creating an interactive dashboard.

## Scraping and Data Storage

Here is the [link](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/Scraping.ipynb) to the file with the scraping and storage code.

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

Here is the [link](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/final_clean_car_data.csv) to access the dataset. Feel free to download the dataset for more analysis.

![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/csv%20img.jpg)

## Problem Statement

The Kenyan car market is characterized by fluctuating prices and a wide variety of options, making it challenging for buyers and sellers to understand current trends. There is a lack of accessible, comprehensive data that highlights pricing dynamics and vehicle availability. This project seeks to address this gap by scraping car data from a car listings website, enabling a deeper understanding of pricing trends and market behavior through a user-friendly dashboard that facilitates exploratory data analysis (EDA).

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

## Findings

* The latest vehicle models tend to command higher price premiums when compared to older versions, reflecting advancements in technology, safety features, and design.

* Comparative analysis indicates that modern vehicles generally feature larger engine displacements compared to their older counterparts.

* A clear inverse relationship exist between engine size and vehicle cost, with more affordable vehicles typically having smaller engine displacements

* All the vehicles manufactured in 2024 are foreign used and are more than KES 10M. This is a clear indication that there is a positive correlation between the age and price of the vehicle

* Imported vehicles typically command higher prices than locally used kenyan vehicles.

## Tableau Dashboard

To interact with the dashboard, click the following [link](https://public.tableau.com/app/profile/morgan.murimi/viz/Book1_17295698513280/Dashboard?publish=yes).

## Template Dashboard Screenshot

![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/img1.jpg)
![](https://github.com/MithamoMorgan/Drive_Data_Analytics/blob/master/Img2.jpg)

## Conclusion

This project successfully scraped and analyzed car data from a car listings website, providing insights into the Kenyan car market. Key findings revealed correlations between vehicle age and price, as well as engine size and affordability. The interactive dashboard offers users a valuable tool for exploring these trends, addressing the need for accessible market data. Future enhancements could involve expanding the dataset and integrating predictive modeling techniques for real-time analysis.
