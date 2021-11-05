# About

----

This project will analyze the housing market in select cities in order to determine if there is seasonality to the housing market using market data to compare the average housing costs during specific timeframes since 2017. Additionally, we will compare housing prices to determine which housing market is most afforadble.  Finally, we will compare list price with sales price in each market to see when a homebuyer might pay a premium, get a discount or pay market value.  Our aim is to determine when the best time to buy a house is in select cities.

The Cities we have selected for this are:

* Chicago, IL
* Denver, CO
* Des Moines, IA
* Kansas City, MO
* Madison, WI
* Minneapolis, MN
* Oklahoma City, OK
* Toledo, OH



## Contributors

---

Kate Davis

Margee Lancaster

Servontius Turner



## About Our Data

---

![zillowLogo](./images/zillowLogo.png)

For our project, we sourced our data from Zillow.com one of the nations largest third party Real Estate sites. We chose Zillow  for this reason and that it is one of the only Real Estate firms that offer and maintain a free API on listings. Real Estate does not (yet) have a central API to allow prospective analysts access to Real Estate information. Alot of provided APIs require the company to be a registered MLS company which requires a fee as well as a fee to access the API. Zillow offers a free API through Quandl as well as a more indepth and free API through a company called Bridge.

---

## Seasonality 
Determine the seasonality for each metro area

The further north the metro we see there is more noticable trends in total inventory of housing. Evaluating each metro area:
>1. Chicago: Is the largest of the metros and has the largest inventroy overall.  Inventory ramps up in the second quarter hits it peak in the third quarter slows in the forth and continues to decline through the first quarter of the next year.
>2. Denver: The market in Denver is similar to Chicago with inventory increase in the second quarter, peaking in the third quarter and declining in the fourth quarter into the next year.
>3. Des Moines: From the end of 2017 through June of 2020 Des Mois=nes saw the same seasonality as the other cities; however, since invetory has decreased signifiantly and has stayed low the last 18 months.
>4. Kansas City: Inventory was consistenly more steady than the ther metro areas but has a seen a steady decline in inventory since February 2020.
>5. Madison: Madison has seasonality trend consistent with Chicago and Denver but overall signifiantly less inventory than those markets.  
>6. Minneapolis: Has the same trends with inventory ramping up over the 2nd quarter, peaking in the third quarter and slowing declining the forth quarter into the next year.  Inventory decreased more noticably the end of 2020 and slowly increase over 2021.
>7. Oklahoma City: Oklahoma City is the southern most metro and reflects the least seasonality of all the markets.  Again, invetory has dipped since February of 2020 and hasn't rebounded since.
>8. Toledo:  Toledo is the smallest market there is some seasonality but noticable as most of the other markets.  Toledo has also seen a decrease in inventroy since February of 2020 and rebounded similar to Oklahoma City.


## Affordability
First we looked at the increase in home prices from 2008 to 2021:
```python
# Join all Metro Areas into a single DataFrame with columns for each City
combined_sale = [chicago_sale, denver_sale, des_moines_sale, kansas_city_sale, madison_sale, 
                 minneapolis_sale, oklahoma_city_sale, toledo_sale]
combined_final = functools.reduce(lambda left,right: pd.merge(left,right, on='date'), combined_sale)
combined_final.head()
```

| Index |       date | Chicago | Denver | Des Moines | Kansas City | Madison | Minneapolis | Oklahoma City | Toledo |
|------:|-----------:|--------:|-------:|-----------:|------------:|--------:|------------:|--------------:|-------:|
|     0 | 2021-06-30 |  300000 | 550000 |     255000 |      280000 |  347000 |      350000 |        225000 | 155300 |
|     1 | 2021-05-31 |  295000 | 540000 |     245000 |      281000 |  325000 |      339400 |        219000 | 140750 |
|     2 | 2021-04-30 |  280000 | 525500 |     249000 |      268000 |  315500 |      337000 |        213500 | 146000 |
|     3 | 2021-03-31 |  269000 | 505650 |     234300 |      257000 |  330000 |      322500 |        212000 | 148500 |
|     4 | 2021-02-28 |  255000 | 475000 |     229773 |      250000 |  289000 |      308900 |        210000 | 132450 |

```python
# Create line Chart to reflect average increase in sale price for each City
sales_price = combined_final.plot.line(x='date', title="Average Sales Price", figsize=(10,5))
```
![Increase In Home Prices](./images/Increase_In_Home_Prices.png)

