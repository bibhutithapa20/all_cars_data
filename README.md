# Consumer Complaints
 ## Contributor
 <p> Quang Nguyen, Bibhuti Thapa, Emma Kidane </p>

 ## Introduction
 <p>I will analyze the cars' speed in different weather and times of the day to see their relationship. </p>

## Data Dictionary
### Main Columns
<p>Date: The date we collect data</p>
<p>MPH: The speed of the car collected from the radar detector</p>
<p>Time: The time of the day</p>
<p>Temp: The temperature of each date</p>
<p>Weather: The weather of each date</p>

 ## Install Packages and Library
```
library(shiny)
library(ggplot2)
library(readxl)
library(dplyr)
library(lubridate)
```
 ## Data Cleaning
 <p>Counting Cars Analysis: Clearing the blank columns and changing the time format for better analysis.</p>

```
df_cars <- counting_cars[, c("Date", "MPH", "Time", "Temp", "Weather")]
df_cars$Time <- as.POSIXct(df_cars$Time, format = "%H:%M:%S")
df_cars$Hour <- hour(df_cars$Time)
```
## Shiny App
<p>I create a shiny app by defining UI and defining Server to publish my visualization on shinyapps.io</p>

```
ui <- fluidPage(
  titlePanel("Counting Cars Analysis"), #Set the Title 
  
  sidebarLayout(
    sidebarPanel( #Users can select the plot they want to see
      # Input for selecting the plot
      selectInput("plot_type", "Choose a plot:",
                  choices = c("Density Plot of MPH by Weather", "Boxplot of MPH by Hour"))
    ),
    
    mainPanel( #Main panel where the plot will be display
      plotOutput("plot")
    )
  )
)
server <- function(input, output) {
  output$plot <- renderPlot({
  })
}
```

## Data Analysis
### 1. Density of Car's Speed by Weather Condition

<p>With numerous cars' speeds, we need to identify if the weather has any impact on driving conditions .</p>

<p> To do this, I create a density with the x-axis as MPH, the y-axis as density, and the fill with the weather. </p>

```
# Check if the user selected "Density Plot of MPH by Weather"
if (input$plot_type == "Density Plot of MPH by Weather") {
      # Create a density plot of MPH by Weather
      ggplot(df_cars, aes(x = MPH, fill = Weather)) +
        geom_density(alpha = 0.5) + # Add density plot with transparency
        labs(title = "Density Plot of MPH by Weather Condition",
             x = "MPH", y = "Density", fill = "Weather") +
        theme_minimal()
    }
```
<img width="556" alt="Density plot of MPH by Weather Condition" src="https://github.com/QDZ03/Data332/assets/159860533/1939e3c5-5f1d-41d8-a09b-17a412770d43">

- The density is highest at about 32 MPH in sunny weather and 28 MPH in cloudy weather.
- This means drivers tend to drive faster in good weather and slower in bad weather. 

### 2. Relationship between Car's Speed and Time 

<p>Knowing the relationship between a Car's Speed and Weather, we need to understand if Time affects the car's speed </p>

<p>To show the relationship of all three categories simultaneously, I create a box plot with the x-axis as Time, y-axis as MPH, and fill with Weather. </p>

```
# Check if the user selected "Boxplot of MPH by Hour"
    else if (input$plot_type == "Boxplot of MPH by Hour") {
      # Create a boxplot of MPH by Hour with weather grouping
      ggplot(df_cars, aes(x = as.factor(Hour), y = MPH)) +
        geom_boxplot(aes(fill = Weather)) + # Add boxplot with weather grouping   
        stat_summary(fun = mean, geom = "point", shape = 23, size = 4, color = "black") + # Add mean points
        stat_summary(fun = mean, geom = "text", aes(label = round(after_stat(y), digits = 2)), vjust = -1) + # Add mean text label
        stat_summary(fun = median, geom = "text", aes(label = round(after_stat(y), digits = 2)), vjust = 1.5, color = "blue") + # Add median text label
        labs(title = "Boxplot of MPH by Hour",
             x = "Hour", y = "MPH", fill = "Weather") +
        theme_minimal() +
        scale_x_discrete(labels = function(x) paste0(x, ":00"))
    }
```
<img width="556" alt="Boxplot of MPH by Hour" src="https://github.com/QDZ03/Data332/assets/159860533/e67768da-2b0c-4095-ac78-52a7d6b62c47">

- We can see that there is not much difference in the speed on a sunny day, as their median is around 32 MPH.
- This is reasonable because both of the data collected in casual time when is not rush or deserted hours 
- With the 30 MPH speed limit, this is a success for radar detectors.

# Conclusion
1. Since the data is limited, we cannot fully conclude the effect of the radar detector.
2. However, the radar detector still helps drivers be more aware of their driving speed in different kinds of weather.# all_cars_data
