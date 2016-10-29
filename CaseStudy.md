# Case Study 1
Victor Yim  
October 29, 2016  



##Introduction


There are many ways to measure and compare economy between countries. The most typical statistics used are either GDP or Income.  
The gross domestic product (GDP) is a unit measure of total values of the final output of the goods and services produced in a year.  The strength of an economy is often measure in GDP and compare year over year to understand growth.  Since this is the total output by a country, there are other variables like population, natural resources, political and other external factors that may impact whether countries have high GDP. 
Average Income is another method to measure strength and prosperity of an economy.  Countries with higher income can typically afford more goods and services, thus in turn create a stronger economy.   As like GDP, there are other factors that could impact this measure.
For this analysis, we attempt to look at the two types for measure to evaluate their relationship.



R codes below explain each of the steps taken to generate the result


```r
# first setting the directory for the download
setwd("C:/Users/Victor Yim/Documents/SMU/6306/Case Study 1")
# Download data using repmis library
library(repmis)
FinURL1<-"https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv"
GDP <- repmis::source_data(FinURL1, sep = ",", skip=3, header = TRUE)
```

```
## Downloading data from: https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv
```

```
## SHA-1 hash of the downloaded data file is:
## 18dd2f9ca509a8ace7d8de3831a8f842124c533d
```

```
## Warning in fread(data, sep = sep, header = header, data.table = F,
## stringsAsFactors = stringsAsFactors, : Bumped column 6 to type character
## on data row 63, field contains 'a'. Coercing previously read values in this
## column from logical, integer or numeric back to character which may not
## be lossless; e.g., if '00' and '000' occurred before they will now be just
## '0', and there may be inconsistencies with treatment of ',,' and ',NA,' too
## (if they occurred in this column before the bump). If this matters please
## rerun and set 'colClasses' to 'character' for this column. Please note
## that column type detection uses the first 5 rows, the middle 5 rows and the
## last 5 rows, so hopefully this message should be very rare. If reporting to
## datatable-help, please rerun and include the output from verbose=TRUE.
```

```
## Warning in fread(data, sep = sep, header = header, data.table = F,
## stringsAsFactors = stringsAsFactors, : Bumped column 2 to type character on
## data row 234, field contains '.. Not available. '. Coercing previously read
## values in this column from logical, integer or numeric back to character
## which may not be lossless; e.g., if '00' and '000' occurred before they
## will now be just '0', and there may be inconsistencies with treatment of
## ',,' and ',NA,' too (if they occurred in this column before the bump).
## If this matters please rerun and set 'colClasses' to 'character' for this
## column. Please note that column type detection uses the first 5 rows, the
## middle 5 rows and the last 5 rows, so hopefully this message should be very
## rare. If reporting to datatable-help, please rerun and include the output
## from verbose=TRUE.
```

```r
#Tidy data process
#rename column headers to reflect field values
names(GDP) [1]<-("country")
names(GDP)[5]<-("GDP")
#subsetting data to remove columns with no value
GDP2012<-subset(GDP, country>0, select = c(country, Ranking,Economy,GDP))
#Continue tidy data to reflect actual data characteristics
GDP2012$Ranking<-as.numeric(GDP2012$Ranking)
GDP2012$GDP<-as.numeric(gsub(",","",GDP2012$GDP))
```

```
## Warning: NAs introduced by coercion
```

```r
#examine data
head(GDP2012)
```

```
##   country Ranking        Economy      GDP
## 2     USA       1  United States 16244600
## 3     CHN       2          China  8227103
## 4     JPN       3          Japan  5959718
## 5     DEU       4        Germany  3428131
## 6     FRA       5         France  2612878
## 7     GBR       6 United Kingdom  2471784
```

```r
#obtain second dataset for income
FinURL2<-"https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv"
EDU <- repmis::source_data(FinURL2, sep = ",",  header = TRUE)
```

```
## Downloading data from: https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv
```

```
## SHA-1 hash of the downloaded data file is:
## 20be6ae8245b5a565a815c18a615a83c34745e5e
```

```r
#rename column variable to match first dataset
names(EDU) [1]<-("country")
#examine data
head(EDU)
```

