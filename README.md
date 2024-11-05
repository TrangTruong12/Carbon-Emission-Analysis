# Carbon-Emission-Analysis
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
## Project's objective
### Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from [https://www.nature.com/] and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

### The reason we choose to do this project
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.
### Tools to use 
Online SQL Console: [https://api.swisscoding.edu.vn/sqlexec/?database=carbon_emissions&standalone=true]
## Mining
### Data parterns
Data have 171 Duplicate, query:
SELECT id, company_id,country_id,industry_group_id,year,product_name,weight_kg,carbon_footprint_pcf,upstream_percent_total_pcf,operations_percent_total_pcf,
downstream_percent_total_pcf
FROM product_emissions
GROUP BY id, company_id,country_id,industry_group_id,year,product_name,weight_kg,carbon_footprint_pcf,upstream_percent_total_pcf,operations_percent_total_pcf,
downstream_percent_total_pcf
having count(*) > 1

Data have N/A values

### Data structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![image](https://github.com/user-attachments/assets/21815dee-84ea-44f7-a3f6-59756c0ffda6)

### Solution
Write a query of group by and condition to filter out the value we dont want to have write the reason why we have the solution
## Findings

### 1. Which products contribute the most to carbon emissions?
SELECT 
	product_name, 
	SUM(carbon_footprint_pcf) AS Total_carbon_footprint
FROM
	product_emissions
GROUP BY 
	product_name
ORDER BY 
	Total_carbon_footprint DESC
LIMIT 10

Here the result:

| product_name                                                                                                                       | Total_carbon_footprint | 
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
### 2. What are the industry groups of these products?

SELECT 
    product_emissions.product_name, 
    industry_groups.industry_group,
    SUM(product_emissions.carbon_footprint_pcf) AS Total_carbon_footprint
FROM
    product_emissions
LEFT JOIN 
    industry_groups
    ON product_emissions.industry_group_id = industry_groups.id
GROUP BY 
    product_emissions.product_name, 
    industry_groups.industry_group
ORDER BY 
    Total_carbon_footprint DESC
LIMIT 10;

Here the result:

| product_name                                                                                                                       | industry_group                     | Total_carbon_footprint | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044                | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187                | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608                | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625                | 
| TCDE                                                                                                                               | Materials                          | 198150                 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687                 | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000                 | 
| Electric Motor                                                                                                                     | Capital Goods                      | 140647                 | 
| Audi A6                                                                                                                            | Automobiles & Components           | 111282                 | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | Automobiles & Components           | 100621                 | 

### 3. What are the industries with the highest contribution to carbon emissions?

SELECT 
    industry_groups.industry_group,
    SUM(product_emissions.carbon_footprint_pcf) AS total_carbon_emissions
FROM 
    product_emissions
JOIN 
    industry_groups ON product_emissions.industry_group_id = industry_groups.id
GROUP BY 
    industry_groups.industry_group
ORDER BY 
    total_carbon_emissions DESC
LIMIT 10

Here the result:
 | industry_group                                   | total_carbon_emissions | 
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

### 4. What are the companies with the highest contribution to carbon emissions?
SELECT 
    companies.company_name,
    SUM(product_emissions.carbon_footprint_pcf) AS total_carbon_emissions
FROM 
    product_emissions
JOIN 
    companies ON product_emissions.company_id = companies.id
GROUP BY 
    companies.company_name
ORDER BY 
    total_carbon_emissions DESC
	limit 10 
 
 Here the result:
 | company_name                            | total_carbon_emissions | 
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

### 5. What are the countries with the highest contribution to carbon emissions?

SELECT 
    countries.country_name,
    SUM(product_emissions.carbon_footprint_pcf) AS total_carbon_emissions
FROM 
    product_emissions
JOIN 
    countries ON product_emissions.country_id = countries.id
GROUP BY 
    countries.country_name
ORDER BY 
    total_carbon_emissions DESC
	limit 10 

Here the result:
| country_name | total_carbon_emissions | 
| -----------: | ---------------------: | 
| Spain        | 9786130                | 
| Germany      | 2251225                | 
| Japan        | 653237                 | 
| USA          | 518381                 | 
| South Korea  | 186965                 | 
| Brazil       | 169337                 | 
| Luxembourg   | 167007                 | 
| Netherlands  | 70417                  | 
| Taiwan       | 62875                  | 
| India        | 24574                  | 

### 6. What is the trend of carbon footprints (PCFs) over the years?
SELECT 
    year,
    SUM(carbon_footprint_pcf) AS total_carbon_emissions
FROM 
    product_emissions
GROUP BY 
    year
ORDER BY 
    year 

Here the result:
| year | total_carbon_emissions | 
| ---: | ---------------------: | 
| 2013 | 503857                 | 
| 2014 | 624226                 | 
| 2015 | 10840415               | 
| 2016 | 1640182                | 
| 2017 | 340271                 | 

### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?

min(year), max(year)
carbon of max(year)/carbon of min(year)
group by industry group

(carbon of max(year) - carbon of min(year))/((carbon of max(year) + carbon of min(year))

select from 
left join(select from) on year, industry = industry

select case when

max(year): current year
min(year): past year

SELECT 
	product_emissions.year,
	industry_groups.industry_group,
	sum(product_emissions.carbon_footprint_pcf) as total_carbon_emissions
FROM 
    product_emissions
JOIN 
	industry_groups ON product_emissions.industry_group_id = industry_groups.id
GROUP BY 
	industry_groups.industry_group,
	product_emissions.year
ORDER BY 
	industry_groups.industry_group,
	product_emissions.year 

## Conclusion
