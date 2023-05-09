
```{r myprettycode1, echo=TRUE, fig.cap='Summary of Key Variables'}

new_hdi<- HDI[, c("Country", "HDI", "Life expectancy", "Mean years of schooling", "Gross national income (GNI) per capita")]

# Change the names of the columns
colnames(new_hdi) <- c("Country", "HDI", "LifeExpectancy", "MeanYearsSchooling", "GNIperCapita")

new_hdi <- na.omit(new_hdi)

# Print summary of key variables in a table
kbl(summary(new_hdi)) %>%
  kable_classic() %>%
  kable_styling()
```

This code chunk first creates a new data frame called new_hdi from the HDI data frame, containing only the variables "Country", "HDI", "Life expectancy", "Mean years of schooling", and "Gross national income (GNI) per capita". It then renames the columns in new_hdi using the colnames() function. Finally, it removes any rows with missing values using the na.omit() function, and outputs a summary of key variables using the summary() function, which is then formatted into a table using the kable() and kable_styling() functions from the kableExtra package.

```{r myprettycode2, echo=TRUE, fig.cap='Graph of HDI by Country'}

ggplot(data=new_hdi, aes(x=reorder(Country, HDI), y=HDI)) +
  geom_bar(stat="identity", fill="#3399FF", width=0.8) +
  theme(axis.text.x = element_text(angle=90, vjust=0.5, hjust=1, size=4),
        axis.text.y = element_text(size = 12),
        axis.title = element_text(size = 12),
        plot.title = element_text(size = 14)) +
  labs(title="HDI by Country", x="Country", y="HDI") +
  scale_x_discrete(labels=function(x) str_wrap(x,  width=50)) + scale_y_continuous(expand=c(0, 0.1))


```

This code chunk creates a bar graph of the HDI by country using the ggplot2 package. The output of this code chunk is a graph of the HDI by country. Each bar represents the HDI of a country, and the countries are sorted in descending order by HDI. The x-axis shows the name of the country and the y-axis shows the HDI value. The graph also includes a title and labels for the x and y axis. The theme() function is used to customize the appearance of the graph.

```{r, model, echo=TRUE}

# Check for missing values
sum(is.na(new_hdi))

# Descriptive statistics
summary(new_hdi)

# Bivariate analysis
ggplot(new_hdi, aes(x = GNIperCapita, y = HDI)) +
   geom_point(color = "blue") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Bivariate Analysis of HDI and GDP per capita",
       x = "GNI per capita", y = "HDI") + scale_x_continuous(labels = scales::comma)

# Bivariate analysis
ggplot(new_hdi, aes(x = LifeExpectancy, y = HDI)) +
  geom_point(color = "blue") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Bivariate Analysis of HDI and Life Expectancy",
       x = "Life Expectancy", y = "HDI") + scale_x_continuous(labels = scales::comma)

# Bivariate analysis
ggplot(new_hdi, aes(x =  MeanYearsSchooling , y = HDI)) +
  geom_point(color = "blue") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Bivariate Analysis of HDI and Mean Years of Schooling",
       x = "Mean Years of Schooling", y = "HDI") + scale_x_continuous(labels = scales::comma)

# Multivariate analysis
model <- lm(HDI ~ GNIperCapita+LifeExpectancy+MeanYearsSchooling, data = new_hdi)
summary(model)


# Create table of regression summary using kableExtra
kbl(summary(model)$coef, digits = 4, caption = "Regression Summary") %>%
  kable_classic() %>%
  kable_styling()
```

Brief explanations for each code chunk:

1. `sum(is.na(new_hdi))`: This code chunk checks for missing values in the `new_hdi` dataset by using the `is.na()` function to create a logical vector of `TRUE` and `FALSE` values for each element in the dataset, and then taking the sum of those values (since `TRUE` is counted as 1 and `FALSE` is counted as 0).
Output: In this case, the output is 0, indicating that there are no missing values in the dataset.

2. `summary(new_hdi)`: This code chunk generates a summary of the `new_hdi` dataset using the `summary()` function. The summary includes basic statistics (mean, median, etc.) for each variable in the dataset.
Output: This code generates a summary of the `new_hdi` dataset, which includes minimum, 1st quartile, median, mean, 3rd quartile, and maximum values for each variable in the dataset.

3. `ggplot(new_hdi, aes(x = GNIperCapita, y = HDI)) + geom_point(color = "blue") + geom_smooth(method = "lm", se = FALSE, color = "red") + labs(title = "Bivariate Analysis of HDI and GDP per capita", x = "GNI per capita", y = "HDI") + scale_x_continuous(labels = scales::comma)`: This code chunk creates a scatterplot of HDI versus GNI per capita, with a blue point for each country and a red linear regression line fitted to the data. It also includes axis labels, a title, and a scale for the x-axis that formats the labels with commas to make them easier to read.
Output: This code generates a scatterplot with a linear regression line that shows the relationship between HDI and GNI per capita.

4. `ggplot(new_hdi, aes(x = LifeExpectancy, y = HDI)) + geom_point(color = "blue") + geom_smooth(method = "lm", se = FALSE, color = "red") + labs(title = "Bivariate Analysis of HDI and Life Expectancy", x = "Life Expectancy", y = "HDI") + scale_x_continuous(labels = scales::comma)`: This code chunk creates a scatterplot of HDI versus life expectancy, with a blue point for each country and a red linear regression line fitted to the data. It also includes axis labels, a title, and a scale for the x-axis that formats the labels with commas to make them easier to read.
Output: This code generates a scatterplot with a linear regression line that shows the relationship between HDI and life expectancy.

5. `ggplot(new_hdi, aes(x = MeanYearsSchooling, y = HDI)) + geom_point(color = "blue") + geom_smooth(method = "lm", se = FALSE, color = "red") + labs(title = "Bivariate Analysis of HDI and Mean Years of Schooling", x = "Mean Years of Schooling", y = "HDI") + scale_x_continuous(labels = scales::comma)`: This code chunk creates a scatterplot of HDI versus mean years of schooling, with a blue point for each country and a red linear regression line fitted to the data. It also includes axis labels, a title, and a scale for the x-axis that formats the labels with commas to make them easier to read.
Output: This code generates a scatterplot with a linear regression line that shows the relationship between HDI and mean years of schooling.

6. model <- lm(HDI ~ GNIperCapita+LifeExpectancy+MeanYearsSchooling, data = new_hdi)

```{r, results='asis', echo=TRUE}

  stargazer(model, type = 'html', keep.stat = 'n')
 
```
This code chunk uses the stargazer package to create an HTML table that summarizes the results of the linear regression model created in the previous chunk of code. The table includes coefficients, standard errors, t-values, and p-values for each predictor variable, as well as the R-squared and adjusted R-squared values for the model.

The results='asis' option is used to display the HTML output in the R Markdown document, and keep.stat = 'n' is used to exclude the number of observations and the residual standard error from the output.
