2013 BISON Nest Data Chipp North.xlsx
Data munging steps
now running R Studio 0.97.551 with R version 3.0.2 (2013-09-25)
24-Dec-13 Annie Simpson
New files created:
ChippNorthIPT.csv, ChippN-BISON.csv, ReadMeAKchipN.txt (this document), and 2013 BISON Nest Data Chipp North-Original.xlsx (the renamed original document)
Final file name for BISON:
USGS-AK-CAE-ChippNorth-Birds-2013.csv
326 records
14 columns (no FIPS, no common name)


Delete row #1
Save as "2013 BISON Nest Data Chipp North.csv" R will not handle a df starting with a number, so changed file name to ChippN.
DhippN" = 326 obs. of 17 variables
colnames =
 [1] "Nest.ID"                "Scientific.Name"        "Common.Name"           
 [4] "Latitude"               "Longitude"              "Country.Code"          
 [7] "ITIS.Taxon.ID"          "Year"                   "Basis.of.Record"       
[10] "Provider.s.institution" "Collection.code"        "Day"                   
[13] "Month"                  "Year.1"                 "Collected.by"          
[16] "State"                  "County.or.Borough" 

Field Map
DF = BISON
1 Nest.ID = id
2 Scientific.Name = scientific_name
x3 Common.Name = common_name
4 Latitude = latitude
5 Longitude = longitude
x6 Country.Code [3 characters; replace]
7 ITIS.Taxon.ID = taxon_id
8 Year = year
9 Basis.of.Record = basis_of_record
10 Provider.s.institution = provider
11 Collection.code = resource
x12 Day [part of occurrence_date]
x13 Month [part of occurrence_date]
x14 Year.1 [eliminate and use Year]
15 Collected.by = collector
16 State = state_name
17 County.or.Borough = county_name
18 [created] occurrence_date
19 [created] iso_country_code

Order of fields for submission to Biva:
id
scientific_name
latitude
longitude
iso_country_code
taxon_id
year
basis_of_record
provider
resource
occurrence_date
collector
fips
state_name
county

=========================

#Instead of renaming columns, I copied as new with BISON field names, then for the BISON version I deleted old columns and 
#reordered BISON field columns. Did not include FIPS.
#Perhaps because of my recent update of R and R Studio, there were glitches I couldn't understand, such as errors of 
#unexpected "," in "write.csv<-(... [when I wrote the command exactly as in my notes]
#I solved the errors by closing and re-opening R Studio, on at least three different occasions.
#Note I inadvertently left out "Collected.By" and reinserted it with new fixed content (there was only one collector).

#R code (edited) copy/pasted here:

getwd()


ChippN <- read.csv("~/ChippN.csv")

ChippN$Year.1<-NULL

ChippN$month<-strtrim(ChippN$Month, 3)

ChippN$occurrence_date <- as.character(paste(ChippN$Year,ChippN$month,ChippN$Day,sep="-"))

ChippN$iso_country_code<-strtrim(ChippN$Country.Code, 2)

write.csv(ChippN,"ChippNorthIPT.csv",row.names=FALSE)

ChippNB<-ChippN[c(1,2,4,5,19,7,8,9,10,11,18,15,16,17)]

ChippNB$id<-ChippNB$Nest.ID

ChippNB$scientific_name<-ChippNB$Scientific.Name

ChippNB$latitude<-ChippNB$Latitude

ChippNB$longitude<-ChippNB$Longitude

ChippNB$taxon_id<-ChippNB$ITIS.Taxon.ID

ChippNB$year<-ChippNB$Year

ChippNB$basis_of_record<-ChippNB$Basis.Of.Record

ChippNB$provider<-ChippNB$Provider.s.institution

ChippNB$resource<-ChippNB$Collection.code

ChippNB$state_name<-ChippNB$State

write.csv(ChippNB,"ChippN-BISON.csv",row.names=FALSE)

ChippNB$county<-ChippNB$County.or.Borough