```
##   country                    Long Name         Income Group
## 1     ABW                        Aruba High income: nonOECD
## 2     ADO      Principality of Andorra High income: nonOECD
## 3     AFG Islamic State of Afghanistan           Low income
## 4     AGO  People's Republic of Angola  Lower middle income
## 5     ALB          Republic of Albania  Upper middle income
## 6     ARE         United Arab Emirates High income: nonOECD
##                       Region Lending category Other groups  Currency Unit
## 1  Latin America & Caribbean                                Aruban florin
## 2      Europe & Central Asia                                         Euro
## 3                 South Asia              IDA         HIPC Afghan afghani
## 4         Sub-Saharan Africa              IDA              Angolan kwanza
## 5      Europe & Central Asia             IBRD                Albanian lek
## 6 Middle East & North Africa                                U.A.E. dirham
##   Latest population census  Latest household survey
## 1                     2000                         
## 2           Register based                         
## 3                     1979               MICS, 2003
## 4                     1970 MICS, 2001, MIS, 2006/07
## 5                     2001               MICS, 2005
## 6                     2005                         
##                                                                 Special Notes
## 1                                                                            
## 2                                                                            
## 3 Fiscal year end: March 20; reporting period for national accounts data: FY.
## 4                                                                            
## 5                                                                            
## 6                                                                            
##   National accounts base year National accounts reference year
## 1                        1995                               NA
## 2                                                           NA
## 3                   2002/2003                               NA
## 4                        1997                               NA
## 5                                                         1996
## 6                        1995                               NA
##   System of National Accounts SNA price valuation
## 1                          NA                    
## 2                          NA                    
## 3                          NA                 VAB
## 4                          NA                 VAP
## 5                        1993                 VAB
## 6                          NA                 VAB
##   Alternative conversion factor PPP survey year
## 1                                            NA
## 2                                            NA
## 3                                            NA
## 4                       1991-96            2005
## 5                                          2005
## 6                                            NA
##   Balance of Payments Manual in use External debt Reporting status
## 1                                                                 
## 2                                                                 
## 3                                                           Actual
## 4                              BPM5                         Actual
## 5                              BPM5                         Actual
## 6                              BPM4                               
##   System of trade Government Accounting concept
## 1         Special                              
## 2         General                              
## 3         General                  Consolidated
## 4         Special                              
## 5         General                  Consolidated
## 6         General                  Consolidated
##   IMF data dissemination standard
## 1                                
## 2                                
## 3                            GDDS
## 4                            GDDS
## 5                            GDDS
## 6                            GDDS
##   Source of most recent Income and expenditure data
## 1                                                  
## 2                                                  
## 3                                                  
## 4                                         IHS, 2000
## 5                                        LSMS, 2005
## 6                                                  
##   Vital registration complete Latest agricultural census
## 1                                                       
## 2                         Yes                           
## 3                                                       
## 4                                                1964-65
## 5                         Yes                       1998
## 6                                                   1998
##   Latest industrial data Latest trade data Latest water withdrawal data
## 1                     NA              2008                           NA
## 2                     NA              2006                           NA
## 3                     NA              2008                         2000
## 4                     NA              1991                         2000
## 5                   2005              2008                         2000
## 6                     NA              2008                         2005
##   2-alpha code WB-2 code           Table Name           Short Name
## 1           AW        AW                Aruba                Aruba
## 2           AD        AD              Andorra              Andorra
## 3           AF        AF          Afghanistan          Afghanistan
## 4           AO        AO               Angola               Angola
## 5           AL        AL              Albania              Albania
## 6           AE        AE United Arab Emirates United Arab Emirates
```

#Questions on Merged Data
1) Merge the data based on the country shortcode. How many of the IDs match?


```r
#merging GDP an EDU dataset from worldbank
total.data <- merge(GDP2012,EDU,by="country")
dim(total.data)
```

```
## [1] 224  34
```

There are 224 matches between the 2 datasets

2) Sort the data frame in ascending order by GDP (so United States is last). What is the 13th country in the resulting data frame?




