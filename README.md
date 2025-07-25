# codsoft-movie-rating-prediction-with-python


## Introduction
Movie Rating Prediction project involves building a model that predicts the rating of a movie based on features like genre, director, and actors. We use regression techniques to tackle this exciting problem. This enables us to explore data analysis, preprocessing, feature engineering, and machine learning modeling techniques

## Goal
The main goal of this project is to analyze historical movie data and develop a model that accurately estimates the rating given to a movie by users or critics. By doing so, we aim to provide insights into the factors that influence movie ratings and create a model that can estimate the ratings of movies accurately.

## Quick Link
Dataset: [Movie Rating Prediction Dataset](https://www.kaggle.com/datasets/adrianmcmahon/imdb-india-movies)

## Technologies Used
- Python
- Jupyter Notebook
- Libraries: pandas, numpy, scikit-learn, seaborn, and matplotlib
  
## Project Steps

### 1. Importing Libraries
```python
# import necessary libraries required
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score
from sklearn.neighbors import KNeighborsRegressor
```

### 2. Reading Data
```python
# read the dataset into a dataframe
df = pd.read_csv("movies.csv", encoding='latin1')
# show first five records of dataframe
df.head()
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/e6694a77-4b7d-450b-bc41-98d4c33f0c08)

### 3. Data Preprocessing
```python
# show the number of records and observations in the dataframe
df.shape
```
![shape](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/1ff0299c-3bc2-48e5-ba4c-28f3b7984b6a)

```python
# check out the information on the dataframe
df.info()
```
![df info](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/cb9ec138-e0ba-4d89-a3a2-fac9a61ecc6f)

```python
# check out the missing values in each observation
df.isna().sum()
```
![null sum](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/f7b67286-f674-45b5-afd3-ab9de0ee15bc)

```python
# drop records with missing value in any of the following columns: Name, Year, Duration, Votes, Rating
df.dropna(subset=['Name', 'Year', 'Duration', 'Votes', 'Rating'], inplace=True)

# check the missing values in each observation again
df.isna().sum()
```
![drop na values](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/d94dae87-123b-46f5-bd45-ad9677cdca44)

```python
# remove rows with duplicate movie records
df.drop_duplicates(subset=['Name', 'Year', 'Director'], keep='first', inplace=True)
```
![duplicates](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/d620b0be-77f8-4f15-87df-4956659608cf)

```python
# remove () from the Year column values and change the datatype to integer
df['Year'] = df['Year'].str.strip('()').astype(int)
```
![year](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/8a4d4334-a9e5-43da-ba2d-53ddeb5e30bb)

```python
# remove minutes from the Duration column values
df['Duration'] = df['Duration'].str.replace(r' min', '').astype(int)
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/dcd77be1-9228-4f6e-903c-781e49db55fb)

```python
# remove commas from Votes column and convert to integer
df['Votes'] = df['Votes'].str.replace(',', '').astype(int)

# show the number of records and observations after cleaning the dataframe
df.shape
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/0bccb3c7-c02a-4ec7-83e0-53dc169baa9e)

```python
# show the info on the cleaned dataframe
df.info()
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/e60eb157-1db1-43c4-9b4a-d0cbad91369e)

```python
# show the statistics of the dataframe
df.describe()
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/59b1d3f0-bd21-4ec5-a035-daed3d7791b4)

### 4. Exploratory Data Analysis (EDA)

#### i. Number of Movies each Year
```python
# group the data by Year and count the number of movies in each year
yearly_movie_counts = df['Year'].value_counts().sort_index()

# create a bar chart
plt.figure(figsize=(18, 9))
plt.bar(yearly_movie_counts.index, yearly_movie_counts.values, color='darkred')
plt.xlabel('Year')
plt.ylabel('Number of Movies')
plt.title('Number of Movies Released Each Year')

# Show every second year on the x-axis and rotate x-labels for better readability
plt.xticks(yearly_movie_counts.index[::2], rotation=90)

for bar in bars:
    xval = bar.get_x() + bar.get_width() / 2
    yval = bar.get_height()
    plt.text(xval, yval, int(yval), ha='center', va='bottom', rotation= 90)

plt.show()
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/06e9d637-468b-4eb4-b94e-ed2de65c5b2d)

