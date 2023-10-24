# Birds-Canada
Birds Canada Simulation Data - This analysis should be viewed as a simulated interpretation or experiment since it is solely intended for demonstrative purposes. The data used in the analysis is simulated. 

- I generated 2 datsets, each corresponding to the locations of Ontario and Manitoba.
- The simulated data revolves around the population of wild birds, incorporating key variables influencing bird population dynamics, namely temperature, habitat loss, and food availability.
- The dataset goes thru adjustments to align with needs. 
- The final dataset is in a standardized format to ensure that the dataset follows a standard format that is meaningful.
- Towards the end i did a regression analysis to see what variables are relevant or are the major factors that influence the population increase or decrease.

1. MetaData:

- Temperature: This variable represents the simulated temperature data for each year.
- HabitatLoss: This variable represents the simulated level of habitat loss for each year. 
- FoodAvailability: This variable represents the simulated level of food availability for each year. 
- Population: This variable represents the simulated bird population for each year. 
- NumberOfChicks: This variable represents the simulated number of chicks for each year.

~ Seth

# Data 1 - 3 variables - Create a data frame - Ontario 
- set.seed(123)

- years <- 1920:2023

- temperature <- rnorm(length(years), mean = 20, sd = 5)  
- habitat_loss <- runif(length(years), min = 0, max = 30)   
- food_availability <- rpois(length(years), lambda = 50)


# Calculate bird populations based on the three variables

- bird_population <- 1000 + 5 * temperature - 2 * habitat_loss + 3 * log(food_availability) + rnorm(length(years), mean = 0, sd = 100)

- number_of_chicks <- rpois(length(years), lambda = 10)  # Poisson distribution for the number of chicks _______#add number of chicks


- bird_data <- data.frame(
  Year = years,
  Temperature = temperature,
  HabitatLoss = habitat_loss,
  FoodAvailability = food_availability,
  Population = bird_population,
  NumberOfChicks = number_of_chicks
)


- head(bird_data)
- bird_data$Location <- "Ontario"
- colnames(bird_data)
- View(bird_data)


<img width="954" alt="Screen Shot 2023-10-23 at 5 04 50 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/9e03b8f5-3cd3-4d0a-b66e-c2362d28a085">






# Codes to Export the data to an excel file  - bird data 1

- install.packages("openxlsx")
- library(openxlsx)


- file_path <- "/Users/sethmenon/Downloads/bird.xlsx"


- write.xlsx(bird_data, file_path, sheetName = "BirdData", rowNames = FALSE)


- cat("Data exported to:", file_path, "\n")

# Data 2 - Manitoba - In the first dataset, the variables arranged in columns, but in data 2 it is arranged in rows. 

- install.packages("tidyr")
- library(tidyr)
- library(openxlsx)
- bird_data_long4 <- pivot_longer(bird_data, cols = c("HabitatLoss", "FoodAvailability", "Population", "NumberOfChicks"), names_to = "Variable", values_to = "Value")
- bird_data_long4$Location <- "Manitoba"
- View(bird_data_long4)

<img width="1054" alt="Screen Shot 2023-10-23 at 7 04 29 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/8c06c685-292f-4446-aa5e-239a793f9409">

# later the manitoba data was converted to colunm format for ease.

<img width="955" alt="Screen Shot 2023-10-23 at 5 06 17 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/1f4d4080-e9cc-4e5c-b43b-2862d1412c28">

# 2 Datasets are being combined here.

- install.packages("dplyr")
- library(dplyr)
- combined_data <- bind_rows(bird_data, bird_data_long)

- rm(combined_data)

- library(tidyr)
- library(openxlsx)

- colnames(bird_data_long)
- head(bird_data_long)

- combined_data <- combined_data %>% 
  mutate(variable_location = paste(Variable, Location, sep = "_"))

- library(tidyr)
- library(openxlsx)

- colnames(combined_data)

- head(combined_data)

<img width="952" alt="Screen Shot 2023-10-23 at 5 07 16 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/0c263551-e310-4d32-9810-61d27a23af91">


- library(tidyr)


- install.packages("writexl")
- library(writexl)
- file_path <- "/Users/sethmenon/Downloads/combined.xlsx"


- write_xlsx(combined_data, path = file_path)


- cat("Data exported to", file_path, "\n")

- head(combined_data_wide)

