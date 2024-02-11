# HR Analyst(Retention)

## Introduction
Welcome to the AI Variant Data Analyst Internship Project: Employee Retention! This project was completed during an internship at AI Variant after completing the Data Analyst course at ExcelR. The project revolves around HR Analytics and focuses on analyzing employee retention using two datasets, HR_1 and HR_2, each containing 50,000 records.

## Project Overview
- **Domain:** HR Analytics
- **Project Name:** Employee Retention
- **Dataset Names:** HR_1 & HR_2
- **Dataset Type:** Excel Data
- **Dataset Size:** 50,000 records each

## Key Performance Indicators (KPIs)
The project emphasizes six key performance indicators (KPIs) to gain insights into employee retention and related factors:

1. Average Attrition rate for all Departments.
2. Average Hourly rate of Male Research Scientist.
3. Attrition rate Vs Monthly income stats.
4. Average working years for each Department.
5. Job Role Vs Work life balance.
6. Attrition rate Vs Year since last promotion relation.

## Tools used for Visulize
- Excel
- Power BI
- Tableau
- MySql

## A Roadmap to Effective Analysis
- Define the Question 		
- Collect & import the data 
- Clean the data by data modeling
- Analyze the data
- Visualize reports	& share insights

## Dashboard Creation
### Excel Dashboard
The Excel dashboard provides a clear and organized platform for visualizing critical HR Analytics insights. It utilizes various data visualization components such as charts, graphs, and PivotTables to represent key performance indicators (KPIs) effectively. User-friendly features like dropdown menus, data filtering, and dynamic updates enhance the user experience.
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/9482ef76-2ecc-4a39-b4e8-4ad0e5407745)
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/517b6915-a46f-459b-beb7-42064fa0fa92)

### Power BI Dashboard
The Power BI dashboard offers an immersive and interactive data exploration experience, allowing users to uncover insights and trends within the HR dataset. Its intuitive layout balances visualizations and filtering options, ensuring clarity and responsiveness. Here are some key features of the Power BI dashboard:
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/09a54da2-6613-478b-952c-b7330823bdcb)


### Tableau Dashboard
The Tableau dashboard provides an interactive platform for in-depth HR Analytics, facilitating a deeper understanding of employee retention patterns. Its modern design integrates a variety of data visualization components, including bar charts, donut charts, and more, to depict KPIs. Here are some highlights of the Tableau dashboard:
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/0980bf5e-a134-43e2-b421-955ae9af6b89)

## MySQL
The HR dataset was imported into an SQL database an HR database is a system where you store and manage data on your company's employees. HR databases can be used to track a variety of information, including HR metrics, which give the HR team insights for better decision-making.

### KPI 1: Average Attrition rate for all Departments
    CREATE
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
    VIEW `kpi_1` AS
    SELECT `a`.`Department` AS `Department`,CONCAT(FORMAT((AVG(`a`.`Attrition_Y`) * 100),2),'%') AS `Attrition_Rate`
    FROM
    (SELECT `hr_1`.`Department` AS `Department`,`hr_1`.`Attrition` AS `Attrition`,
    (CASE WHEN (`hr_1`.`Attrition` = 'Yes') THEN 1
    ELSE 0 END) AS `Attrition_Y`
    FROM `hr_1`) `a`
    GROUP BY `a`.`Department`;
    
    select * from kpi_1
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/31e899b5-07bb-42a4-a912-dac48471a48e)

### KPI 2: Average Hourly rate of Male Research Scientist
    CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
    VIEW `kpi_2` AS
    SELECT AVG(`hr_1`.`HourlyRate`) AS `AverageHourlyRate`,`hr_1`.`Gender` AS `Gender`,`hr_1`.`JobRole` AS `JobRole`
    FROM `hr_1`
    WHERE((`hr_1`.`JobRole` = 'Research Scientist')
    AND (`hr_1`.`Gender` = 'Male'))
    GROUP BY `hr_1`.`Gender` , `hr_1`.`JobRole`;
    
    select * from kpi_2;
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/3e52bc08-7363-496f-9b74-754a282c12aa)

