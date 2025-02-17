### Problem Statement

This dashboard helps the sales person visualize reports that effectively communicate sales performance and trends. It helps them know the Profit made and compare profit for current year with profit for the prevoius year. Through charts, they get to know the most product sold and compare with sales for previous year . It also lets them know the top cities that made sales in a particular time. They can also see customers who made sales and compare their sales with the previous sales made.All the visual can be filterred by either months, years, city,products and/or channels.



### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a excel file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the columns errors & empty values were present. 
- Step 5 : In the report view, under the view tab, theme was selected.
- Step 6 : Since the data contains various ratings, thus in order to represent ratings, a new visual was added using the three ellipses in the visualizations pane in report view. 
- Step 7 : Visual filters (Slicers) were added for four fields named "Date", "City", "Product" & "Channel".
- Step 8 : Four card visuals were added to the canvas, representing average Sales, Profit,Profit Margin & Product Sold.Using visual level filter from the filters pane, basic filtering was used.
- Step 9 : Two bar charts were added to the report design area,first one representing sales current year vs sales previous year by the product name and another one representing sales current year vs sales previous year by months .                              
- Step 10 : Column Chart was added to compare sales by customers.
- Step 11 :Donut Chart was added to present top 5 cities that made most sales.
- Step 12 :lastly, line chart was added to compare profits by the channel. 


for creating new column following DAX expression was written;
       
    DAX DateTable = 
ADDCOLUMNS (
    //CALENDAR(DATE(2020,1,1), DATE(2024,12,31)),
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Quarter", "Q" & FORMAT(CEILING(MONTH([Date])/3, 1), "#"),
    "Quarter No", CEILING(MONTH([Date])/3, 1),
    "Month No", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Short Name", FORMAT([Date], "MMM"),
    "Month Short Name Plus Year", FORMAT([Date], "MMM,yy"),
    "DateSort", FORMAT([Date], "yyyyMMdd"),
    "Day Name", FORMAT([Date], "dddd"),
    "Details", FORMAT([Date], "dd-MMM-yyyy"),
    "Day Number", DAY ( [Date] )
)

the following measures were created.

//Measures Total Sales
Sales = SUM(Sales_Data[Sales])
//Measures Previous Year Toal Sales
Sales PY = CALCULATE([Sales], SAMEPERIODLASTYEAR(DateTable[Date]))
//Diffrence Between Current Year Sales & Previous Year Sales
Sales vs PY = [Sales] - [Sales PY]
//Percentage Increase or Decrease in sales year on year (YOY%)
Sales vs py % = DIVIDE([Sales vs PY],[Sales],0)
>> Products Sold = SUM(Sales_Data[Order Quantity])
>> Profit = SUM(Sales_Data[Profit]) 
>> Profit LY = CALCULATE([Profit], SAMEPERIODLASTYEAR(DateTable[Date]))
>> Profit Vs LY = [Profit]- [Profit LY]
>> Profit vs LY % = [Profit Vs LY]/[Profit]
>> Profit Margin = DIVIDE([Profit],[Sales],0)
>> Total Cost = SUM(Sales_Data[Total Cost])

 ### Some insights
 
 ### Sales
 
  3.63M was the total sales for 2017
  4M was the total sales for 2018
  3.49M was the total sales for 2019
 
 
 ### Profit

   1.41M was the total profir for 2017
   1.49M was the total profit for 2018
   1.29M was the total profit for 2019
 
  ### Sales by City
  
   Hamilton made most sales in 2017 with 25%
   Waitakere made most sales in 2018 with 23.2%
   Cristchurch made most sales in 2018 with 26%

