
# Adventure Wroks 

## Version 1.0.0 

## Features 
1. ETL Completed


## Power BI Course Practice Steps

## PowerQuery ELT Steps V1.0.0
1. Add Parameters in Parameters group
    1. DataFilesPath
    1. SalesDataFilesSearchKey
    1. CalendarStartDate and CalendarEndDate for Calendar Lookup Table
1. Add Helper Functions Group
    1. Add function ExtractTablesFromCSV = Function to read csv document and promote first rows to headers
    1. Add function GenerateCalendarTable = function to generate Calendar Lookup table with start and end date as parameters
1. Create a Column Mapping Model
    1. Generate a table of colun names for Sales that will be included in the model. Also to map column names i.e. OrderNumber -> Order Numbe 
1. Add a configuration Group   
    1. Generate SalesDataColumnNamesList = Source to Destination filter List of Lists. Disable list from loading into model  
1. Import Customer Lookup , Product Categories Lookup , Product Lookup, Product subcategories lookup and Returns Data
    1. Fix Erorrs and remove Empty customer ids in Customer Lookup
1. Generate Sales Data Table
    1. Custom Query to import and combine sales data from 3 files and rename columns to mapped column names in SalesDataColumnNamesList
1. Add Measures Table
1. Change types of all columns in Sales Data table

## Section 6 : Data Model - V1.1.0
1. Create 1 to many relationships
1. Hide all foreign keys in Sales , Returns , Product and Product subcategory tables 
1. Convert Field Formats and Categories
    1. Convert date formats in Calendar , Sales and Returns table to short date, 
    1. Assign geographical categories to fields in Territory Lookup
    1. Change currency formats in product table
1. Hierarchies
    1. Date Hierarchy Start of Year , Start of Month , Start of Week, Day 
    1. Territory Hierarchy -> Continent->Region->Country
1. Sort By Column
    1. Calendar Table : Columns "Month Name" and "Short Month" Sort By Column "Month"
    1. Calendar Table : Column "Day Name" Sort By Column "Day Of Week"

## Measures - V1.2.0
1. Add Measures (steps)
    1. Basic Math, Counting, Stat Functions
        1. Quantity Sold
        1. Quantity Returned 
        1. Total Customers
        1. Return Rate
        1. Total Orders
        1. Total Returns
        1. Avergae Retail Price
    1.  Logical functions (Switch)
        1. Customer Priority = annual income >100K and is a parent priority = high else standard
        1. Income Level : >=150K ver high , >100K high , >=50k average else low
        1. Education Category
    1. Text Functions : Left , Find 
        1. Month Short
        1. SKU Category
    1. Date functions : Year , IN 
        1. Weekend in Calendar table
        1. Birth Year in Customer Lookup table

    1. Related
        1. Add Revenue Column to Sales 
    1. Calculate
        1. Bulk Orders (order quantity > 1)
        1. Weekend Orders
        1. Red Sales
        1. Total Orders (by product category) - goodly solution
        1. Bike Returns , Bike Sales , Bike Return Rate
    1. All
        1. All Orders , % of All Orders
        1. Average Retial Price
        1. Overall Average Retail Price
        1. All Returns , % of All Returns 
    1. Filter
        1. High Ticket Orders (orders where price > Overall Average Price)
    1. Iterator functions SUMX
        1. Total Revenue
        1. Average Revenue Per Customer
        1. Total Cost
        1. Total Profit
    1. Time Functions
        1. YTD Revenue
        1. Previous month Revenue
        1. Revenue Target
        1. 10 day rolling revenue

1. Add measures (All)
    1. Sales Quanity, Returned Quanity
    1. % of All Orders
    1. % of All Returns
    1. 10-day Rolling Revenue
    1. 90-day Rolling Profit 
    1. Adjusted Price
    1. Adjusted Profit
    1. Adjusted Revenue 
    1. All Orders
    1. All Returns
    1. Average Retail Price
    1. Average Revenue Per Customer
    1. Bike Return Rate
    1. Bike Returns
    1. Bike Sales
    1. Bulk Orders
    1. High Ticket Orders
    1. Order Target 
    1. Order Target Gap
    1. Overall Average  Price
    1. Previous Month Orders
    1. Previous Month Profit 
    1. Previous Month Returns
    1. Previous Month Revenue
    1. Profit Target
    1. Profit Target Gap
    1. Quantity Returned
    1. Quantity Sold
    1. Return Rate
    1. Revenue Target
    1. Revenue Target Gap
    1. Total Cost
    1. Total Customers
    1. Total Orders
    1. Total Profit
    1. Total Returns
    1. Total Revenue
    1. Weekend Orders
    1. YTD Revenue