ChippNB$basis_of_record<-ChippNB$Basis.of.Record

ChippNB$collector<-"Tom Fondell"

View(ChippNB)

colnames(ChippNB)

ChippNorthBISON<-ChippNB[c(15,16,17,18,5,19,7,25,21,22,11,26,23,24)]

write.csv(ChippNorthBISON,"ChippNorth-BISON.csv",row.names=FALSE)

savehistory("~/AKchipNorthBirds/ChippNorthRhistory.txt")


There was a hell of a lot of trouble after all this...
The leading zeros of the days and month were dropped and I had to redo the date a million times.
For some strange reason, the year for the last 15 records or so started increasing by one for each record, so I had to redo the date field.

Here is some more of the R scripts:

FOR BISON:
 ChippN<-read.csv("ChippNorthIPT.csv",sep=",",header=TRUE)
> ChippN$basisOfRecord<-paste("Human Observation")
> ChippN$Basis.of.Record<-NULL
> ChippN2<-replace(as.character(ChippN(ChippN$Month, "June,July","06,07")))
Error in replace(as.character(ChippN(ChippN$Month, "June,July", "06,07"))) : 
  argument "values" is missing, with no default
> ChippN<-replace(ChippN$Month,June,06)
Error in `[<-.factor`(`*tmp*`, list, value = 6) : object 'June' not found
In addition: Warning message:
In `[<-.factor`(`*tmp*`, list, value = 6) :
  invalid factor level, NA generated
> ChippN$Month<-as.character(ChippN$Month)
> ChippN$Month<-[ChippN$Month == "June"]<-"06"
Error: unexpected '[' in "ChippN$Month<-["
> ChippN$Month[ChippN$Month == "June"]<-"06"
> View(ChippN)
> ChippN$Month[ChippN$Month == "July"]<-"07"
> View(ChippN)
> ChippN$Day<-as.character(ChippN$Day)
> ChippN$Day<-[ChippN$Day == "1"]<-"01"
Error: unexpected '[' in "ChippN$Day<-["
> ChippN$Day[ChippN$Day == "1"]<-"01"
> ChippN$Day[ChippN$Day == "2"]<-"02"
> ChippN$Day[ChippN$Day == "3"]<-"03"
> ChippN$Day[ChippN$Day == "4"]<-"04"
> ChippN$Day[ChippN$Day == "5"]<-"05"
> ChippN$Day[ChippN$Day == "6"]<-"0"
> ChippN$Day[ChippN$Day == "0"]<-"06"
> ChippN$Day[ChippN$Day == "7"]<-"07"
> ChippN$Day[ChippN$Day == "8"]<-"08"
> ChippN$Day[ChippN$Day == "9"]<-"09"
> ChippN$eventDate<-paste(ChippN$Year,ChippN$Month,ChippN$Day,sep="-")
> ChippN$eventDate<-as.date(ChippN$eventDate)
Error: could not find function "as.date"
> ChippN$eventDate<-as.factor(ChippN$eventDate)
> View(ChippN)
> ChippN$Country.Code=NULL
> View(ChippN)
> ChippN$occurrence_date=NULL
> View(ChippN)
> write.csv(ChippN,"ChippNorthIPT.csv",row.names=FALSE)
> View(ChippNB)
> rm(ChippNB)
> ChippNB<-ChippN
> View(ChippNB)
> ChippNB$id<-ChippNB$Nest.ID
> View(ChippNB)
> ChippNB$scientific_name<-ChippNB$Scientific.Name
> ChippNB$latitude<-ChippNB$Latitude
> ChippNB$longitude<-ChippNB$Longitude
> ChippNB$ITIS.Taxon.ID<-ChippNB$taxon_id
> ChippNB$year<-ChippNB$Year
> ChippNB$taxon_id<-ChippNB$ITIS.Taxon.ID
> ChippNB$ITIS.Taxon.ID=NULL
> ChippNB$basis_of_record<-ChippNB$basisOfRecord
> ChippNB$basis_of_record[ChippN$basis_of_record == "Human Observation"]<-"Observation"
> View(ChippNB)
> View(ChippNB)
> list(unique(ChippNB$basis_of_record))
[[1]]
[1] "Human Observation"

