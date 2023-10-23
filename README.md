# Birds-Canada
Birds Canada Simulation Data 

# Set seed 
set.seed(123)

# years from 1920 to 2023
years <- 1920:2023

# Generate data for three variables: Temperature, Habitat Loss, and Food Availability
temperature <- rnorm(length(years), mean = 20, sd = 5)  # Normal distribution for temperature
habitat_loss <- runif(length(years), min = 0, max = 30)   # Uniform distribution for habitat loss
food_availability <- rpois(length(years), lambda = 50)    # Poisson distribution for food availability

# Calculate bird populations based on the three variables

bird_population <- 1000 + 5 * temperature - 2 * habitat_loss + 3 * log(food_availability) + rnorm(length(years), mean = 0, sd = 100)

# add number of chicks
number_of_chicks <- rpois(length(years), lambda = 10)  # Poisson distribution for the number of chicks

# Create a data frame

bird_data <- data.frame(
  Year = years,
  Temperature = temperature,
  HabitatLoss = habitat_loss,
  FoodAvailability = food_availability,
  Population = bird_population,
  NumberOfChicks = number_of_chicks
)


- head(bird_data)

- colnames(bird_data)



# Install and load the openxlsx package 
- install.packages("openxlsx")
- library(openxlsx)


- file_path <- "/Users/sethmenon/Downloads/bird.xlsx"


- write.xlsx(bird_data, file_path, sheetName = "BirdData", rowNames = FALSE)


- cat("Data exported to:", file_path, "\n")

- install.packages("tidyr")
- library(tidyr)

- bird_data_long <- gather(bird_data, key = "Variable", value = "Value", -Year)

- head(bird_data_long)

- library(tidyr)

- bird_data_long <- gather(bird_data, key = "Variable", value = "Value", -Year)

- head(bird_data_long)

- bird_data_long <- gather(bird_data, key = "Variable", value = "Value", -c(Year, Temperature, HabitatLoss, FoodAvailability, Population, NumberOfChicks))


- file_path_long <- "path/to/your/folder/bird_simulation_data_long.xlsx"

- write.xlsx(bird_data_long, file_path_long, sheetName = "BirdDataLong", row.names = FALSE)


- cat("Long-format data exported to:", file_path_long, "\n")

- library(tidyr)
- library(openxlsx)

- bird_data_long <- pivot_longer(bird_data, cols = -Year, names_to = "Variable", values_to = "Value")

- file_path_long <- "/Users/sethmenon/Downloads/bird-manitoba.xlsx"

- write.xlsx(bird_data_long, file_path_long, sheetName = "BirdDataLong", rowNames = FALSE)

- cat("Long-format data exported to:", file_path_long, "\n")

- bird_data$Location <- "Ontario"
- bird_data_long$Location <- "Manitoba"

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

- library(tidyr)


- install.packages("writexl")
- library(writexl)
-  file_path <- "/Users/sethmenon/Downloads/combined.xlsx"


- write_xlsx(combined_data, path = file_path)


- cat("Data exported to", file_path, "\n")

- head(combined_data_wide)
- library(dplyr)

# Chnage number of Chicks to a dummy variable 
- combined_data_wide <- combined_data_wide %>%
  mutate(Chicks_Survived_Ontario = ifelse(NumberOfChicks_Ontario > 0, "Yes", "No"),
         Chicks_Survived_Manitoba = ifelse(NumberOfChicks_Manitoba > 0, "Yes", "No"))


- final_data <- combined_data_wide2 %>%
  select(Year, starts_with("Temperature"), starts_with("HabitatLoss"), starts_with("FoodAvailability"),
         starts_with("Population"), starts_with("Chicks_Survived"))


- head(final_data)


- library(writexl)


- file_path <- "/Users/sethmenon/Downloads/final .xlsx"


- write_xlsx(combined_data_wide2, path = file_path)


- cat("Data exported to", file_path, "\n")


# Change the values of the variables as it is similar - this is so the plot will differ

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


- colnames(final_data)


- library(writexl)

- file_path <- "/Users/sethmenon/Downloads/final .xlsx"

- write_xlsx(final_data_modified, path = file_path)
- cat("Data exported to", file_path, "\n")

# Visualization

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