## Backup Code

```
let
    Source = Folder.Files("C:\Users\vishal\Documents\Learning\PowerBI\Microsoft Power BI Desktop for Business Intelligence\AdventureWorks Raw Data\Sales Data"),
    #"Filtered Hidden Files1" = Table.SelectRows(Source, each [Attributes]?[Hidden]? <> true),
    #"Invoke Custom Function1" = Table.AddColumn(#"Filtered Hidden Files1", "Transform File", each #"Transform File"([Content])),
    #"Renamed Columns1" = Table.RenameColumns(#"Invoke Custom Function1", {"Name", "Source.Name"}),
    #"Removed Other Columns1" = Table.SelectColumns(#"Renamed Columns1", {"Source.Name", "Transform File"}),
    #"Expanded Table Column1" = Table.ExpandTableColumn(#"Removed Other Columns1", "Transform File", Table.ColumnNames(#"Transform File"(#"Sample File"))),
    #"Changed Type" = Table.TransformColumnTypes(#"Expanded Table Column1",{{"Source.Name", type text}, {"OrderDate", type date}, {"StockDate", type date}, {"OrderNumber", type text}, {"ProductKey", Int64.Type}, {"CustomerKey", Int64.Type}, {"TerritoryKey", Int64.Type}, {"OrderLineItem", Int64.Type}, {"OrderQuantity", Int64.Type}})
in
    #"Changed Type"

```

```

    let
    Source = Folder.Files("C:\Users\vishal\Documents\Learning\PowerBI\Microsoft Power BI Desktop for Business Intelligence\AdventureWorks Raw Data\Sales Data"),
    #"Filtered Hidden Files1" = Table.SelectRows(Source, each [Attributes]?[Hidden]? <> true),
    #"Invoke Custom Function1" = Table.AddColumn(#"Filtered Hidden Files1", "Transform File", each #"Transform File"([Content])),
    #"Renamed Columns1" = Table.RenameColumns(#"Invoke Custom Function1", {"Name", "Source.Name"}),
    #"Removed Other Columns1" = Table.SelectColumns(#"Renamed Columns1", {"Source.Name", "Transform File"}),
    #"Expanded Table Column1" = Table.ExpandTableColumn(#"Removed Other Columns1", "Transform File", Table.ColumnNames(#"Transform File"(#"Sample File"))),
    #"Changed Type" = Table.TransformColumnTypes(#"Expanded Table Column1",{{"Source.Name", type text}, {"OrderDate", type date}, {"StockDate", type date}, {"OrderNumber", type text}, {"ProductKey", Int64.Type}, {"CustomerKey", Int64.Type}, {"TerritoryKey", Int64.Type}, {"OrderLineItem", Int64.Type}, {"OrderQuantity", Int64.Type}})
in
    #"Changed Type"

```

```
// power query read from folder
let
    Source = Folder.Files("C:\Users\vishal\Documents\Learning\PowerBI\Microsoft Power BI Desktop for Business Intelligence\AdventureWorks Raw Data\Sales Data"),
    #"Filtered Hidden Files1" = Table.SelectRows(Source, each [Attributes]?[Hidden]? <> true),
    #"Invoke Custom Function1" = Table.AddColumn(#"Filtered Hidden Files1", "Transform File", each #"Transform File"([Content])),
    #"Renamed Columns1" = Table.RenameColumns(#"Invoke Custom Function1", {"Name", "Source.Name"}),
    #"Removed Other Columns1" = Table.SelectColumns(#"Renamed Columns1", {"Source.Name", "Transform File"}),
    #"Expanded Table Column1" = Table.ExpandTableColumn(#"Removed Other Columns1", "Transform File", Table.ColumnNames(#"Transform File"(#"Sample File"))),
    #"Changed Type" = Table.TransformColumnTypes(#"Expanded Table Column1",{{"Source.Name", type text}, {"OrderDate", type date}, {"StockDate", type date}, {"OrderNumber", type text}, {"ProductKey", Int64.Type}, {"CustomerKey", Int64.Type}, {"TerritoryKey", Int64.Type}, {"OrderLineItem", Int64.Type}, {"OrderQuantity", Int64.Type}})
in
    #"Changed Type"

```

```
let
    Source = (Parameter1) => let
        Source = Csv.Document(Parameter1,[Delimiter=",", Columns=8, Encoding=1252, QuoteStyle=QuoteStyle.None]),
        #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true])
    in
        #"Promoted Headers"
in
    Source 

```

