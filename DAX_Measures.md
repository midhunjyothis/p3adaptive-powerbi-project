# DAX Measures for Customer Return Analysis – Power BI

```DAX
First Return After FP =
VAR curCust = SELECTEDVALUE ( Customers[AltCustomerKey] )
VAR fp      = [FP]
RETURN
    CALCULATE (
        MIN ( Sales[OrderDate] ),
        FILTER(
            ALL ( Sales ),
            Sales[CustomerKey] = curCust
                && Sales[OrderDate] > fp
        )
    )

FP =
MIN ( Customers[DateFirstPurchase] )

Has Return 1-90 =
VAR fp  = [FP]
VAR ret = [First Return After FP]
VAR d   = IF ( NOT ISBLANK ( ret ), DATEDIFF ( fp, ret, DAY ) )
RETURN IF ( NOT ISBLANK ( ret ) && d <= 90, 1, 0 )

Has Return Next 3 Months =
VAR curCust  = SELECTEDVALUE ( Customers[AltCustomerKey] )
VAR fp       = [FP]
VAR startWin = EOMONTH ( fp, 0 ) + 1
VAR endWin   = EOMONTH ( fp, 3 )
VAR anyBuy =
    CALCULATE (
        COUNTROWS ( Sales ),
        FILTER (
            ALL ( Sales ),
            Sales[CustomerKey] = curCust
                && Sales[OrderDate] >= startWin
                && Sales[OrderDate] <= endWin
        )
    )
RETURN IF ( anyBuy > 0, 1, 0 )

Cohort Customers =
CALCULATE(
    DISTINCTCOUNT(Customers[AltCustomerKey]),
    USERELATIONSHIP('Date'[Date], Customers[DateFirstPurchase])
)

Customers Returned 1-90 =
CALCULATE (
    SUMX ( VALUES ( Customers[AltCustomerKey] ), [Has Return 1-90] ),
    USERELATIONSHIP ( 'Date'[Date], Customers[DateFirstPurchase] )
)

Customers Returned Next 3 Months =
CALCULATE (
    SUMX ( VALUES ( Customers[AltCustomerKey] ), [Has Return Next 3 Months] ),
    USERELATIONSHIP ( 'Date'[Date], Customers[DateFirstPurchase] )
)

Percentage of Customers First Return 1-90 =
DIVIDE ( [Customers Returned 1-90], [Cohort Customers], 0 )

Percentage of Customers First return Following 3 Months =
DIVIDE ( [Customers Returned Next 3 Months], [Cohort Customers], 0 )

Date =
VAR _minDate = CALCULATE(MIN(Sales[OrderDate]), REMOVEFILTERS(Sales))
VAR _maxDate = CALCULATE(MAX(Sales[OrderDate]), REMOVEFILTERS(Sales))
RETURN
ADDCOLUMNS(
    CALENDAR(_minDate, _maxDate),
    "Month Label MMM yy", FORMAT([Date], "MMM yy"),
    "Month Start", DATE(YEAR([Date]), MONTH([Date]), 1),
    "Year", YEAR([Date])
)
