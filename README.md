---
title: "Expl_Data_Analysis_Assignment_week1"
author: "Umesh"
date: "11/11/2018"
output: html_document
---
##Importing Data and tidying
```{r}
library(tidyverse)
library(lubridate)
#Import Data
df <- read.table("household_power_consumption.txt", 
                 sep =";", header = TRUE, dec =".")
##Convert to Date and Time format and Combine date and time
df$Date <- dmy(df$Date)
df$dttm <- as.POSIXct(paste(df$Date, df$Time), format="%Y-%m-%d %H:%M:%S")

df1<- df%>%
        filter(Date >= "2007-02-01" & Date <=  "2007-02-02")

###Changing the class of variables
df1$Global_active_power <- as.numeric(as.character(df1$Global_active_power))
df1$Global_reactive_power <- as.numeric(as.character(df1$Global_reactive_power))
df1$Voltage <- as.numeric(as.character(df1$Voltage))
df1$Sub_metering_1 <- as.numeric(as.character(df1$Sub_metering_1))
df1$Sub_metering_2 <- as.numeric(as.character(df1$Sub_metering_2))
df1$Sub_metering_3 <- as.numeric(as.character(df1$Sub_metering_3))
```

##Plot 1 

```{r}

plot1 <- hist(df1$Global_active_power, main = "Global Active Power",
              xlab = "Global Active Power (KilloWats)", 
              col = "red")
print(plot1)
dev.copy(png, filename = 'plot1.png')
dev.off()

```

##Plot 2

```{r}
plot2 <- plot(df1$dttm, df1$Global_active_power,
              type="l",
              xlab="",
              ylab="Global Active Power (kilowatts)")
print(plot2)
dev.copy(png, filename = 'plot2.png')
dev.off()

```

##Plot 3 

``` {r}
plot3 <- plot(df1$dttm, df1$Sub_metering_1, type="l", col="black",
              xlab="", ylab="Energy sub metering")
lines(df1$dttm, df1$Sub_metering_2, col="red")
lines(df1$dttm, df1$Sub_metering_3, col="blue")
legend("topright",
       col=c("black", "red", "blue"),
       c("Sub_metering_1", "Sub_metering_2", "Sub_metering_3"),
       lty=1)
print(plot3)
dev.copy(png, filename = 'plot3.png')
dev.off()

```
### Plot 4

``` {r}

par(mfrow = c(2,2))
plot(df1$dttm, df1$Global_active_power,
     type="l",
     xlab="",
     ylab="Global Active Power")
# 2
plot(df1$dttm, df1$Voltage, type="l",
     xlab="datetime", ylab="Voltage")
# 3
plot(df1$dttm,df1$Sub_metering_1, type="l", 
     xlab="", ylab="Energy sub metering")
lines(df1$dttm,df1$Sub_metering_2,col="red")
lines(df1$dttm,df1$Sub_metering_3,col="blue")
legend("topright", col=c("black","red","blue"), c("Sub_metering_1  ","Sub_metering_2  ", "Sub_metering_3  "),lty=c(1,1), bty="n", cex=.5)
# 4
plot(df1$dttm,df1$Global_reactive_power, type="l", xlab="datetime", ylab="Global_reactive_power")

dev.copy(png, filename = 'plot4.png')
dev.off()

```