```r
total.data2<-subset(total.data,  select = c(country, Ranking,Economy,GDP))
The13th<-total.data2[order(total.data$GDP),]
head(The13th, n=13)
```

```
##     country Ranking                        Economy GDP
## 204     TUV     190                         Tuvalu  40
## 105     KIR     189                       Kiribati 175
## 132     MHL     188               Marshall Islands 182
## 161     PLW     187                          Palau 228
## 185     STP     186          São Tomé and Principe 263
## 68      FSM     185          Micronesia, Fed. Sts. 326
## 200     TON     184                          Tonga 472
## 51      DMA     183                       Dominica 480
## 42      COM     182                        Comoros 596
## 219     WSM     181                          Samoa 684
## 212     VCT     180 St. Vincent and the Grenadines 713
## 78      GRD     178                        Grenada 767
## 106     KNA     178            St. Kitts and Nevis 767
```
there are 2 countries tied for 13:  Grenada and St. Kitts and Nevis


3) What are the average GDP rankings for the "High income: OECD" and "High income:##nonOECD" groups?


```r
#tidy data process to remove commas
colnames(total.data) <- gsub(" ","",colnames(total.data))
HIC<-subset(total.data,  select = c(country, Ranking,Economy,GDP,IncomeGroup))
HIC$IncomeGroup<-gsub(" ","",HIC$IncomeGroup)
HIC$IncomeGroup<-gsub(":","",HIC$IncomeGroup)
#subsetting only data with high income
HICOECD<-subset(HIC, IncomeGroup=="HighincomenonOECD" | IncomeGroup=="HighincomeOECD" , NA.RM=TRUE,  select = c(GDP, country, IncomeGroup))
aggregate( GDP~IncomeGroup, HICOECD, mean )
```

```
##         IncomeGroup       GDP
## 1 HighincomenonOECD  104349.8
## 2    HighincomeOECD 1483917.1
```
The Average GDP for HighincomenonOECD = 104349.8 and the Average GDP for HighincomeOECD  = 1483917.1

4) Plot the GDP for all of the countries. Use ggplot2 to color your plot by Income Group.


```r
library(ggplot2)
HICGRAPH<-subset(HIC, NA.RM=TRUE, GDP>1,  select = c(GDP, country, IncomeGroup))
HICGRAPH2<-HICGRAPH[complete.cases(HICGRAPH),]
#qplot(x, y, data=, color=, shape=, size=, alpha=, geom=, method=, formula=, facets=, xlim=, ylim= xlab=, ylab=, main=, sub=)
qplot(country, GDP, data=HICGRAPH2, color=factor(IncomeGroup),  main='GDP by Country')
```

![](blob/master/unnamed-chunk-4-1.png)<!-- -->

5) Cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group.
How many countries are Lower middle income but among the 38 nations with highest
GDP?


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
HIC$quantile <- ntile(HIC$GDP, 5) 
attach(HIC)
```

```
## The following object is masked _by_ .GlobalEnv:
## 
##     GDP
```

```r
Pivottabletable <- table(IncomeGroup, quantile)
detach(HIC)

Pivottabletable
```

```
##                    quantile
## IncomeGroup          1  2  3  4  5
##                      0  0  0  2 12
##   HighincomenonOECD  4  7  7  5  0
##   HighincomeOECD     0  1  5 17  7
##   Lowermiddleincome 17 10 17  8  2
##   Lowincome         13 19  5  0  0
##   Uppermiddleincome 11  8 11 13  2
```

There are 17 countries that are Lower middle income but among the 38 nations with highest GDP (in first quantity above)


## Summary
After analyzing the GDP data from Worldbank, there appears to be a significant range between the countries' gross domestic product output. the top 20% of the countries has an average GDP > 1000 times greater than the botton 20% of the countries.
By using the Organization for Economic Co-operation and Development (OECD) comparison,  we  found that countries with OECD membership has a higher average GDP than those that are not members.
Using the 5 income tier provided by the worldbank:   
- High income: nonOECD  
- High income: OECD  
- Lower middle income  
- Upper middle income  
- Low income  
we found that the average income level does not also result in higher GDP or vise versa.   These measures are useful to answer specific questions in related to the topics.  

