# Matplotlib

## Ways of creating plot

### PyPlot

```python
# Import matplotlib and setup the figures to display within the notebook
%matplotlib inline
import matplotlib.pyplot as plt
```

```python
x = [1, 2, 3, 4]
y = [3, 4, 1, 5]
plt.plot(x, y)
plt.show()
```

### Object oriented API (Recommended)

```python
x = [1, 2, 3, 4]
y = [3, 4, 1, 5]

# Create a fig of dimension 5,10
fig, ax = plt.subplots(figsize = (5, 10))
ax.plot(x, y)


# 4. Customize plot
ax.set(title="Sample Simple Plot", xlabel="x-axis", ylabel="y-axis")

# 5. Save & show
fig.savefig("../images/simple-plot.png")
```

### &#x20;Types of plots using NumPy arrays <a href="#id-2.-making-the-most-common-type-of-plots-using-numpy-arrays" id="id-2.-making-the-most-common-type-of-plots-using-numpy-arrays"></a>

* `line`
* `scatter`
* `bar`
* `hist`
* `subplots()`

### Import and Create array

```python
y = np.random.randint(10, size = 100)
x = np.arange(0, 200, 2)
```

```python
fig, ax = plt.subplots(figsize=(10, 5))
```

### Line

```python
# The default plot is line
fig, ax = plt.subplots()
ax.plot(x, x**2);
```

### Scatter

```python
We Need to recreate our figure and axis instances when we want a new figure
fig, ax = plt.subplots()
ax.scatter(x, np.exp(x));
```

### Bar

```python
You can make plots from a dictionary
nut_butter_prices = {"Almond butter": 10,
                     "Peanut butter": 8,
                     "Cashew butter": 12}
fig, ax = plt.subplots()
# Vertical bars
ax.bar(nut_butter_prices.keys(), nut_butter_prices.values())
ax.set(title="Dan's Nut Butter Store", ylabel="Price ($)");

# Horizontal bars
fig, ax = plt.subplots()

# barh accepts list only
ax.barh(list(nut_butter_prices.keys()), list(nut_butter_prices.values()));
```

#### Histogram (hist)

```
Make some data from a normal distribution
x = np.random.randn(1000) # pulls data from a normal distribution

fig, ax = plt.subplots()
ax.hist(x);
```

#### Subplots ( More than one plot)

```python
Option 1: Create multiple subplots
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(nrows=2). 
                                             ncols=2, 
                                             figsize=(10, 5))

Plot data to each axis
ax1.plot(x, x/2);
ax2.scatter(np.random.random(10), np.random.random(10));
ax3.bar(nut_butter_prices.keys(), nut_butter_prices.values());
ax4.hist(np.random.randn(1000));


Option 2: Create multiple subplots
fig, ax = plt.subplots(nrows=2, ncols=2, figsize=(10, 5))

# Index to plot data
ax[0, 0].plot(x, x/2);
ax[0, 1].scatter(np.random.random(10), np.random.random(10));
ax[1, 0].bar(nut_butter_prices.keys(), nut_butter_prices.values());
ax[1, 1].hist(np.random.randn(1000));
```

## Plot from Pandas Dataframe <a href="#line-plot-from-a-pandas-dataframe" id="line-plot-from-a-pandas-dataframe"></a>

### Pd Series

```python
# Start with some dummy data
ts = pd.Series(np.random.randn(1000),
               index=pd.date_range('1/1/2024', periods=1000))

# Add up the values cumulatively
ts = ts.cumsum()

# Plot the values over time with a line plot (note: both of these will return the same thing)
# ts.cumsum().plot() # kind="line" is set by default
ts.plot(kind="line");
```

### Actual data from Car\_sales.csv file

```python
# Import the car sales data 
car_sales = pd.read_csv("../data/car-sales.csv")

# Remove price column symbols with regex
car_sales["Price"] = car_sales["Price"].str.replace('[\$\,\.]', '', regex=True)

# Convert Price column from str to int
car_sales["Price"] = pd.to_numeric(car_sales["Price"], errors='coerce')

# Add a new column, total_sales
car_sales["total_sales"] = car_sales["Price"].cumsum()

# Plot graph
car_sales.plot(x="sale_date", y="Price", kind='scatter');
```

### Plot bar graph

```python
x = np.random.randint(0, 10, size=(10, 5))
df = pd.DataFrame(x, columns=['a', 'b', 'c', 'd', 'e'])
df.plot(kind='bar', y=['a', 'b', 'c'])
```

### Plotting more advanced plots from a pandas DataFrame <a href="#id-4.-plotting-more-advanced-plots-from-a-pandas-dataframe" id="id-4.-plotting-more-advanced-plots-from-a-pandas-dataframe"></a>

```python
# Perform data analysis on patients over 50
over_50 = heart_disease[heart_disease["age"] > 50]

# Create a scatter plot directly from the pandas DataFrame
over_50.plot(kind="scatter",
             x="age", 
             y="chol", 
             c="target", # colour the dots by target value
             figsize=(10, 6));
```

### Using OO method mixed with pyplot

```
# Create a Figure and Axes instance
fig, ax = plt.subplots(figsize=(10, 6))

# Plot data from the DataFrame to the ax object
over_50.plot(kind="scatter", 
             x="age", 
             y="chol", 
             c="target", 
             ax=ax); # set the target Axes

# Customize the x-axis limits (to be within our target age ranges)
ax.set_xlim([45, 100]);

# Customize the plot parameters 
ax.set(title="Heart Disease and Cholesterol Levels",
       xlabel="Age",
       ylabel="Cholesterol");

# Setup the legend
ax.legend(*scatter.legend_elements(), 
          title="Target");
```

### OO method from scratch

```
# Create Figure and Axes instance
fig, ax = plt.subplots(figsize=(10, 6))

# Plot data directly to the Axes intance
scatter = ax.scatter(over_50["age"], 
                     over_50["chol"], 
                     c=over_50["target"]) # Colour the data with the "target" column

# Customize the plot parameters 
ax.set(title="Heart Disease and Cholesterol Levels",
       xlabel="Age",
       ylabel="Cholesterol");

# Setup the legend
ax.legend(*scatter.legend_elements(), 
          title="Target");
```