From 2008 to 2015 home sale prices stayed realtively flat with small increases each year.  Around 2016 sale prices have seen a steady increase in pricing.  A short synopsis for each city is listed below
> 1. Toledo: Toledo has had the smallest increase in home prices over the last 14 years. It saw a significant decrease in value in March of 2014 followed by a significant incrase by August of 2014 and then steadily has increased since.
> 2. Kansas City: Kansas City has seen a consistent incrase in home prices over the last 14 years. The graph doesn't reflect any sharp increase or decreases in home prices but seasonality seems to be a factor.  
> 3. Oklahoma City: Oklahoma City has seen a consistent increase in home prices over the last 14 years. The graph doesn't reflect any sharp increase or decrease in home prices and seasonality seems to be less of a factor than Kansas City.
> 4. Chicago:  Chicago saw a decrease in home prices from February 2008 to February 2012 then saw a sharp incrase in July 2012 and then a sharp drop again in February 2013.  Prices were on the rise but much more volitile than any other city.
> 5. Minneapolis:  Minneapolis saw a slight decrease in prices from 2008 through 2012 but has seen a steady increase since. The increase seems higher than the other cities and has seen a sharp increase since January 2021.
> 6. Denver: Denver has seen a steady increase in home prices over the last 14 years with a sharper increase in the last 5 years.  Denver home prices have consistently been the highest of all markets.
> 7. Madison: Madison home prices stayed realtively flat from 2008 to 2014 and has steadily rose since.  Home prices have seen a steeper increase in prices over the last 18 months.
> 8. Des Moines: Des Moines home prices stayed relatively flat from 2008 to 2012 and since has steadily rose since.

Secondly we looked at the affordability of each city in comparison with the median income of those markets.

From the United States Census we were able to find the median family income for the 8 metro areas from 2017 through 2021

```python
# Reading Median Family Income csv
median_family_income_csv = "Data/Median_Family_Income_edit.csv"
family_income = pd.read_csv(median_family_income_csv, infer_datetime_format=True, parse_dates=True, index_col="date")
family_income.head(8)
```

Then we merged the meidan family income dataframe with the combined final dataframe 

```python
# combine dataframes
joined = pd.merge(annual_2, family_income, how="inner", on=["date", "metro_area"], sort=True, copy=True, indicator=False, validate=None)
joined.head(8)

```
|       date |    metro_area |  value | avg_income_household |   |
|-----------:|--------------:|-------:|---------------------:|---|
| 2017-12-31 |       Chicago | 225833 |                79000 |   |
| 2017-12-31 |        Denver | 382546 |                83900 |   |
| 2017-12-31 |    Des Moines | 195571 |                82200 |   |
| 2017-12-31 |   Kansas City | 198150 |                74800 |   |
| 2017-12-31 |       Madison | 244862 |                85200 |   |
| 2017-12-31 |   Minneapolis | 244582 |                90400 |   |
| 2017-12-31 | Oklahoma City | 166688 |                67300 |   |
| 2017-12-31 |        Toledo | 118300 |                61500 |   |

To find the cost of living we divided the home price by average income to get the price to income ratio

```python
# compute the price to income ratio
joined['Price_Income'] = (joined['value']/joined['avg_income_household'])
# joined['Price_Income'] = joined.Price_Income.round(decimals=2)
```
|       date |    metro_area |  value | avg_income_household | Price_Income |
|-----------:|--------------:|-------:|---------------------:|-------------:|
| 2017-12-31 |       Chicago | 225833 |                79000 |            3 |
| 2017-12-31 |        Denver | 382546 |                83900 |            5 |
| 2017-12-31 |    Des Moines | 195571 |                82200 |            2 |
| 2017-12-31 |   Kansas City | 198150 |                74800 |            3 |
| 2017-12-31 |       Madison | 244862 |                85200 |            3 |
| 2017-12-31 |   Minneapolis | 244582 |                90400 |            3 |
| 2017-12-31 | Oklahoma City | 166688 |                67300 |            2 |
| 2017-12-31 |        Toledo | 118300 |                61500 |            2 |

Year over year the most expensive market to live in was Denver and the least expesnive market was Toledo.  In 2021, Madison and Minneapolis saw the price to income ratio jump above 3 due to increases in home prices.