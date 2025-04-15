# Airbnb-Analysis Using Python
# This comprehensive analysis provides insights factors affecting Airbnb pricing, ratings and other key metrics accross different cities in Europe , room types and other listing characters.
# Airbnb Data Analysis Code Explanation

This code analyzes an Airbnb dataset using statistical methods including ANOVA, Tukey's HSD test, and t-tests to understand relationships between various listing characteristics. Below is a detailed explanation of each section.

## Data Loading and Preprocessing

python
import pandas as pd

df1 = pd.read_csv("C:/Users/davek/OneDrive/Desktop/SSDI/DATASETS/AIRBNB_1.csv") 
df1.head()


This imports pandas and loads the Airbnb dataset from a CSV file[1]. The head() function displays the first 5 rows of data to provide a quick overview of the dataset structure[1].

python
df1.dtypes 
df1['Private_Room'] = df1['Private_Room'].astype(int)

df1.columns = df1.columns.str.strip()
float_columns = ['City_Center', 'Metro_Distance', 'Attraction_Index', 
                'Restraunt_Index', 'Normalised_Attraction', 'Normalised_Restraunt_Index'] 
df1[float_columns] = df1[float_columns].astype(int)


These lines check the data types and perform data conversions[1][2]:
- Convert the 'Private_Room' boolean column to integer (True/False becomes 1/0)
- Remove leading/trailing spaces from column names
- Convert several float columns to integer type for easier analysis[1]

## ANOVA Analysis

python
import statsmodels.api as sm
from statsmodels.formula.api import ols

dependent_vars = ['Price', 'Business', 'Cleanliness_Rating', 'Guest_Satisfaction',
                 'City_Center', 'Metro_Distance', 'Attraction_Index', 'Restraunt_Index',
                 'Normalised_Attraction','Normalised_Restraunt_Index','Private_Room']
independent_vars = 'City + Day + Room_Type + Superhost+ Shared_Room+Person_Capacity'

for dv in dependent_vars:
    formula = f'{dv} ~ {independent_vars}'
    model = ols(formula, data=df1).fit()
    anova_table = sm.stats.anova_lm(model, type=2)
    print(f"ANOVA results for {dv}:\n", anova_table, "\n")


This code performs Analysis of Variance (ANOVA)[1]:
- Imports statsmodels for statistical modeling
- Defines dependent variables (outcomes to analyze) and independent variables (predictors)
- Loops through each dependent variable and runs a Type II ANOVA
- Type II ANOVA tests the significance of each predictor while accounting for all other variables in the model
- Results show which factors significantly influence each dependent variable[1]

## Post-Hoc Analysis with Tukey's HSD Test

Multiple sections use Tukey's HSD (Honestly Significant Difference) test for pairwise comparisons:

python
from statsmodels.stats.multicomp import pairwise_tukeyhsd
tukey = pairwise_tukeyhsd(df1["Price"], df1['Day'])
print(tukey)


Tukey's HSD is a post-hoc test used after ANOVA to determine which specific groups differ from each other[1]. The code applies this test to various combinations, like:

- Price differences between Weekday and Weekend listings[1]
- Business ratings across different Room Types[1]
- Guest satisfaction differences between cities[1]
- City center proximity by Room Type[1]

## T-Tests Between Groups

The code creates subsets of data and uses t-tests to compare means:

python
df2 = df1[df1['Day']=='Weekday']
df3 = df1[df1['Day']=='Weekend']

from scipy import stats
stats.ttest_ind(df2["Price"], df3["Price"], equal_var=False, alternative="greater")


These t-tests[1]:
- Use Welch's t-test (equal_var=False) which doesn't assume equal variances
- Test directional hypotheses with alternative="greater"
- Compare means between different groups like:
  - Weekday vs Weekend prices
  - Private room vs Shared room business ratings
  - Properties with/without shared rooms and their cleanliness ratings
  - Barcelona vs Budapest guest satisfaction

## Additional Group Comparisons

The code explores numerous specific relationships:

python
df10 = df1[df1['Room_Type']=='Entire home/apt']
df11 = df1[df1['Room_Type']=='Shared room']
stats.ttest_ind(df10["City_Center"], df11["City_Center"], equal_var=False, alternative="greater")


These tests examine[1][2]:
- Distance to city center by accommodation type
- Metro distance differences by room type
- Attraction index differences by person capacity
- Restaurant index variations by room type
- Normalized attraction/restaurant indices across different properties

The comprehensive analysis provides insights into factors affecting Airbnb pricing, ratings, and other key metrics across different cities, room types, and other listing characteristics[1][2].

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/47424942/6e5008b4-566f-4522-be11-4b199bb9b8e8/SSDI-PROJECT-3.1.html
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/47424942/6e5008b4-566f-4522-be11-4b199bb9b8e8/SSDI-PROJECT-3.1.html

---
Answer from Perplexity: pplx.ai/share