> ChippNB$basis_of_record<-as.character(ChippN$basis_of_record)
Error in `$<-.data.frame`(`*tmp*`, "basis_of_record", value = character(0)) : 
  replacement has 0 rows, data has 326
> ChippNB$basis_of_record<-as.character(ChippNB$basis_of_record)
> ChippNB$basis_of_record[ChippN$basis_of_record == "Human Observation"]<-"Observation"
> View(ChippNB)
> ChippNB$basis_of_record[ChippN$basis_of_record == "Human Observation"]<-"Observation"
> View(ChippNB)
> write.csv(ChippNB, "ChippNorth-BISON1.csv",row.names=FALSE)
> rm1stchar(ChippNB$basis_of_record, n = 6)
Error: could not find function "rm1stchar"
> rm(ChippNB$basis_of_record, n = 6)
Error in rm(ChippNB$basis_of_record, n = 6) : 
  ... must contain names or character strings
> rm(ChippNB$basis_of_record, "Human ")
Error in rm(ChippNB$basis_of_record, "Human ") : 
  ... must contain names or character strings
> rm(ChippNB$basis_of_record, "Human ")<-ChippNB$basis_of_record
Error in rm(ChippNB$basis_of_record, "Human ") <- ChippNB$basis_of_record : 
  could not find function "rm<-"
> ChippNB$basis_of_record<-rm(ChippNB$basis_of_record, "Human ")
Error in rm(ChippNB$basis_of_record, "Human ") : 
  ... must contain names or character strings
> rm(ChippNB$basis_of_record, list = "Human ")
Error in rm(ChippNB$basis_of_record, list = "Human ") : 
  ... must contain names or character strings
> rm("Human ", ChippNB$basis_of_record)
Error in rm("Human ", ChippNB$basis_of_record) : 
  ... must contain names or character strings
