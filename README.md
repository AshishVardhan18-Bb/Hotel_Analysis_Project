# Colingwood Estate Hotels-Dashboard

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiMDA3NjU4NzMtNzQ0NS00ZGVhLWI3OWEtNTQ5NjE5ZjlmYWM1IiwidCI6IjM0ODNhYmVlLWI2MmMtNDcwYS04MTQ2LTczYTAzOTk3NzZiYyJ9 

## Problem Statement

Colingwood Estate Hotels is a luxury brand hotels chain in south India. They have been in the
hospitality industry for 5 years. Colingwood Estate Hotel business is losing its money and market
share to their competitors due to their poor management decisions.
To fix this, the Managing Director of Colingwood Estate Hotels wants to use “Business and Data
Intelligence”. They do not have their own team to do this. So, They have decided to
approach a freelance BI Analyst to seek help, and use their past information to get
important insights

### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the columns errors & empty values were present.
- Step 5 : Now After seeing the data we need to create required metrics which would help the hotels to analyse their data 
- Step 6 : Let us create important measures which are used in the Report 

## Key Measures

 ### ADR (Average Daily Rate)
 ADR = DIVIDE( [Revenue], [Total Bookings],0)

### ADR wow Change %
ADR WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[week_number]),SELECTEDVALUE(dim_date[week_number]),MAX(dim_date[week_number]))

var revcw = CALCULATE([ADR],dim_date[week_number]= selv)

var revpw =  CALCULATE([ADR],FILTER(ALL(dim_date),dim_date[week_number]= selv-1))

return

DIVIDE(revcw,revpw,0)-1

### Average Ratings

Average Rating = AVERAGE(fact_bookings[ratings_given])

### Average Length of Stay

Average_Length_of_stay = AVERAGEX(fact_bookings, DATEDIFF(fact_bookings[check_in_date], fact_bookings[checkout_date], DAY))

### Booking % by Platform 

Booking % by Platform = DIVIDE([Total Bookings],

 CALCULATE([Total Bookings], 

 ALL(fact_bookings[booking_platform])
  ))

  ### Booking % by Room class

  Booking % by Room class = DIVIDE([Total Bookings],

 CALCULATE([Total Bookings], 

 ALL(dim_rooms[room_class])
  ))

### Cancellation % 

Cancellation % = DIVIDE([Total Cancellations],[Total Bookings],0)



### Daily Bookings Rate Number (DBRN)

DBRN = DIVIDE([Total Bookings], [No.Of.Days])

### Daily Supply Rate Number (DSRN)
DSRN = DIVIDE([Total Capacity], [No.Of.Days])

### DSRN WoW change %

DSRN WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[week_number]),SELECTEDVALUE(dim_date[week_number]),MAX(dim_date[week_number]))

var revcw = CALCULATE([DSRN],dim_date[week_number]= selv)

var revpw = CALCULATE([DSRN],FILTER(ALL(dim_date),dim_date[week_number]= selv-1))

return


DIVIDE(revcw,revpw,0)-1

###  Daily Utilization Rate Number (DURN)
DURN = DIVIDE([Total Checked Out],[No.Of.Days])

### No Show rate %
No Show rate % = DIVIDE([Total no show bookings],[Total Bookings])

### No.Of.Days

No.Of.Days = DATEDIFF(MIN(dim_date[date]),MAX(dim_date[date]),DAY) +1

### Occupancy%
Occupancy % = DIVIDE([Total Successful Bookings],[Total Capacity],0)

### Occupancy WoW change %

Occupancy WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[week no]),SELECTEDVALUE(dim_date[week_number]),MAX(dim_date[week_number]))

var revcw = CALCULATE([Occupancy %],dim_date[week_number]= selv)

var revpw =  CALCULATE([Occupancy %],FILTER(ALL(dim_date),dim_date[week_number]= selv-1))

return


DIVIDE(revcw,revpw,0)-1

### Realisation %
Realisation % = 1- ([Cancellation %]+[No Show rate %])

### Realisation WoW change %

Realisation WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[week_number]),SELECTEDVALUE(dim_date[week_number]),MAX(dim_date[week_number]))

var revcw = CALCULATE([Realisation %],dim_date[week_number]= selv)

var revpw =  CALCULATE([Realisation %],FILTER(ALL(dim_date),dim_date[week_number]= selv-1))

return


DIVIDE(revcw,revpw,0)-1

### Revenue 

Revenue = SUM(fact_bookings[revenue_realized])

### Revenue WoW change %
Revenue WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[week_number]),SELECTEDVALUE(dim_date[week_number]),MAX(dim_date[week_number]))

var revcw = CALCULATE([Revenue],dim_date[week_number]= selv)

var revpw =  CALCULATE([Revenue],FILTER(ALL(dim_date),dim_date[week_number]= selv-1))

return

DIVIDE(revcw,revpw,0)-1

### RevPAR

RevPAR = DIVIDE([Revenue],[Total Capacity])

### Revpar WoW change %
Revpar WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[week_number]),SELECTEDVALUE(dim_date[week_number]),MAX(dim_date[week_number])) 

var revcw = CALCULATE([RevPAR],dim_date[week_number]= selv)

var revpw =  CALCULATE([RevPAR],FILTER(ALL(dim_date),dim_date[week_number]= selv-1))

return


DIVIDE(revcw,revpw,0)-1

### Total Bookings
Total Bookings = count(fact_bookings[booking_id])

### Total Cancellations

Total Cancellations = CALCULATE([Total Bookings],fact_bookings[booking_status] = "Cancelled")

### Total Capacity

Total Capacity = sum(fact_aggregrated_bookings[capacity])

