# Patient-Health-Monitoring 
[Data Visualization Use Case and Pathways .pdf](https://github.com/Diza007/Patient-Health-Monitoring/files/14207676/Data.Visualization.Use.Case.and.Pathways.pdf)

[Patient DATA (1).xlsx](https://github.com/Diza007/Patient-Health-Monitoring/files/14207681/Patient.DATA.1.xlsx)

[Rcodes.txt](https://github.com/Diza007/Patient-Health-Monitoring/files/14209286/Rcodes.txt) 

https://hell24.shinyapps.io/project_directory/  

library(shiny)
library(rsconnect)
rsconnect::setAccountInfo(name='hell24',
                          token='AC01515C93054BD01F4FE48E329D889C',
                          secret='Diai+I0rT68PfB1JknN+h4JsfF/9UjpHXEy3Vdwk')

rsconnect::deployApp ("C://Users//hadiz//OneDrive//Documents//Project Directory")
library(shiny)
library(ggplot2)
library(readxl)
setwd("C:/Users/hadiz/OneDrive/Documents/Project Directory/")

data <- read_excel("Patient DATA (1).xlsx")

ui <- shinyUI(fluidPage(
  titlePanel("Patient Data Visualization"),
  
  sidebarLayout(
    sidebarPanel(
      # Add a slider input for age selection
      sliderInput("ageRange", "Select Age Range:",
                  min = min(data$age), max = max(data$age),
                  value = c(min(data$age), max(data$age)))
    ),
    
    mainPanel(
      # Display the plot
      plotOutput("patientPlot")
    )
  )
))

server <- function(input, output) {
  
  output$patientPlot <- renderPlot({
    
    # Filter data based on selected age range
    filtered_data <- data[data$age >= input$ageRange[1] & data$age <= input$ageRange[2], ]
    
    # Customize the ggplot based on your data columns
    ggplot(filtered_data, aes(x = age, y = blood_pressure, color = gender)) +
      geom_point() +
      labs(title = "Relationship between Age, Gender, and Blood Pressure",
           x = "Age",
           y = "Blood Pressure")
  })
}

shinyApp(ui = ui, server = server)

