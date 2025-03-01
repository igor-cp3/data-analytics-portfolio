![Alt text](https://github.com/igor-cp3/data-analytics-portfolio/blob/main/Analysis%20of%20used%20car%20sales%20in%20Ukraine%202023/Cars_image.jpg)


# Analysis of used car sales in Ukraine in 2023


## Project Overview
Welcome to my first portfolio project. This project showcases my data analysis skills on a real dataset. 

The applied dataset contains information about vehicles, vehicle registration and re-registration transactions during 2023 in Ukraine. I chose this dataset because I am fascinated by the automotive topic, car specifications, and cars selling rates. While in automotive publications we often see statistics on new car sales, we do not see statistics on used car sales. That is why I was specifically interested in researching and analyzing this type of information.

The dataset was cleaned and processed using SQL, after which a data visualization was created in Tableau, which allowed me to draw some conclusions.

## Purposes of the Project
The main questions I wanted to get an answer to were:
- What brands and models of used cars are the most popular in Ukraine in 2023?
- How many cars are sold in the first year of use among the top 20 best-selling new cars in Ukraine in 2023?

## About data
The dataset contains information about vehicles, vehicles registration and re-registration transactions during 2023 in Ukraine. 
- Data source: Ministry of Internal Affairs of Ukraine 
- Data storage location: https://data.gov.ua/dataset
- Data type: Public 
- The data contains 20 columns and 2 124 732 rows:
  
![Alt text](https://github.com/igor-cp3/data-analytics-portfolio/blob/main/Analysis%20of%20used%20car%20sales%20in%20Ukraine%202023/Data_scheme.png)
  
## Steps Taken
1. **Defining goals**
2. **Collecting data**
3. **Looking into data**
4. **Cleaning and preparing data with SQL queries in DBeaver**
5. **Analyzing data**
6. **Visualizing by creating a dashboard in Tableau**
7. **Interpreting of the results**


## SQL Code

I performed all data transformations in DBeaver using the PostgreSQL relational database management system.

- First, I loaded the dataset 'tz_opendata_z01012023_po01012024' into DBeaver and checked if there were any missing cells in the dataset among the columns of interest.

```sql
SELECT count(*),
       count(OPER_NAME),
       count(OPER_CODE),
       count(D_REG),
       count(DEP),
       count(BRAND),
       count(MODEL),
       count(MAKE_YEAR),
       count(BODY),
       count(KIND),
       count(VIN)
FROM tz_opendata_z01012023_po01012024 tozp;
```
- Next, I familiarized myself with the brands and models of cars and the number of VIN code of each model in the dataset.

```sql
SELECT BRAND, MODEL, COUNT(VIN) as COUNT_VIN
FROM tz_opendata_z01012023_po01012024 tozp 
GROUP BY BRAND, MODEL
ORDER BY 3 desc;
```

- There is a nuance here: we have different model names in the dataset, although in fact it is one car model. For example: BMW 525 and BMW 520D, the model is the same - BMW 5, but the engines are different. The situation is similar with Toyota RAV4 and RAV-4 HYBRID. The model is the same, but the engines are different. Or as in the case of Volkswagen ID.4 PRO and PRO S - different configurations. Therefore, to understand people's interest in a particular model, I made changes to the dataset and unified the most popular models.

```sql
#RAV4 Different engines of the same model
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = 'RAV-4 HYBRID' THEN 'RAV4'
        WHEN MODEL = 'RAV4 EV' THEN 'RAV4'
        ELSE MODEL
    END
WHERE BRAND LIKE 'TOYOTA';

#ID.4 Different configurations and manufacturers of the same model
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = 'ID.4 PRO S' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ PRIME' THEN 'ID.4'
        WHEN MODEL = 'ID.4 PRO' THEN 'ID.4'
        WHEN MODEL = 'ID.4X' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ LITE PRO' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ PRO' THEN 'ID.4'
        ELSE MODEL
    END
WHERE BRAND LIKE 'VOLKSWAGEN';

#BMW Different engines of the same model
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = '530D' THEN '5'
        WHEN MODEL = '530' THEN '5'
        WHEN MODEL = '530I' THEN '5'
        WHEN MODEL = '520I' THEN '5'
        WHEN MODEL = '520' THEN '5'
        WHEN MODEL = '520D' THEN '5'
        WHEN MODEL = '525D' THEN '5'
        WHEN MODEL = '525' THEN '5'
        WHEN MODEL = '525I' THEN '5'
        WHEN MODEL = '540' THEN '5'
        WHEN MODEL = '540I' THEN '5'
        WHEN MODEL = '528I' THEN '5'
        WHEN MODEL = '523I' THEN '5'
        WHEN MODEL = '528' THEN '5'
        WHEN MODEL = '528XI' THEN '5'
        WHEN MODEL = '535XI' THEN '5'
        WHEN MODEL = '535' THEN '5'
        WHEN MODEL = '535I' THEN '5'
        WHEN MODEL = '535D' THEN '5'
        WHEN MODEL = '520D' THEN '5'
        WHEN MODEL = '328I' THEN '3'
        WHEN MODEL = '328' THEN '3'
        WHEN MODEL = '320D' THEN '3'
        WHEN MODEL = '318' THEN '3'
        WHEN MODEL = '316' THEN '3'
        WHEN MODEL = '320' THEN '3'
        WHEN MODEL = '318I' THEN '3'
        WHEN MODEL = '320I' THEN '3'
        WHEN MODEL = '316I' THEN '3'
        WHEN MODEL = '318D' THEN '3'
        WHEN MODEL = '330I' THEN '3'
        WHEN MODEL = '335I' THEN '3'
        WHEN MODEL = '325I' THEN '3'
        WHEN MODEL = '330D' THEN '3'
        WHEN MODEL = '328XI' THEN '3'
        WHEN MODEL = '330XI' THEN '3'
        WHEN MODEL = '328D' THEN '3'
        WHEN MODEL = '325' THEN '3'
        ELSE MODEL
    END
WHERE BRAND LIKE 'BMW';

#Lanos Different manufacturers of the same model
UPDATE tz_opendata_z01012023_po01012024 
SET BRAND = 
    CASE 
        WHEN BRAND = 'DAEWOO' THEN 'ЗАЗ-DAEWOO'
        WHEN BRAND = 'ЗАЗ' THEN 'ЗАЗ-DAEWOO'
        WHEN BRAND = 'CHEVROLET' THEN 'ЗАЗ-DAEWOO'
        WHEN BRAND = 'FSO' THEN 'ЗАЗ-DAEWOO'
        ELSE BRAND
    END
WHERE MODEL LIKE 'LANOS';

#Toyota Different configurations and engines of the same model
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = 'LAND CRUISER 200' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER 150' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER PRADO 150' THEN 'LAND CRUISER PRADO'
        WHEN MODEL = 'PRADO' THEN 'LAND CRUISER PRADO'
        WHEN MODEL = 'LAND CRUISER 120' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER 100' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER PRADO 120' THEN 'LAND CRUISER PRADO'
        WHEN MODEL = 'LAND CRUISER ' THEN 'LAND CRUISER'
        WHEN MODEL = 'C-HR HYBRID' THEN 'C-HR'
        WHEN MODEL = 'HIGHLANDER HYBRID' THEN 'HIGHLANDER'
        WHEN MODEL = 'YARIS HYBRID' THEN 'YARIS'
        ELSE MODEL
    END
WHERE BRAND LIKE 'TOYOTA';
```

- I checked whether the changes to the dataset worked correctly using the example of models: LANOS, RAV4 and ID.4.
```sql
SELECT BRAND, MODEL, COUNT(VIN) as COUNT_VIN
FROM tz_opendata_z01012023_po01012024 tozp 
WHERE MODEL LIKE 'LANOS' or MODEL LIKE 'RAV%' or MODEL LIKE 'ID.4%'
GROUP BY BRAND, MODEL
ORDER BY 3 desc;
```

- At this stage, I analyzed the types of car transactions that could be possible in principle. There are 106 types of car transactions in this dataset, but I am not interested in all of them. I am only interested in those transactions and their codes that indicate the purchase of a used car.
```sql
OPER_NAME, OPER_CODE, count(OPER_CODE) AS COUNT_CODE
FROM tz_opendata_z01012023_po01012024 tozp 
GROUP BY OPER_CODE, OPER_NAME
ORDER BY 3 desc;
```

- I chose the transaction codes that indicate the purchase of a used car.

Also, to display the map of Ukraine in Tableau, I created the file 'tz_opendata_reg' (this file can be downloaded). In this file, I registered all the codes of the car registration departments of Ukraine - 'DEP', and for each department I indicated the region of Ukraine in which it is located - 'REG'.

After that, I loaded the 'tz_opendata_reg' file into DBeaver and did a JOIN with the main file. I did this in order to see the region where the registration took place. 

As a result of these manipulations I saved the table I got as a CSV file for further work with it in Tableau

```sql
#Selection of transaction codes indicating the purchase of a used car 
SELECT tozp.OPER_NAME, tozp.OPER_CODE, tozp.D_REG, tozp.BRAND, tozp.MODEL, tozp.MAKE_YEAR, tozp.KIND, tozp.VIN, tz.REG as REG
FROM tz_opendata_z01012023_po01012024 tozp 
RIGHT JOIN tz_opendata_reg tz ON tozp.DEP=tz.DEP
WHERE OPER_CODE IN (315, 308, 100, 70, 71, 319, 329, 313, 310, 314, 331)
AND KIND LIKE 'ЛЕГКОВИЙ';
```
- But I still didn't have enough data to build the last visualization, where you can see the % of cars that were bought new and sold in the same year. To do this, I wrote a query to display only those car VIN codes that are present both among the cars that have registration codes as a new car and among those cars that have registration codes as a used car. That is, I have selected the transaction codes (used cars) that were re-registered during 2023, and whose VIN code matches the VIN code of cars purchased for the first time in 2023 (new cars) and whose production date is 2022 and 2023.

```sql
#Selection of VIN codes of new cars that were bought and sold in 2023
SELECT BRAND, MODEL, MAKE_YEAR, VIN
FROM tz_opendata_z01012023_po01012024 tozp 
WHERE OPER_CODE IN (315, 308, 100, 70, 71, 319,329, 313, 310, 314, 331) AND VIN IN (
                                        SELECT VIN
                                        FROM tz_opendata_z01012023_po01012024 tozp 
                                        WHERE OPER_CODE IN (99, 105, 69, 72) AND KIND LIKE 'ЛЕГКОВИЙ'
                                        AND MAKE_YEAR IN (2022, 2023))
```

## Visualization

![Alt text](https://github.com/igor-cp3/data-analytics-portfolio/blob/main/Analysis%20of%20used%20car%20sales%20in%20Ukraine%202023/Dashboard%20resize.png)


Better quality
https://public.tableau.com/app/profile/igor.lavrentiev/viz/UsedcarssalesUkrainedashboard/Dashboard1

## Conclusions
Based on the analysis of the dataset, I have made certain conclusions:

- The leaders of used car sales in Ukraine in 2023 are representatives of the German automobile concern VAG. Among the top 15 best-selling used cars, the first three are VAG cars, and 6 models out of 15 also belong to the VAG group. The presence of the VAZ brand in the top 15 best selling cars (5th position) indicates a rather low solvency of the population, who buy rather old and cheap cars.
   
- If we look at each model separately and determine the year of manufacture of the model that was most often bought in 2023, we see that these are quite old cars manufactured from 2006 to 2013. This fact also tells us about certain financial capabilities and solvency of the population.
  
- Looking at the car models with the highest sales in each region of Ukraine, we can see a certain trend. In Kharkiv, Donetsk, Zaporizhzhia, Kherson and Dnipro regions, the best-selling car is ZAZ-DEAWOO Lanos. This is a fairly budgetary and old car. One of the hypotheses is that the reason for this phenomenon is the hostilities and the direct front line that passed through these regions in 2023. In fact, Dnipropetrovsky region is surrounded by these regions where hostilities are taking place on 3 sides. These circumstances directly affect the availability of solvent population and the value and purpose of the respective car model.
  
- The latest visualization provides information on the top 20 new models that were most often bought by Ukrainians in 2023. As well as the percentage of sales of these models in the same year of 2023. The visualization shows certain outliers that stand out against the background of other indicators. Three car models have a significant percentage of sales in the same year. These are Volkswagen ID.4 - 13%, Toyota Land Cruiser - 12%, and BMW X5 - 18%. These indicators indicate a certain dissatisfaction of owners with their cars that they bought new.
  
In general, this analysis can be used by importers of spare parts and car maintenance services to formulate a spare parts procurement policy.




