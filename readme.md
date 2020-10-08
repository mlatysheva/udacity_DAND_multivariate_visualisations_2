
# Udactity Data Analyst

## Multivariate Visualizations Excercises, Part 2

### Scatterplots


```python
import pandas as pd
import seaborn as sb
import matplotlib.pyplot as plt
import numpy as np

%matplotlib inline
```


```python
pokemon = pd.read_csv('pokemon.csv')
pokemon.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>species</th>
      <th>generation_id</th>
      <th>height</th>
      <th>weight</th>
      <th>base_experience</th>
      <th>type_1</th>
      <th>type_2</th>
      <th>hp</th>
      <th>attack</th>
      <th>defense</th>
      <th>speed</th>
      <th>special-attack</th>
      <th>special-defense</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>bulbasaur</td>
      <td>1</td>
      <td>0.7</td>
      <td>6.9</td>
      <td>64</td>
      <td>grass</td>
      <td>poison</td>
      <td>45</td>
      <td>49</td>
      <td>49</td>
      <td>45</td>
      <td>65</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>ivysaur</td>
      <td>1</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>142</td>
      <td>grass</td>
      <td>poison</td>
      <td>60</td>
      <td>62</td>
      <td>63</td>
      <td>60</td>
      <td>80</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>venusaur</td>
      <td>1</td>
      <td>2.0</td>
      <td>100.0</td>
      <td>236</td>
      <td>grass</td>
      <td>poison</td>
      <td>80</td>
      <td>82</td>
      <td>83</td>
      <td>80</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>charmander</td>
      <td>1</td>
      <td>0.6</td>
      <td>8.5</td>
      <td>62</td>
      <td>fire</td>
      <td>NaN</td>
      <td>39</td>
      <td>52</td>
      <td>43</td>
      <td>65</td>
      <td>60</td>
      <td>50</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>charmeleon</td>
      <td>1</td>
      <td>1.1</td>
      <td>19.0</td>
      <td>142</td>
      <td>fire</td>
      <td>NaN</td>
      <td>58</td>
      <td>64</td>
      <td>58</td>
      <td>80</td>
      <td>80</td>
      <td>65</td>
    </tr>
  </tbody>
</table>
</div>



### Plot Matrices
To look at the relationship between many pairs of variables, rather than generating these bivariate plots one by one, a preliminary option you might consider for exploration is the creation of a plot matrix. In a plot matrix, a matrix of plots is generated. Each row and column represents a different variable, and a subplot against those variables is generated in each plot matrix cell. This contrasts with faceting, where rows and columns will subset the data, and the same variables are depicted in each subplot.

Seaborn's `PairGrid` class facilitates the creation of this kind of plot matrix.

By default, `PairGrid` only expects to depict numeric variables; a typical invocation of PairGrid plots the same variables on the horizontal and vertical axes. On the diagonals, where the row and column variables match, a histogram is plotted. Off the diagonals, a scatterplot between the two variables is created.


```python
pkmn_stats = ['hp', 'attack', 'defense', 'speed', 'special-attack', 'special-defense']
g = sb.PairGrid(data = pokemon, vars = pkmn_stats);
g = g.map_offdiag(plt.scatter)
g.map_diag(plt.hist);
```


![png](output_5_0.png)


If we want to look at the relationship between the numeric and categorical variables in the data, we need to set the different variable types on the rows and columns, then use an appropriate plot type for all matrix cells.


```python
g = sb.PairGrid(data = pokemon, x_vars = ['hp', 'attack', 'defense'],
               y_vars = ['type_1', 'type_2']);
