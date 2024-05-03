# Counting Cars Analysis
## Contributors
- Quang Nguyen
- Bibhuti Thapa
- Emma Kidane

## Introduction
In this analysis, we delve into the relationship between car speed, weather conditions, and time of day. 

## Data Dictionary
### Main Columns
- **Date:** Date of data collection
- **MPH:** Speed of the car detected
- **Time:** Time of the day
- **Temp:** Temperature recorded
- **Weather:** Weather condition during data collection

## Install Packages and Library
```
library(shiny)
library(ggplot2)
library(readxl)
library(dplyr)
library(lubridate)


```
## Data Cleaning
Counting Cars Analysis: Clearing the blank columns and changing the time format for better analysis.

```
df <- bind_rows(group_1, group_2, group_3, group_4, group_5, group_6, group_7, group_8)

# Time Format
df$Time <- as.POSIXct(df$Time, format = "%H:%M:%S")
df$Hour <- hour(df$Time)

# Assign Values
df$Speed <- as.numeric(df$Speed)
df$Weather <- as.character(df$Weather)
df$Weather <- replace(df$Weather, df$Weather %in% c('Clear skies, sundown', 'Sunny, clear skies'), 'Sunny') 
df$Weather <- capital_letter(df$Weather) 

```
## Shiny App
We create a Shiny app to visualize our analysis, allowing users to explore the relationship between car speed, weather, and time of day.
```
ui <- fluidPage(
  titlePanel("Counting Cars Analysis"),
  sidebarLayout(
    sidebarPanel(
      selectInput("plot_type", "Choose a plot:",
                  choices = c("Density Plot of MPH by Weather", "Boxplot of MPH by Hour"))
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

server <- function(input, output) {
  output$plot <- renderPlot({
    # Check if the user selected "Density Plot of MPH by Weather"
    if (input$plot_type == "Density Plot of MPH by Weather") {
      # Create a density plot of MPH by Weather
      ggplot(df, aes(x = Speed, fill = Weather)) +
        geom_density(alpha = 0.5) +
        labs(title = "Density Plot of MPH by Weather Condition",
             x = "MPH", y = "Density", fill = "Weather") +
        theme_minimal()
    }
    # Check if the user selected "Boxplot of MPH by Hour"
    else if (input$plot_type == "Boxplot of MPH by Hour") {
      # Create a boxplot of MPH by Hour with weather grouping
      ggplot(df, aes(x = as.factor(Hour), y = Speed)) +
        geom_boxplot(aes(fill = Weather)) +  
        stat_summary(fun = mean, geom = "point", shape = 23, size = 4, color = "black") +
        stat_summary(fun = mean, geom = "text", aes(label = round(after_stat(y), digits = 2)), vjust = -1) +
        stat_summary(fun = median, geom = "text", aes(label = round(after_stat(y), digits = 2)), vjust = 1.5, color = "blue") +
        labs(title = "Boxplot of MPH by Hour",
             x = "Hour", y = "MPH", fill = "Weather") +
        theme_minimal() +
        scale_x_discrete(labels = function(x) paste0(x, ":00"))
    }
  })
}

shinyApp(ui = ui, server = server)

```
## Data Analysis
1. Density of Car's Speed by Weather Condition

We investigate the distribution of car speeds across different weather conditions.

- The density of car speeds peaks around 32 MPH in sunny weather and 28 MPH in cloudy weather.
-  Drivers tend to drive faster in good weather and slower in bad weather.

2. Relationship between Car's Speed and Time
 
We explore the relationship between car speed and time of day.

- On sunny days, there's little variation in speed, with a median around 32 MPH.
- This consistency suggests that drivers maintain a steady speed during typical hours.
- The radar detector's efficiency is evident as speeds mostly align with the 30 MPH speed limit.

## Conclusion

- Due to data limitations, we can't conclusively determine the radar detector's impact on driving behavior.
- Nevertheless, it appears that the radar detector aids drivers in maintaining appropriate speeds across different weather conditions and times of day.










