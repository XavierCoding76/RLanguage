****************************************************************************************************************
Brief Description
- This file requires R 4.5.0 to run.
- This code consists of the fundamental codes and their explanation, which is written in the form of comment.
****************************************************************************************************************

  R Data Analysis & Manipulation Examples
This README provides an overview and examples of common data analysis and manipulation techniques in R, using various packages like dplyr, tidyr, and ggplot2. It covers foundational R concepts, data frame operations, and essential tidyverse functions for transforming and summarizing data.

1. Environment Setup
This section outlines the R packages required for the examples provided.

# Seeks for the dplyr module, and installs it to allow to code to run it, if needed.
library(dplyr)
# Seeks for the tidyr module.
library(tidyr)
# Seeks for the ggplot2 module.
library(ggplot2)

2. Basic R Operations
Here are some fundamental R operations for variables, vectors, and basic calculations.

Variable & Vector Declaration
# Declaration of variables in R
a <- 5
b <- 6

# The c represents binding elements together. (Concatenate)
# - Reminder: It has to be the same data type.
ages <- c(a, b, 8)

# Print function basically do print, yeah, just print.
names <- c("John", "Chris", "Nigga")
print(names)

Numeric Vector Operations
# Total up the integers in a numeric vector (Array)
# - Reminder: Only usable to integers.
sum(ages)
# - Reminder: You can also use it for a single row in a table.
# sum(linked$ages)

3. Data Frames: Creation & Inspection
Learn how to create data frames and inspect their structure and content.

Creating a Data Frame
A data.frame links data from different vectors that share the same relative location.

linked <- data.frame(names, ages)

Inspecting Data Frames
# View the linked data in a table.
View(linked)

# Quick overview of the data
glimpse(linked)

# Views the first 6 rows of data.
# - Reminder: The number of data to be seen can be adjusted by adding a number behind the dataset name.
head(StudentList)

# Specifies the datatype of a variable in a dataset.
class(linked$age)

# Typically views how many variables are in a dataset.
length(linked)

# Shows all the names of variables in a dataset.
names(linked)

# Specifies all the unique data in a dataset.
unique(linked$age)

# View the details of the linked data in a table.
str(linked)

# The data() function is used to load datasets that are stored in a specific way within R or within an R package.
# - Reminder: If no datasets were inserted in the parameter list, the system will popup a R datasets, in which states the datasets reserved.
data()

Accessing Data Frame Elements
# Prints the data that stays in that location, depending on the specified row or column.
# Shows the data that sits in the first row, first column.
linked[1,1]
# Typically shows the variables of the whole table.
linked[1, ]
# Shows the first row of data.
linked[, 1]

Example: Creating a StudentList Dataset
StudentList <- data.frame(
  ID = 1:100,
  Name = paste0("Name", 1:100),
  Age = sample(18:60, 100, replace = TRUE),
  Score = round(runif(100, 0, 100), 1),
  Passed = sample(c(TRUE, FALSE), 100, replace = TRUE),
  Group = factor(sample(c("A", "B", "C"), 100, replace = TRUE)),
  DateJoined = sample(seq(as.Date('2020-01-01'), as.Date('2023-01-01'), by="day"), 100)
)

# Note: Directly typing the name will print the whole dataset.
StudentList

4. Data Transformation with dplyr
The dplyr package is crucial for efficient data manipulation. The %>% (pipe operator) connects functions, making code more readable.

Chaining Operations (Conditional Search & Modify)
starwars %>%
  # filter filters the data based on the requirements given.
  filter(height > 150) %>%
  # Mutate changes the data format desired.
  mutate(height_in_meters = height/100) %>%
  # Select selects the data rows that will be shown.
  select(height_in_meters, mass) %>%
  # Arrange orders the data rows based on the desired variable.
  # - Reminder: If doing it in descending order, do as such: arrange(desc(mass))
  arrange(mass) %>%
  # Plots the graph based on what was selected before.
  # - Reminder: It has to be connected with pipe operator to view the plot.
  plot()

Selecting Variables
# These two codes do the same.
starwars %>%
  select("name", "height", "mass")

# This selects the data by selecting the variable continuously.
starwars %>%
  select(1:3)

# If you want to specific which column to select, do this instead.
starwars %>%
  select(c(1, 3)) %>% # Selects column 1 (name) and column 3 (mass)
  head()