g.map(sb.violinplot, inner = 'quartile');
```


![png](output_7_0.png)


The time it takes to render the plot depends on the number of data points you have and the number of variables you want to plot. Increasing the number of variables increases the number of plots that need to be rendered in a quadratic fashion. One recommended approach is to take a random subset of the data to plot in the plot matrix instead. Use the plot matrix to identify interesting variable pairs, and then follow it up with individual plots on the full data.

### Correlation Matrices

For numeric variables, it can be useful to create a correlation matrix as part of your exploration. While it's true that the `.corr` function is perfectly fine for computing and returning a matrix of correlation coefficients, it's not too much trouble to plot the matrix as a heat map to make it easier to see the strength of the relationships.


```python
pokemon[pkmn_stats].corr()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>hp</th>
      <th>attack</th>
      <th>defense</th>
      <th>speed</th>
      <th>special-attack</th>
      <th>special-defense</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>hp</th>
      <td>1.000000</td>
      <td>0.433318</td>
      <td>0.231540</td>
      <td>0.169512</td>
      <td>0.377446</td>
      <td>0.367949</td>
    </tr>
    <tr>
      <th>attack</th>
      <td>0.433318</td>
      <td>1.000000</td>
      <td>0.435514</td>
      <td>0.335289</td>
      <td>0.325937</td>
      <td>0.202138</td>
    </tr>
    <tr>
      <th>defense</th>
      <td>0.231540</td>
      <td>0.435514</td>
      <td>1.000000</td>
      <td>-0.023866</td>
      <td>0.199560</td>
      <td>0.508688</td>
    </tr>
    <tr>
      <th>speed</th>
      <td>0.169512</td>
      <td>0.335289</td>
      <td>-0.023866</td>
      <td>1.000000</td>
      <td>0.440411</td>
      <td>0.202847</td>
    </tr>
    <tr>
      <th>special-attack</th>
      <td>0.377446</td>
      <td>0.325937</td>
      <td>0.199560</td>
      <td>0.440411</td>
      <td>1.000000</td>
      <td>0.481345</td>
    </tr>
    <tr>
      <th>special-defense</th>
      <td>0.367949</td>
      <td>0.202138</td>
      <td>0.508688</td>
      <td>0.202847</td>
      <td>0.481345</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
sb.heatmap(pokemon[pkmn_stats].corr(), cmap = 'rocket_r', annot = True, fmt = '.2f', vmin = 0);
```


![png](output_11_0.png)


### Feature Engineering

Feature engineering is a tool that we can leverage as we explore and learn about our data. As we explore a dataset, we might find that two variables are related in some way. Feature engineering is all about creating a new variable with a sum, difference, product, or ratio between those original variables that may lend a better insight into the research questions we seek to answer.


```python
pokemon['atk_ratio'] = pokemon['attack'] / pokemon['special-attack']
pokemon['def_ratio'] = pokemon['defense'] / pokemon['special-defense']
```


```python
plt.scatter(data = pokemon, x = 'atk_ratio', y = 'def_ratio')
plt.xlabel('Offensive Bias (Phys/Spec)')
plt.ylabel('Defensive Bias (Phys/Spec)');
```


![png](output_14_0.png)



```python
plt.scatter(data = pokemon, x = 'atk_ratio', y = 'def_ratio', alpha = 1/3)
plt.xlabel('Offensive Bias (Phys/Spec)')
plt.ylabel('Defensive Bias (Phys/Spec)')
plt.xscale('log')
plt.yscale('log')
tick_loc = [0.25, 0.5, 1, 2, 4]
plt.xticks(tick_loc, tick_loc)
plt.yticks(tick_loc, tick_loc)
plt.xlim(2 ** -2.5, 2 ** 2.5)
plt.ylim(2 ** -2.5, 2 ** 2.5);
```


![png](output_15_0.png)


Another way that we can perform feature engineering is to use the `cut` function to divide a numeric variable into ordered bins. When we split a numeric variable into ordinal bins, it opens it up to more visual encodings. For example, we might facet plots by bins of a numeric variable, or use discrete color bins rather than a continuous color scale. This kind of discretization step might help in storytelling by clearing up noise, allowing the reader to concentrate on major trends in the data. 


```python

```