1. Calendar Lookup

```
let
    Source = Csv.Document(File.Contents("C:\Users\vishal\Documents\Learning\PowerBI\Microsoft Power BI Desktop for Business Intelligence\AdventureWorks Raw Data\AdventureWorks Calendar Lookup.csv"),[Delimiter=",", Columns=1, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date", type date}}),
   
   "Start of Week", each Date.StartOfWeek([Date],Day.Monday)),
   "Start of Month", each Date.StartOfMonth([Date]), type date),
   "Start of Quarter", each Date.StartOfQuarter([Date]), type 
    date),
   "Start of Year", each Date.StartOfYear([Date]), type date),
   
    #"Changed Type with Locale" = Table.TransformColumnTypes(#"Inserted Start of Quarter", {{"Date", type date}}, "en-US"),
    #"Inserted Month" = Table.AddColumn(#"Changed Type with Locale", "Month", each Date.Month([Date]), Int64.Type),
    #"Inserted Year" = Table.AddColumn(#"Inserted Start of Year", "Year", each Date.Year([Date]), Int64.Type),
   
in
    #"Inserted Month Name"

```

```
//Rolling Calendar
let
    Source = #date(2023,1,1),
    Custom1 = List.Dates(
        Source,Number.From(DateTime.LocalNow()) - Number.From(Source), 
        #duration(1,0,0,0)
    ),
    #"Converted to Table" = Table.FromList(Custom1, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Renamed Columns" = Table.RenameColumns(#"Converted to Table",{{"Column1", "Date"}}),
    #"Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"Date", type date}}),
    #"Inserted Year" = Table.AddColumn(#"Changed Type", "Year", each Date.Year([Date]), Int64.Type),
    #"Inserted Start of Quarter" = Table.AddColumn(#"Inserted Year", "Start of Quarter", each Date.StartOfQuarter([Date]), type date),
    #"Inserted Start of Month" = Table.AddColumn(#"Inserted Start of Quarter", "Start of Month", each Date.StartOfMonth([Date]), type date) 
in
    #"Inserted Start of Month"

```

####################################################################################################################
# Code for Custom Queries Adnevture Works

1. Rolling Calendar

```
let
    startDate = #date(Date.Year(StartDate),Date.Month(StartDate),Date.Day(StartDate)),
    count = Number.From(DateTime.LocalNow()) - Number.From(startDate),
    duration = #duration(1,0,0,0),
    listOfDates = List.Dates(startDate,count,duration),
    dateTable = Table.FromList(listOfDates,Splitter.SplitByNothing()),
    #"Renamed Columns" = Table.RenameColumns(dateTable,{{"Column1", "Date"}}),
    #"Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"Date", type date}}),
    #"Inserted Day Name" = Table.AddColumn(#"Changed Type", "Day Name", each Date.DayOfWeekName([Date]), type text),
    #"Inserted Month Name" = Table.AddColumn(#"Inserted Day Name", "Month Name", each Date.MonthName([Date]), type text),
    #"Inserted Start of Week" = Table.AddColumn(#"Inserted Month Name", "Start of Week", each Date.StartOfWeek([Date]), type date),
    #"Inserted Start of Month" = Table.AddColumn(#"Inserted Start of Week", "Start of Month", each Date.StartOfMonth([Date]), type date),
    #"Inserted Start of Quarter" = Table.AddColumn(#"Inserted Start of Month", "Start of Quarter", each Date.StartOfQuarter([Date]), type date),
    #"Inserted Start of Year" = Table.AddColumn(#"Inserted Start of Quarter", "Start of Year", each Date.StartOfYear([Date]), type date),
    #"Inserted Month" = Table.AddColumn(#"Inserted Start of Year", "Month", each Date.Month([Date]), Int64.Type),
    #"Inserted Quarter" = Table.AddColumn(#"Inserted Month", "Quarter", each Date.QuarterOfYear([Date]), Int64.Type),
    #"Inserted Year" = Table.AddColumn(#"Inserted Quarter", "Year", each Date.Year([Date]), Int64.Type)
in
    #"Inserted Year"

```

1. ExtractTablesFromCSV function 

```
let
    Source = (parameter1)=> let
        step1 = Csv.Document(parameter1,[Delimiter=",", Columns=8, Encoding=1252, QuoteStyle=QuoteStyle.None]),
        step2 = Table.PromoteHeaders(step1,[PromoteAllScalars=true]),
        returnTable = step2

    in
        returnTable
in
    Source

```