> ChippNB$basis_of_record=NULL
> View(ChippNB)
> View(ChippNorthBISON)
> ChippNB$basis_of_record="observation"
> View(ChippNB)
> ChippNB$provider<-ChippNB$Provider.s.institution
> ChippNB$resource<-ChippNB$Collection.code
> ChippNB$occurrence_date<-ChippNB$eventDate
> ChippNB$collector<-ChippNB$Collected.by
> ChippNB$state_name<-ChippNB$State
> ChippNB$county<-ChippNB$County.or.Borough
> View(ChippNB)
> ChippNB$taxon_id<-ChippNorthBISON$taxon_id
> View(ChippNB)
> ChippNorthBISON<-ChippNorth[c("id","scientific_name","latitude","longitude","iso_country_code","taxon_id","year","basis_of_record","provider","resource","occurrence_date","collector","fips","state_name","county")]
Error: object 'ChippNorth' not found
> ChippNorthBISON<-ChippN[c("id","scientific_name","latitude","longitude","iso_country_code","taxon_id","year","basis_of_record","provider","resource","occurrence_date","collector","fips","state_name","county")]
Error in `[.data.frame`(ChippN, c("id", "scientific_name", "latitude",  : 
  undefined columns selected
> ChippNorthBISON<-ChippNB[c("id","scientific_name","latitude","longitude","iso_country_code","taxon_id","year","basis_of_record","provider","resource","occurrence_date","collector","fips","state_name","county")]
Error in `[.data.frame`(ChippNB, c("id", "scientific_name", "latitude",  : 
  undefined columns selected
> colnames(ChippNB)
 [1] "Nest.ID"                "Scientific.Name"        "Common.Name"           
 [4] "Latitude"               "Longitude"              "Year"                  
 [7] "Provider.s.institution" "Collection.code"        "Day"                   
[10] "Month"                  "Collected.by"           "State"                 
[13] "County.or.Borough"      "month"                  "iso_country_code"      
[16] "basisOfRecord"          "eventDate"              "id"                    
[19] "scientific_name"        "latitude"               "longitude"             
[22] "year"                   "basis_of_record"        "provider"              
[25] "resource"               "occurrence_date"        "collector"             
[28] "state_name"             "county"                 "taxon_id"              
> ChippNorthBISON<-ChippNB[,c(18,19,20,21,15,30,22,23,24,25,26,27,28,29)]
> View(ChippNorthBISON)
> write.csv(ChippNorthBISON,"ChippNorth-BISON.csv",row.names=FALSE)
> ChippNorthBISON$year<-as.character(ChippNorthBISON$year)
> View(ChippN)
> ChippNorthBISON$occurrence_date<-paste(ChippNorthBISON$year,ChippN$Month,ChippN$Day,sep="-")
> View(ChippNorthBISON)
> write.csv(ChippNorthBISON,"ChippNorth-BISON.csv",sep=",",header=TRUE)


FOR IPT:

> ChippN<-read.csv("ChippNorthIPT.csv",sep=",",header=TRUE)
> View(ChippN)
> ChippN$Month<-as.character(ChippN$Month)
> ChippN$Month<-[ChippN$Month == "6"]<-"06"
Error: unexpected '[' in "ChippN$Month<-["
> ChippN$Month[ChippN$Month == "6"]<-"06"
> ChippN$Month[ChippN$Month == "7"]<-"07"
> ChippN$eventDate<-paste(ChippN$Year,ChippN$Month,ChippN$Day,sep="-")
> View(ChippN)
> ChippN$Year<-paste(ChippNorthBISON$year)
> View(ChippN)
> ChippN$eventDate<-paste(ChippN$year,ChippN$Month,ChippN$Day,sep="-")
> View(ChippN)
> ChippN$eventDate<-paste(ChippN$Year,ChippN$Month,ChippN$Day,sep="-")
> View(ChippN)
> ChippN$Day<-as.character(ChippN$Day)
> ChippN$Day[ChippN$Day == "1"]<-"01"
> ChippN$Day[ChippN$Day == ""]<-"02"
> ChippN$Day[ChippN$Day == "2"]<-"02"
> ChippN$Day[ChippN$Day == "3"]<-"03"
> ChippN$Day[ChippN$Day == "4"]<-"04"
> ChippN$Day[ChippN$Day == "5"]<-"05"
> ChippN$Day[ChippN$Day == "6"]<-"06"
> ChippN$Day[ChippN$Day == "7"]<-"07"
> ChippN$Day[ChippN$Day == "8"]<-"08"
> ChippN$Day[ChippN$Day == "9"]<-"09"
> ChippN$eventDate<-paste(ChippN$Year,ChippN$Month,ChippN$Day,sep="-")
> View(ChippN)
> write.csv(ChippN,"ChippNorthIPT.csv",row.names=FALSE)

2014-03-28
renamed "resource" in R and created Jira ticket.

Rscripts:

ChippN<-read.csv("ChippNorth-BISON.csv",sep=",",header=TRUE)
> View(ChippN)
> ChippN$resource<-"USGS - Alaska Science Center - Changing Arctic Ecosystems - Chipp North - Birds - 2013"
> View(ChippN)
> write.csv(ChippN,"USGS-AK-CAE-ChippNorth-Birds-2013.csv",row.names=FALSE)
> colnames(ChippN)
 [1] "id"               "scientific_name"  "latitude"         "longitude"        "iso_country_code"
 [6] "taxon_id"         "year"             "basis_of_record"  "provider"         "resource"        
[11] "occurrence_date"  "collector"        "state_name"       "county"          
> 

Realized I lost common name at some point. Since BISON inserts them from ITIS, I didn't re-insert them.