### KPI 3: Attrition rate Vs Monthly income stats
    CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
    VIEW `kpi_3` AS
    SELECT `a`.`Department` AS `Department`,CONCAT(FORMAT((AVG(`a`.`Attrition_Rate`) * 100),2),'%') AS `Average_Attrition`,
    FORMAT(AVG(`b`.`MonthlyIncome`), 2) AS `Average_Monthly_Income`
    FROM
    ((SELECT `hr_1`.`Department` AS `Department`,`hr_1`.`Attrition` AS `Attrition`,`hr_1`.`EmployeeNumber` AS `EmployeeNumber`,
    (CASE WHEN (`hr_1`.`Attrition` = 'Yes') THEN 1 ELSE 0 END) AS `Attrition_Rate`
    FROM `hr_1`) `a` INNER JOIN `hr_2` `b` ON ((`b`.`employeenumber` = `a`.`EmployeeNumber`)))
    GROUP BY `a`.`Department`;
    
    select * from kpi_3;
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/c1574318-f4e7-4675-8152-10b956ac61d3)

### KPI 4: Average working years for each Department
    CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
    VIEW `kpi_4` AS
    SELECT `a`.`Department` AS `Department`,FORMAT(AVG(`b`.`TotalWorkingYears`), 1) AS `Average_Working_Year`
    FROM (`hr_1` `a` JOIN `hr_2` `b` ON ((`b`.`employeenumber` = `a`.`EmployeeNumber`)))
    GROUP BY `a`.`Department`;
    
    select * from kpi_4;
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/4a786969-19cd-4f6e-b6e2-2438f413813f)

### KPI 5: Job Role Vs Work life balance
    CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
    VIEW `kpi_5` AS
    SELECT `a`.`JobRole` AS `JobRole`,
    SUM((CASE WHEN (`b`.`PerformanceRating` = 1) THEN 1 ELSE 0 END)) AS `1st_Rating_Total`,
    SUM((CASE WHEN (`b`.`PerformanceRating` = 2) THEN 1 ELSE 0 END)) AS `2nd_Rating_Total`,
    SUM((CASE WHEN (`b`.`PerformanceRating` = 3) THEN 1 ELSE 0 END)) AS `3rd_Rating_Total`,
    SUM((CASE WHEN (`b`.`PerformanceRating` = 4) THEN 1 ELSE 0 END)) AS `4th_Rating_Total`,
    COUNT(`b`.`PerformanceRating`) AS `Total_Employee`,
    FORMAT(AVG(`b`.`WorkLifeBalance`), 2) AS `Average_WorkLifeBalance_Rating`
    FROM (`hr_1` `a` JOIN `hr_2` `b` ON ((`b`.`employeenumber` = `a`.`EmployeeNumber`)))
    GROUP BY `a`.`Job Role`;
        
    select * from kpi_5;
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/df98a80a-cc82-49a3-9ab7-54f39d5d4cf9)

### KPI 6: Attrition rate Vs Year since last promotion relation
    CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
    VIEW `kpi_6` AS
    SELECT `a`.`JobRole` AS `JobRole`,CONCAT(FORMAT((AVG(`a`.`Attrition_Rate`) * 100),2),'%') AS `Average_Attrition_Rate`,
    FORMAT(AVG(`b`.`YearsSinceLastPromotion`), 2) AS `Average_YearsSinceLastPromotion`
    FROM
    ((SELECT `hr_1`.`JobRole` AS `JobRole`,`hr_1`.`Attrition` AS `Attrition`,`hr_1`.`EmployeeNumber` AS `EmployeeNumber`,
    (CASE WHEN (`hr_1`.`Attrition` = 'Yes') THEN 1 ELSE 0 END) AS `Attrition_Rate`
    FROM `hr_1`) `a` JOIN `hr_2` `b` ON ((`b`.`employeenumber` = `a`.`EmployeeNumber`)))
    GROUP BY `a`.`JobRole`;
    
    select * from kpi_6;
![image](https://github.com/Harmanprits/HR-Analyst/assets/142983120/a938f9bb-ccde-4e09-9c2f-1f31417de1a4)

## Conclusion-
- Attrition rates are high because the **attrition ratio is above 50%** which is
not a good sign for organization to conclusion of this problem **Surveys
need to be done** regularly and identify the high potential employees and
create development plans for top talent.
- By training and development, you can make feel valued and interested in
their career growth which can lead to **higher retention rate**.
- Employees are leaving their jobs because of the **lack of facilities** provided
by the organization. To improve this, must improve the facilities for
employees also the average distance from home is **26 km** to provide bus
facilities. Provide a positive work environment and maintain **good work
life balance.**