### Total Checked Out

Total Checked Out = CALCULATE([Total Bookings],fact_bookings[booking_status]="Checked Out")

### Total no show bookings
Total no show bookings = CALCULATE([Total Bookings],fact_bookings[booking_status]="No Show")

### Total Successful Bookings
Total Successful Bookings = sum(fact_aggregrated_bookings[successful_bookings])



- Step 7 : Now let us start the visualising part let us take few key metrics and make them as KPI Icons in our Key metrics Table

### ADR_WoW_Icon

ADR_WoW_Icon = 

VAR _positiveIcon = UNICHAR ( 9650 )

VAR _neutralIcon = UNICHAR ( 9632 )

VAR _negativeIcon = UNICHAR ( 9660 )

VAR _result =

    IF (
        [ADR WoW Change %] < 0,

        _negativeIcon,

        IF ( [ADR WoW Change %] = 0, _neutralIcon, _positiveIcon )
    )

RETURN

    _result

### DSRN_WoW_Icon

DSRN_WoW_Icon = 

VAR _positiveIcon = UNICHAR ( 9650 )

VAR _neutralIcon = UNICHAR ( 9632 )

VAR _negativeIcon = UNICHAR ( 9660 )

VAR _result =

    IF (
        [DSRN WoW Change %] < 0,
        _negativeIcon,
        IF ( [DSRN WoW Change %] = 0, _neutralIcon, _positiveIcon )
    )
RETURN
    _result

### Occupancy%_WoW_Icon

Occupancy%_WoW_Icon = 

VAR _positiveIcon = UNICHAR ( 9650 )

VAR _neutralIcon = UNICHAR ( 9632 )

VAR _negativeIcon = UNICHAR ( 9660 )

VAR _result =

    IF (
        [Occupancy WoW Change %] < 0,
        _negativeIcon,
        IF ( [Occupancy WoW Change %] = 0, _neutralIcon, _positiveIcon )
    )
RETURN
    _result

### Realisation_WoW_Icon

Realisation_WoW_Icon = 

VAR _positiveIcon = UNICHAR ( 9650 )

VAR _neutralIcon = UNICHAR ( 9632 )

VAR _negativeIcon = UNICHAR ( 9660 )

VAR _result =

    IF (
        [Realisation WoW Change %] < 0,
        _negativeIcon,
        IF ( [Realisation WoW Change %] = 0, _neutralIcon, _positiveIcon )
    )
RETURN

    _result



### Revenue_WoW_Icon

Revenue_WoW_Icon = 

VAR _positiveIcon = UNICHAR ( 9650 )

VAR _neutralIcon = UNICHAR ( 9632 )

VAR _negativeIcon = UNICHAR ( 9660 )

VAR _result =

    IF (
        [Revenue WoW Change %] < 0,
        _negativeIcon,
        IF ( [Revenue WoW Change %] = 0, _neutralIcon, _positiveIcon )
    )
RETURN

    _result

### Revpar_WoW_Icon = 

VAR _positiveIcon = UNICHAR ( 9650 )

VAR _neutralIcon = UNICHAR ( 9632 )

VAR _negativeIcon = UNICHAR ( 9660 )

VAR _result =

    IF (
        [Revpar WoW Change %] < 0,
        _negativeIcon,
        IF ( [Revpar WoW Change %] = 0, _neutralIcon, _positiveIcon )
    )
RETURN

    _result

### Snap of Key metrics

![image](https://github.com/user-attachments/assets/de55e0d6-0399-4a14-ad6c-9b2e5df18168)

- Step 8 : 

The Next step is to make the visuals acoording to the data given and analyze

- step 9 :
 Create a Line chart which shows the Trends of the Key Metrics and give the date column according to the weeks and we can use the slicers if we want to get the trends of the data on some particular dates or on some particular months.

![image](https://github.com/user-attachments/assets/ec8a7f16-4ec3-48ca-9c0f-2708d148b8e0)

- Step 10 : Let us create a table which shows the property by key metrics include the property name and property id with the key metrics

![image](https://github.com/user-attachments/assets/2629c2ac-fec8-4ba5-b5d8-3cb813f7c47e)

- Step 11 : Let us create the pie chart showing the revenue generated by the different categories 

- step 12: Next let us create the tree map showing the top 5 revenue genrators in our field

- step 13 : Let us create the line charts for Revenue, Relaisation, Cancellation, Occupancy percentages across the date table showing the variance.

_ step 14 : The Last step is to create tool tip visuals for the key metrics.
So, when we Hoover across the key metrics kpi icons then we will be able to see the tool tip visual which is a line chart visual based on the particular key Metric showing the data in the weekend and weekdays




# Snapshot of Dashboard (Power BI Service)

![image](https://github.com/user-attachments/assets/51864cfe-5359-4869-9181-84183ca0e3ac)


# Insights

A single page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

### Highest Revenue Generators in Room_Class

 Deluxe - 554M

 Junior Suite - 456M

 Presidential Suite - 372M

 Studio - 306M

 ### Highest Revenue Generated by Cities

 Mumbai - 0.66bn

 Bangalore - 0.42bn

 Hyderabad - 0.32bn

 Goa - 0.29bn


 ### Booking Platform used 

Direct Offline Booking - (5.06%)

Travelex - (6.01%)

Booker - (7.19%)

Direct Online - (9.88%)

GoTrip - (10.99%)

MakeYourTrip - (19.97%)

Others - (40.91%)


### Revenue Generated By Category

Business Category - 38.38%

Leisure Category - 61.62%


These are the insights and the remaining insights can be only seen through the visuals. So, just hoover across the visual and select wanted date or anything and jsut see the corresponding values
