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