- library(dplyr)

# Chnage number of Chicks to a dummy variable 
- combined_data_wide <- combined_data_wide %>%
  mutate(Chicks_Survived_Ontario = ifelse(NumberOfChicks_Ontario > 0, "Yes", "No"),
         Chicks_Survived_Manitoba = ifelse(NumberOfChicks_Manitoba > 0, "Yes", "No"))

  <img width="1059" alt="Screen Shot 2023-10-23 at 7 12 58 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/2e2d2e91-042e-4211-8056-1a68962fd7db">


# THE FINAL DATA with Modifications - Changed some values of the variables as it is similar - this is so the plot will differ

- final_data <- combined_data_wide2 %>%
  select(Year, starts_with("Temperature"), starts_with("HabitatLoss"), starts_with("FoodAvailability"),
         starts_with("Population"), starts_with("Chicks_Survived"))


- head(final_data)

- library(writexl)


- file_path <- "/Users/sethmenon/Downloads/final .xlsx"


- write_xlsx(combined_data_wide2, path = file_path)


- cat("Data exported to", file_path, "\n")

- library(dplyr)

- final_data_modified <- final_data %>%
  mutate(
    Temperature_Manitoba = Temperature_Manitoba - 1,
    HabitatLoss_Manitoba = HabitatLoss_Manitoba - 1,
    FoodAvailability_Manitoba = FoodAvailability_Manitoba - 1,
    Population_Manitoba = Population_Manitoba - 1,
    Chicks_Survived_Manitoba = ifelse(Chicks_Survived_Ontario > 0, "Yes", "No")
  )


- head(final_data_modified)

<img width="1378" alt="Screen Shot 2023-10-23 at 5 20 56 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/dbdca2bd-cde1-43b6-8979-9f6c506615f0">

- library(writexl)

- file_path <- "/Users/sethmenon/Downloads/final .xlsx"

- write_xlsx(final_data_modified, path = file_path)
- cat("Data exported to", file_path, "\n")

# Visualization - There is not much differnce in the visual as the data is quite similar. 

- install.packages("ggplot2")
- library(ggplot2)
- library(gridExtra)

- plot_ontario <- ggplot(final_data_modified, aes(x = Year, y = Population_Ontario)) +
  geom_smooth(color = "blue", size = 1) +
  labs(title = "Smooth Population Trend - Ontario",
       x = "Year", y = "Population") +
  theme_minimal()

- plot_manitoba <- ggplot(final_data_modified, aes(x = Year, y = Population_Manitoba)) +
  geom_smooth(color = "red", size = 1) +
  labs(title = "Smooth Population Trend - Manitoba",
       x = "Year", y = "Population") +
  theme_minimal()


- grid.arrange(plot_ontario, plot_manitoba, ncol = 1)

![image](https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/9bd860d3-3ae6-4f8c-ac05-dc7aeb91cdcc)


# linear regression model - For Ontario 

- final_data_modified$Chicks_Survived_Ontario <- as.numeric(final_data_modified$Chicks_Survived_Ontario == "Yes")
- model <- lm(Population_Ontario ~ Temperature_Ontario + HabitatLoss_Ontario + FoodAvailability_Ontario + Chicks_Survived_Ontario, data = final_data_modified)
- summary(model)

<img width="800" alt="Screen Shot 2023-10-23 at 5 36 21 PM" src="https://github.com/SethCodesABitForSchool/Birds-Canada/assets/147195203/8e4ad2ce-7407-468c-a1b8-a87be45d6d11">


# The analysis says 

- Temperature_Ontario (7.5598): For a one-unit increase in Temperature_Ontario, Population_Ontario is expected to increase by 7.5598 units, holding other variables constant.
- HabitatLoss_Ontario (-4.6760): For a one-unit increase in HabitatLoss_Ontario, Population_Ontario is expected to decrease by 4.6760 units, holding other variables constant.
- The effect of FoodAvailability_Ontario on Population_Ontario is not statistically significant.
- The coefficient for Chicks_Survived_Ontario is NA - perfect multicollinearity.
- The o/p say that both Temperature_Ontario and HabitatLoss_Ontario have statistically significant associations with the population of Ontario, as evidenced by their low p-values.
- Specifically, an increase in temperature is positively correlated with population growth, while habitat loss exhibits a negative correlation. 