#### ii. Creating Genre Dummy Columns and Analyzing Movie Counts by Genre
```python
# create dummy columns for each genre
dummies = df['Genre'].str.get_dummies(', ')
# creating a new dataframe which combines df and dummies
df_genre = pd.concat([df, dummies], axis=1)
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/ff5be8d9-4a1d-4ba2-a3fb-a8ce75b33eeb)

```python
genre_columns = df_genre.columns[10:]  # Assuming genre columns start from the 11th column
genre_columns
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/bfca044b-f95c-4344-b59a-7195c7048043)
```python
# group the data by genre_columns and count the number of movies in each genre
genre_movie_counts = df_genre[genre_columns].sum().sort_index()

# create a bar chart
plt.figure(figsize=(18, 9))
plt.bar(genre_movie_counts.index, genre_movie_counts.values, color='darkred')
plt.xlabel('Genre')
plt.ylabel('Number of Movies')
plt.title('Number of Movies Released Per Genre')

plt.xticks(rotation=90)  

for bar in bars:
    xval = bar.get_x() + bar.get_width() / 2
    yval = bar.get_height()
    plt.text(xval, yval, int(yval), ha='center', va='bottom')

plt.show()
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/7dde9e14-dc5d-4d30-a50b-d43c1e64e008)

#### iii. Top 20 Directors with the Most Movies
```python
# Analyzing count of movies of each director
director_movie_counts = df['Director'].value_counts()

# Create a bar chart
plt.figure(figsize=(10, 5))
bars = director_movie_counts.head(20).plot(kind='bar', color='maroon')
plt.xlabel('Director')
plt.ylabel('Number of Movies')
plt.title('Top 20 Directors with the Most Movies')
plt.xticks(rotation=90) 

# Add count labels on top of the bars
for bar in bars.patches:
    xval = bar.get_x() + bar.get_width() / 2
    yval = bar.get_height()
    plt.text(xval, yval, int(yval), ha='center', va='bottom')

plt.show()
```

![director count](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/86517817-b813-42f6-b18b-97af6f2d19b5)

#### iv. Top 20 Actors with the most Movies

```python
# To Count Top 20 movies for each actor
actor_movie_counts = df['Actor 1'].value_counts()

# Create a bar chart
plt.figure(figsize=(10, 5))
actor_movie_counts.head(20).plot(kind='bar', color='maroon')
plt.xlabel('Actors')
plt.ylabel('Number of Movies')
plt.title('Top 20 Actors with the Most Movies')
plt.xticks(rotation=90) 

# Add count labels on top of the bars
for i, v in enumerate(actor_movie_counts.head(20)):
    plt.text(i, v, str(v), ha='center', va='bottom')

plt.show()
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/c22865c7-9456-48d2-b7a5-96e4ecd59e6c)

#### v. Movie Duration vs. Rating Scatter Plot
```python
plt.figure(figsize=(20, 8))
# create a scatter plot with Duration and Rating relationship
sns.scatterplot(x=df['Duration'], y=df['Rating'],  color = 'maroon')
plt.xlabel('Duration of Movie (mins)')
plt.ylabel('Movie Rating')
plt.title('Movie Duration vs Rating')
plt.show()
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/21e4732e-700a-4cdd-800b-8d8b621b44e4)

### 5. Feature Engineering
```python
# dropping the columns from the dataframe since these are the least dependable observations for target variable 'Rating'
df.drop(['Name','Director','Actor 1','Actor 2','Actor 3'], axis=1,inplace=True)
# show first five records of the dataframe
df.head()
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/7e5a2962-1db6-49e6-8bac-139df715f303)

```python
# creating target variable and learning observations for the model
X = df[['Year','Duration','Votes']]
y = df['Rating']

# split the data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=231)
```

### 6. Machine Learning Modeling Techniques

#### i. Linear Regression Model
```python
# creating a liner regression model
lr = LinearRegression()

# training the data on linear regression model
lr.fit(X_train, y_train)

# predicting the test data on trained model
pred = lr.predict(X_test)

# evaluating linear regression model
r2_score(y_test,pred)
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/5debc250-5bd3-4ee2-9312-55692a90592c)

#### ii. K-Nearest Neighbors (KNN) Regression Model
```python
# creating a range for number of neighbors parameter of the KNN model
kRange = range(1,40,1)

# creating an empty scores list
scores_list = []

# iterate every value in kRange list 
for i in kRange:
    # create a K Nearest Neighbor model with i as number of neighbors
    regressor_knn = KNeighborsRegressor(n_neighbors = i)
    
    # fit training data to the KNN model
    regressor_knn.fit(X_train,y_train)
    # evaluate the model
    pred = regressor_knn.predict(X_test)
    
    # append the regression score for evaluation of the model to scores_list
    scores_list.append(r2_score(y_test,pred))
