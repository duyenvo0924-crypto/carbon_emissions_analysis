# carbon_emissions_analysis

## Introduction
<img width="640" height="427" alt="image" src="https://github.com/user-attachments/assets/54f82527-e91c-41e2-8ba9-731fa50f6515" />
This project aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

## Data overview
### Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
<img width="687" height="487" alt="image" src="https://github.com/user-attachments/assets/e97fbb36-836d-42f6-9aa5-59bbd9db98aa" />
### Data Detail
#### Table 'product_emissions'
Query
SELECT * FROM product_emissions
LIMIT 5;
Result:
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

#### Table 'industry_groups'
Query
SELECT * FROM industry_groups
LIMIT 5;
Result:
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

#### Table 'companies'
Query
SELECT * FROM companies
LIMIT 5;
Result:
| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 

#### Table 'countries'
Query
SELECT * FROM countries
LIMIT 5;
Result:
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 


#### Q1.Which products contribute the most to carbon emissions?
Query
SELECT product_name,
    SUM(carbon_footprint_pcf) AS total_carbon_emissions
FROM product_emissions
GROUP BY product_name
ORDER BY total_carbon_emissions DESC
LIMIT 10;

Result:
| product_name                                                                                                                       | total_carbon_emissions | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044                | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187                | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608                | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625                | 
| TCDE                                                                                                                               | 198150                 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687                 | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000                 | 
| Electric Motor                                                                                                                     | 160655                 | 
| Audi A6                                                                                                                            | 111282                 | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | 100621                 | 


#### Q2.What are the industry groups of these products?
Query
SELECT 
    product_name,
	industry_group_id,
    SUM(carbon_footprint_pcf) AS total_carbon_emissions
FROM product_emissions
GROUP BY product_name
ORDER BY total_carbon_emissions DESC
LIMIT 10;

Result:
| product_name                                                                                                                       | industry_group_id | total_carbon_emissions | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ----------------: | ---------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 13                | 3718044                | 
| Wind Turbine G132 5 Megawats                                                                                                       | 13                | 3276187                | 
| Wind Turbine G114 2 Megawats                                                                                                       | 13                | 1532608                | 
| Wind Turbine G90 2 Megawats                                                                                                        | 13                | 1251625                | 
| TCDE                                                                                                                               | 19                | 198150                 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 7                 | 191687                 | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 19                | 167000                 | 
| Electric Motor                                                                                                                     | 8                 | 160655                 | 
| Audi A6                                                                                                                            | 7                 | 111282                 | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | 7                 | 100621                 | 

#### Q3.What are the industries with the highest contribution to carbon emissions?
Query
SELECT 
    ig.industry_group AS industry,
    SUM(pe.carbon_footprint_pcf) AS total_carbon_emissions
FROM product_emissions pe
JOIN industry_groups ig 
    ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY total_carbon_emissions DESC
LIMIT 10;

Result:
| industry                                         | total_carbon_emissions | 
| -----------------------------------------------: | ---------------------: | 
| Electrical Equipment and Machinery               | 9801558                | 
| Automobiles & Components                         | 2582264                | 
| Materials                                        | 577595                 | 
| Technology Hardware & Equipment                  | 363776                 | 
| Capital Goods                                    | 258712                 | 
| "Food, Beverage & Tobacco"                       | 111131                 | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486                  | 
| Chemicals                                        | 62369                  | 
| Software & Services                              | 46544                  | 
| Media                                            | 23017                  | 

#### Q4. What are the companies with the highest contribution to carbon emissions?
Query
SELECT 
    c.company_name AS company,
    SUM(pe.carbon_footprint_pcf) AS total_carbon_emissions
FROM product_emissions pe
JOIN companies c 
    ON pe.company_id = c.id
GROUP BY c.company_name
ORDER BY total_carbon_emissions DESC
LIMIT 10;

Result:
| company                                 | total_carbon_emissions | 
| --------------------------------------: | ---------------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464                | 
| Daimler AG                              | 1594300                | 
| Volkswagen AG                           | 655960                 | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016                 | 
| "Hino Motors, Ltd."                     | 191687                 | 
| Arcelor Mittal                          | 167007                 | 
| Weg S/A                                 | 160655                 | 
| General Motors Company                  | 137007                 | 
| "Lexmark International, Inc."           | 132012                 | 
| "Daikin Industries, Ltd."               | 105600                 | 

#### Q5. What are the countries with the highest contribution to carbon emissions?
Query
SELECT 
    c.country_name AS country,
    SUM(pe.carbon_footprint_pcf) AS total_carbon_emissions
FROM product_emissions pe
JOIN countries c 
    ON pe.country_id = c.id
GROUP BY c.country_name
ORDER BY total_carbon_emissions DESC
LIMIT 10;

Result:
| country     | total_carbon_emissions | 
| ----------: | ---------------------: | 
| Spain       | 9786130                | 
| Germany     | 2251225                | 
| Japan       | 653237                 | 
| USA         | 518381                 | 
| South Korea | 186965                 | 
| Brazil      | 169337                 | 
| Luxembourg  | 167007                 | 
| Netherlands | 70417                  | 
| Taiwan      | 62875                  | 
| India       | 24574                  | 


#### Q6. What is the trend of carbon footprints (PCFs) over the years?
Query
SELECT 
    year,
    SUM(carbon_footprint_pcf) AS total_carbon_emissions
FROM product_emissions
GROUP BY year
ORDER BY year;

Result:
| year | total_carbon_emissions | 
| ---: | ---------------------: | 
| 2013 | 503857                 | 
| 2014 | 624226                 | 
| 2015 | 10840415               | 
| 2016 | 1640182                | 
| 2017 | 340271                 | 


#### Q6. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
Query