# This filters the variables that fulfill the requirement, through using helper functions.
starwars %>%
  select(ends_with("color")) %>%
  View()

# You can also mix continuous and non-continuous columns
starwars %>%
  select(c(1:3, 5, 8)) %>% # Selects 1,2,3, then 5, then 8
  head()

# Changing variable order.
starwars %>%
  select(name, mass, height, everything())

Renaming Variables
starwars %>%
  rename("characters" = "name") %>%
  head()

Changing Variable Type
# Step 1: Change the type
class(starwars$hair_color)
starwars$hair_color <- as.factor(starwars$hair_color)
class(starwars$hair_color)

# Step 2: Change the data
starwars %>%
  mutate(hair_color = as.character(hair_color)) %>%
  glimpse()

Changing Factor Levels
# Sets a temporary list to dataset, which can prevent unwanted changes toward the original dataset.
df <- starwars

# Sets the variable into factor from anything else.
df$sex <- as.factor(df$sex)

df <- df %>%
  mutate(sex = factor(sex, levels = c("male", "female", "hermaphroditic", "none")))

# This line of code shows all the factors in the variable.
levels(df$sex)

Recoding Data
starwars %>%
  select(sex) %>%
  mutate(sex = recode(sex, "male" = "alpha male", "female" = "alpha female"))

5. Handling Missing Data
Dealing with missing values (NA) is a common task in data cleaning.

Identifying Missing Data
customer_data <- data.frame(
  CustomerID = c(101, 102, 103, 104, 105),
  Name = c("Alice", "Bob", NA, "David", "Eve"), # NA in row 3
  Age = c(28, 35, 42, NA, 30), # NA in row 4
  Products = I(list(
    c("Shirt", "Pants"),
    "Laptop",
    c("Book", "Pen", "Bag"),
    list(),
    "Keyboard"
  )),
  stringsAsFactors = FALSE
)

# This code identifies rows with missing values in specified columns.
missing_flags <- !complete.cases(customer_data[, c("CustomerID", "Name", "Age")])
customer_data[missing_flags, ]

Removing Missing Data
# - Reminder: This is an numeric example.
mean(starwars$height, na.rm = TRUE)

# Drops row that has NA on specified variable.
starwars %>%
  drop_na(hair_color)

6. Dealing with Duplicates
Remove duplicate rows from your dataset.

# - Reminder: It removes data in dataset which has exact same value for all variables.
example_name <- c("Nigga", "John", "Nigga", "Ching Chong")
example_age <- c(22, 33, 22, 44)

example_friends <- data.frame(example_name, example_age)

example_friends

example_friends %>%
  distinct()

distinct(example_friends)

7. Reshaping Data with tidyr
The tidyr package helps in restructuring your data for easier analysis, particularly with pivot_wider and pivot_longer.

pivot_wider: From Long to Wide Format
Makes data easier to be viewed, as it'll be grouped row by row based on a key variable (e.g., year).

library(gapminder)
View(gapminder)

gapminder_data <- select(gapminder, country, year, lifeExp)
View(gapminder_data)

wide_data <- gapminder_data %>%
  pivot_wider(names_from = year, values_from = lifeExp)

View(wide_data)

pivot_longer: From Wide to Long Format
Converts data grouped columns by columns back into a long format.

long_data <- wide_data %>%
  pivot_longer(2:13, names_to = "year", values_to = "lifeExp")

View(long_data)

8. Descriptive Statistics
Understand the central tendency and spread of your data using various statistical functions. The msleep dataset is used for these examples.

View(msleep)

Range / Spread
min(msleep$awake)
max(msleep$awake)
range(msleep$awake)
IQR(msleep$awake)

Centrality
mean(msleep$awake)
median(msleep$awake)

Variance
var(msleep$awake)

Summary Functions
# Provide data of minimum value, first quartile, median, mean, third quartile and maximum value.
summary(msleep$awake)

# Selects multiple variables to do summary.
msleep %>%
  select(awake, sleep_total) %>%
  summary()

# Customize your own summary
msleep %>%
  drop_na(vore) %>%
  group_by(vore) %>%
  summarise(Lower = min(sleep_total),
            Average = mean(sleep_total),
            Upper = max(sleep_total),
            Difference = max(sleep_total - min(sleep_total))) %>%
  arrange(Average) %>%
  View()