```

```python
plt.figure(figsize=(12,8))
# create a line graph for showing regression score (scores_list) for respective number of neighbors used in the KNN model
plt.plot(kRange, scores_list, linewidth=2, color='green')
# values for x-axis should be the number of neighbors stored in kRange
plt.xticks(kRange)
plt.xlabel('Neighbor Number')
plt.ylabel('r2_Score of KNN')
plt.show() 
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/833215bc-6375-4d0d-a411-3950516e2de8)

```python 
# Creating a KNN model with best parameters i.e., number of neighbors = 23
regressor_knn = KNeighborsRegressor(n_neighbors = 23)

# fit training data to the KNN model
regressor_knn.fit(X_train,y_train)
# evaluate test data on the model
pred = regressor_knn.predict(X_test)
# show regression score
r2_score(y_test,pred)
```

![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/6e40f640-6e68-41cb-9dc4-eb7cd090cdb0)

#### iii. SGD Regression

```python
from sklearn.linear_model import SGDRegressor
from sklearn.metrics import r2_score

# Create an instance of the SGDRegressor
sgd_regressor = SGDRegressor(max_iter=100, random_state=1)  # You can adjust the max_iter and random_state

# Fit the model to your training data
sgd_regressor.fit(X_train, y_train)

# Make predictions
pred = sgd_regressor.predict(X_test)

# Evaluate the model
r2 = r2_score(y_test, pred)

print("R-squared score:", r2)
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/68688281-9693-4b69-99b6-97a3c02a4d27)

#### iv. Random Forest Regression
```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import r2_score

rf_regressor = RandomForestRegressor(n_estimators=100, random_state=1)
rf_regressor.fit(X_train, y_train)
rf_pred = rf_regressor.predict(X_test)
r2_rf = r2_score(y_test, rf_pred)
print(f'R-squared score (Random Forest): {r2_rf}')
```
![random forest](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/138512da-a7cc-4815-be15-1c50eaf72b8d)

#### v. Gradient Boosting Regression
```python
from sklearn.ensemble import GradientBoostingRegressor
gb_regressor = GradientBoostingRegressor(n_estimators=100, random_state=231)
gb_regressor.fit(X_train, y_train)
gb_pred = gb_regressor.predict(X_test)
r2_gb = r2_score(y_test, gb_pred)
print(f'R-squared score: {r2_gb}')
```
![image](https://github.com/Tayyaba-Abro/CodSoft-Internship-Task---Movie-Rating-Prediction-with-Python/assets/47588244/89bab72a-ad5d-419c-b5af-a125c2870050)

!pip install textblob
from textblob import TextBlob
reviews = [
    "This movie was absolutely fantastic! The acting was superb.",
    "Terrible plot with bad acting. Would not recommend.",
    "Average movie with some good moments but poor ending."
]
for review in reviews:
    analysis = TextBlob(review)
    print(f"Review: {review}")
    print(f"Sentiment: {analysis.sentiment.polarity:.2f}")
    print(f"Subjectivity: {analysis.sentiment.subjectivity:.2f}\n")
    from statsmodels.tsa.arima.model import ARIMA
yearly_avg = df.groupby('Year')['Rating'].mean()



# Convert the index to datetime objects
yearly_avg.index = pd.to_datetime(-yearly_avg.index, format='%Y')


model = ARIMA(yearly_avg, order=(1,1,1))
results = model.fit()
forecast = results.get_forecast(steps=5)
forecast_index = pd.date_range(start=yearly_avg.index.max(), periods=6, freq='Y')[1:]

plt.figure(figsize=(12,6))
plt.plot(yearly_avg.index, yearly_avg, label='Historical')
plt.plot(forecast_index, forecast.predicted_mean, label='Forecast', color='red')
plt.fill_between(forecast_index,
                forecast.conf_int()['lower Rating'],
                forecast.conf_int()['upper Rating'],
                color='pink', alpha=0.3)
plt.title('Movie Rating Trends and Forecast')
plt.legend()
plt.show()


"C:\Users\jeeva\Downloads\Screenshot_4-7-2025_19034_colab.research.google.com.jpeg"
## Conclusion
In conclusion, this project allowed us to explore various data analysis, data preprocessing, feature engineering, and machine learning modeling techniques. It provided valuable insights into the factors influencing movie ratings and equipped us with a model for accurate movie rating predictions.



