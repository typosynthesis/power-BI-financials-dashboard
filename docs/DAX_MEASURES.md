# DAX Measures Reference

## Core KPIs

```dax
Total Sales = SUM(financials[Sales])

Total Profit = SUM(financials[Profit])

Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)

Total Discount Given = SUM(financials[Discounts])

Total Units Sold = SUM(financials[Units Sold])
```

## Time Intelligence

```dax
MoM Sales Change % = 
VAR CurrentYear = MAX(financials[Year])
VAR CurrentMonth = MAX(financials[Month Number])
VAR PrevYear = IF(CurrentMonth = 1, CurrentYear - 1, CurrentYear)
VAR PrevMonth = IF(CurrentMonth = 1, 12, CurrentMonth - 1)
VAR CurrentSales = [Total Sales]
VAR PreviousSales = 
    CALCULATE(
        [Total Sales],
        REMOVEFILTERS(financials),
        financials[Year] = PrevYear,
        financials[Month Number] = PrevMonth
    )
RETURN
    IF(
        NOT(ISBLANK(PreviousSales)) && PreviousSales <> 0,
        DIVIDE(CurrentSales - PreviousSales, PreviousSales, 0),
        BLANK()
    )
```

## Ranking
```dax
Rank Sales Country = 
RANKX(
    ALLSELECTED(financials[Country]),
    CALCULATE([Total Sales], REMOVEFILTERS(financials[Country])),
    ,
    DESC,
    Dense
)
```
## Tooltip Context

```dax
Top Country Month = 
MAXX(
    TOPN(1, VALUES(financials[Country]), [Total Sales], DESC),
    financials[Country]
)

Top Product Month = 
MAXX(
    TOPN(1, VALUES(financials[Product]), [Total Sales], DESC),
    financials[Product]
)

Top Segment Month = 
MAXX(
    TOPN(1, VALUES(financials[Segment]), [Total Sales], DESC),
    financials[Segment]
)
```
