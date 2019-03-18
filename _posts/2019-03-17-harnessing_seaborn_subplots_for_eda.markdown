---
layout: post
title:      "Harnessing Seaborn Subplots for EDA  "
date:       2019-03-17 21:32:20 -0400
permalink:  harnessing_seaborn_subplots_for_eda
---


Seaborn is known for making exploratory data analysis (EDA) easy and pleasing to the eye. Seaborn is built upon Matplotlib, however, the complexities of its syntax make it harder to customize than its Matplotlib counterparts. In this post, we shall be re-creating a version of a function I created for EDA called ‘plot_hist_scat_sns’ while explaining how to customize a Seaborn figure’s subplots.

```python
plot_hist_scat_sns(df_final_data,'price')
```
<img src="https://www.dropbox.com/s/x6f6bo662u8bxvm/sqft_living_dist_regr_plots.png?raw=1" width="800" height="400" />

This function creates a figure for every numerical data column in a dataframe vs the target variable Price. The figure consists of 2 subplots, a seaborn distplot on the left, normalized based on the kernel density estimation, and a seaborn regplot on the right, with a regression line for the relationship between the current variable and the target variable. This is a fantastic shortcut for initial inspection of a dataset.
In one clean 2-panel figure, you can simultaneously examine the distribution of each variable on the left, while examining the linear relationship between each predictor variable and the target on the right. This is vital information to consider when contemplating if your dataset requires transformation before it can be used in a linear regression model.


## Check out the full code for plot_hist_scat_sns before we break it down into pieces below.

```python

# Import matplot.pyplot, and matplotlib.ticker 
import matplotlib.ticker as mtick
import matplotlib.pyplot as plt
import seaborn as sns
plt.style.use('dark_background')

# Plots histogram and scatter (vs price) side by side
def plot_hist_scat_sns(df, target='price'):
    
    
    ## ----------- DEFINE AESTHETIC CUSTOMIZATIONS ----------- ##
   # Axis Label fonts
    fontTitle = {'fontsize': 16,
               'fontweight': 'bold',
                'fontfamily':'serif'}

    fontAxis = {'fontsize': 14,
               'fontweight': 'bold',
                'fontfamily':'serif'}

    fontTicks = {'fontsize': 12,
               'fontweight':'bold', 
                'fontfamily':'serif'}

    # Formatting dollar sign labels
    fmtPrice = '${x:,.0f}'
    tickPrice = mtick.StrMethodFormatter(fmtPrice)
    

    ## ----------- PLOTTING ----------- ##
    
    ## Loop through dataframe to plot
    for column in df.describe():
    
        # Create figure with subplots for current column
        # Note: in order to use identical syntax for large # of subplots (ax[i,j]), 
        #  declare an extra row of subplots to be removed later
        fig, ax = plt.subplots(figsize=(12,10), ncols=2, nrows=2)

        ## ----- SUBPLOT 1 -----##
        i,j = 0,0
        ax[i,j].set_title(column.capitalize(),fontdict=fontTitle)
        
        # Define graphing keyword dictionaries for distplot (Subplot 1)
        hist_kws = {"linewidth": 1, "alpha": 1, "color": 'blue','edgecolor':'w'}
        kde_kws = {"color": "white", "linewidth": 1, "label": "KDE"}
        
        # Plot distplot on ax[i,j] using hist_kws and kde_kws
        sns.distplot(df[column], norm_hist=True, kde=True,
                     hist_kws = hist_kws, kde_kws = kde_kws,
                     label=column+' histogram', ax=ax[i,j])
 

        # Set x axis label
        ax[i,j].set_xlabel(column.title(),fontdict=fontAxis)
    
        # Get x-ticks, rotate labels, and return
        xticklab1 = ax[i,j].get_xticklabels(which = 'both')
        ax[i,j].set_xticklabels(labels=xticklab1, fontdict=fontTicks, rotation=45)
        ax[i,j].xaxis.set_major_formatter(mtick.ScalarFormatter())

        
        # Set y-label 
        ax[i,j].set_ylabel('Density',fontdict=fontAxis)
        yticklab1=ax[i,j].get_yticklabels(which='both')
        ax[i,j].set_yticklabels(labels=yticklab1,fontdict=fontTicks)
        ax[i,j].yaxis.set_major_formatter(mtick.ScalarFormatter())
        
        
        # Set y-grid
        ax[i, j].set_axisbelow(True)
        ax[i, j].grid(axis='y',ls='--')

        
        ## ----- SUBPLOT 2-----  ##
        i,j = 0,1
        ax[i,j].set_title(column.capitalize(),fontdict=fontTitle)

        # Define the ketword dictionaries for  scatter plot and regression line (subplot 2)
        line_kws={"color":"white","alpha":0.5,"lw":4,"ls":":"}
        scatter_kws={'s': 2, 'alpha': 0.5,'marker':'.','color':'blue'}

        # Plot regplot on ax[i,j] using line_kws and scatter_kws
        sns.regplot(df[column], df[target], 
                    line_kws = line_kws,
                    scatter_kws = scatter_kws,
                    ax=ax[i,j])
        
        # Set x-axis label
        ax[i,j].set_xlabel(column.title(),fontdict=fontAxis)

         # Get x ticks, rotate labels, and return
        xticklab2=ax[i,j].get_xticklabels(which='both')
        ax[i,j].set_xticklabels(labels=xticklab2,fontdict=fontTicks, rotation=45)
        ax[i,j].xaxis.set_major_formatter(mtick.ScalarFormatter())

        # Set  y-axis label
        ax[i,j].set_ylabel('Price',fontdict=fontAxis)
        
        # Get, set, and format y-axis Price labels
        yticklab = ax[i,j].get_yticklabels()
        ax[i,j].set_yticklabels(yticklab,fontdict=fontTicks)
        ax[i,j].get_yaxis().set_major_formatter(tickPrice) 

        # Set y-grid
        ax[i, j].set_axisbelow(True)
        ax[i, j].grid(axis='y',ls='--')       
        
        ## ---------- Final layout adjustments ----------- ##
        # Deleted unused subplots 
        fig.delaxes(ax[1,1])
        fig.delaxes(ax[1,0])

        # Optimizing spatial layout
        fig.tight_layout()
    return 
```

## We are going to break-down how to achieve customized, attractive subplots in seaborn to empower you to create customized and clean figures in Seaborn.



#### Importing packages
```python
# Import matplot.pyplot, and matplotlib.ticker 
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.ticker as mtick

plt.style.use('dark_background')

# Plots histogram and scatter (vs price) side by side
def plot_hist_scat_sns(df, target='price'):

```
In addition to calling matplotlib.pyplot, as we usually do, we will also be taking advantage of matplotlib.ticker to customize the ticks of our axes. For these figures, I am using the ‘dark_background’ style. The function was constructed to be run on the King County housing data set and assumed a target variable of price.

#### Defining fonts for titles, axes, and ticks
```python
   ## ----------- DEFINE AESTHETIC CUSTOMIZATIONS ----------- ##
   # Axis Label fonts
    fontTitle = {'fontsize': 16,
               'fontweight': 'bold',
                'fontfamily':'serif'}

    fontAxis = {'fontsize': 14,
               'fontweight': 'bold',
                'fontfamily':'serif'}

    fontTicks = {'fontsize': 12,
               'fontweight':'bold', 
                'fontfamily':'serif'}								
```

We will be using the parameter **fontdict** in Seaborn/matplotlib in order to reference previously defined font dictionaries. In this example we have a separate fontdict for Titles, Axis Titles, and Ticks. We may define the fontfamily, fontsize, and fontweight. [To see the full list of parameters checkout the matplotlib documentation](https://matplotlib.org/tutorials/text/text_props.html#sphx-glr-tutorials-text-text-props-py) 

#### Formatting Price to include $'s and the comma separator for thousands ( i.e. $100,000)
```python
    # Formatting dollar sign labels
    fmtPrice = '${x:,.0f}'
    tickPrice = mtick.StrMethodFormatter(fmtPrice)
```

Here we are defining fmtPrice, which indicates we want a $ in front of our string x, using commas, with 0 decimal places. We then define tickPrice, which will use Matplotlib.ticker’s StrMethodFormatter to format out text when called during plotting.


#### Looping through the dataframe to produce separate figures with subplots for each column in our dataframe. 
```python
    ## ----------- PLOTTING ----------- ##
    
    ## Loop through dataframe to plot
    for column in df.describe():
    
        # Create figure with subplots for current column
        # Note: in order to use identical syntax for large # of subplots (ax[i,j]), 
        #  declare an extra row of subplots to be removed later
        fig, ax = plt.subplots(figsize=(12,10), ncols=2, nrows=2)
```
The important thing to note here is that **I declared my subplots from the beginning using plt.subplots(ncols,nrows)**. Even though our final figure will only have 2 subplots, I am declaring a subplot with 2 cols 2 rows. The reason I do this is to use the most flexible code that can easily be used in figures with more complicated subplots.
If we were to declare only 2 subplots, ax would be np array with 2 elements, one for each subplot. The first subplot would be ax[0], the second would be ax[1]. While this is straightforward and easy to code, it is not very flexible and would need to be re-written once we create more complex 2-D layouts.
 

With more than 1 row and 1 column, the addresses in ax become 2-D where the indices are: **ax[row, column]**. 
In our 2 x 2 subplot layout, the top-left subplot is ax[0,0], the top-right is ax[0,1], bottom-left is ax[1,0], and bottom-right is ax[1,1]. By declaring ncols=2, nrows=2, the syntax for all of the plots will be the same regardless of how many subplots are in the figure.

Instead of having to manually change all of the figure’s axes in each location of its code, we can simply refer to all axes by row i and column j, such that **every axis is referred to as ax[i,j]**. Then, we define what i and j before each plot, saving us from replacing each axis address independently.

```python
        ## ----- SUBPLOT 1 -----##
        i,j = 0,0
        ax[i,j].set_title(column.capitalize(),fontdict=fontTitle)
```
**See how we first declare i and j to be 0,0 respectively. Now we can refer to ax[i,j] and this code would work anywhere** in our figure without having to change the row and column each time.

**Also, see how when I call set_title,** I specify the title text first (column.capitalize()) and then feed in fontdict=fontTitle. This way, the title will be constructed with the font settings that we declared at the beginning of the function, without having to type all of that information for each plot.

 ### Now here is where things start to get tricky with Seaborn...**

#### Defining seaborn plot specifications before plotting
```python
        
        # Define graphing keyword dictionaries for distplot (Subplot 1)
        my_hist_kws = {"linewidth": 1, "alpha": 1, "color": 'blue','edgecolor':'w'}
        my_kde_kws = {"color": "white", "linewidth": 1, "label": "KDE"}
        
        # Plot distplot on ax[i,j] using hist_kws and kde_kws
        sns.distplot(df[column], norm_hist=True, kde=True,
                     hist_kws = my_hist_kws, kde_kws = my_kde_kws,
                     label=column+' histogram', ax=ax[i,j])
 ```
 
 Since Seaborn plots are usually actually built upon matplotlib's built-in functions, there are often a large number of possible parameters that could be passed when creating a plot. The way seaborn tries to simplify this is by creating a **keyword dictionary** for the different elements of the plot. 
 
 Here, we are going to be calling sns.distplot, which will be producing a histogram and kde plot. Accordingly, we will be defining the histogram keywords (**hist_kws**) and kde keywords (**kde_kws**). The keywords may contain any of the arguments that are normally accepted by the underlying matplotlib functions. In this case, we are specifying the line aesthetics for both the histogram and kde. In order to use our keyword dictionaries, we must pass in the name of the dict such as hist_kws = my_hist_kws, as seen above. 
 
 **The syntax for plotting a seaborn figure as a sublot is to add the ax parameter when you creat the subplot.**
 Here, that is passing ax=ax[i,j] as the final parameter.

#### Customizing x-axis labels and ticks
```python 
        # Set x axis label
        ax[i,j].set_xlabel(column.title(),fontdict=fontAxis)
```
Just like when we created our title, we pass the proper fontdict when we set our xlabel. 

```python
        # Get x-ticks, rotate labels, and return
        xticklab1 = ax[i,j].get_xticklabels(which = 'both')
```
In order to customize our ticks, we must first get the current x-tick lables (ax[i,j].get_xticklabels(which = 'both'), before we can then set our labels using our fontdict and adding 45 degree rotation to the labels. 
```python
        ax[i,j].set_xticklabels(labels=xticklab1, fontdict=fontTicks, rotation=45)
        ax[i,j].xaxis.set_major_formatter(mtick.ScalarFormatter())
```

#### Cutomizing y-axis labels and ticks using the same syntax
```python
        ax[i,j].set_ylabel('Density',fontdict=fontAxis)
        yticklab1=ax[i,j].get_yticklabels(which='both')
        ax[i,j].set_yticklabels(labels=yticklab1,fontdict=fontTicks)
        ax[i,j].yaxis.set_major_formatter(mtick.ScalarFormatter())
```

#### Adding a y-axis grid that appears *behind* our data. 
 ```python   
        # Set y-grid
        ax[i, j].set_axisbelow(True)
        ax[i, j].grid(axis='y',ls='--')
```
**and thats it for sub-plot one!**.

#### Subplot 2 repeats much of the same code
The major difference is that the second plot is a sns.regplot, which is made of different componets than the sns.distplot above, and consequently takes in different keyword dictionaries. It takes it one keyword dictionary for the line (line_kws) and another for the scatter plot (scatter_kws).
```python
 ## ----- SUBPLOT 2-----  ##
        i,j = 0,1
        ax[i,j].set_title(column.capitalize(),fontdict=fontTitle)

        # Define the ketword dictionaries for  scatter plot and regression line (subplot 2)
        line_kws={"color":"white","alpha":0.5,"lw":4,"ls":":"}
        scatter_kws={'s': 2, 'alpha': 0.5,'marker':'.','color':'blue'}

        # Plot regplot on ax[i,j] using line_kws and scatter_kws
        sns.regplot(df[column], df[target], 
                    line_kws = line_kws,
                    scatter_kws = scatter_kws,
                    ax=ax[i,j])
        
        # Set x-axis label
        ax[i,j].set_xlabel(column.title(),fontdict=fontAxis)

         # Get x ticks, rotate labels, and return
        xticklab2=ax[i,j].get_xticklabels(which='both')
        ax[i,j].set_xticklabels(labels=xticklab2,fontdict=fontTicks, rotation=45)
        ax[i,j].xaxis.set_major_formatter(mtick.ScalarFormatter())
```

#### Now here is where we will use our price formatting we declared in the beginning:
The only difference here is that we call upon our tickPrice formatter that we defined at the beginning of the function in order to get our y-axis to have $'s , use comma to separate thousands, and to show no decimal places. 
```python
 # Set  y-axis label
        ax[i,j].set_ylabel('Price',fontdict=fontAxis)
        
        # Get, set, and format y-axis Price labels
        yticklab = ax[i,j].get_yticklabels()
        ax[i,j].set_yticklabels(yticklab,fontdict=fontTicks)
				
        ax[i,j].get_yaxis().set_major_formatter(tickPrice) 
```
​We then finish our subplot by defiing the y-axis grid like in subplot 1. 
```python
        # Set y-grid
        ax[i, j].set_axisbelow(True)
        ax[i, j].grid(axis='y',ls='--')       
```

#### Deleting our un-used subplots and using the magic fig.tight_layout() command. 
```python
 ## ---------- Final layout adjustments ----------- ##
        # Deleted unused subplots 
        fig.delaxes(ax[1,1])
        fig.delaxes(ax[1,0])

        # Optimizing spatial layout
        fig.tight_layout()
```

### There we go! Thats it. :-) Now that you've seen how to deal with the curve-balls that seaborn subplots throws at us, you  are now well-equipped to begin produce more complex figures such as my summary figure for King's County house pricing, below.
[You may see both in action by running Summary Figure Alone.ipynb in this data repository.](https://github.com/jirvingphd/dsc-1-final-project-online-ds-ft-021119)

<img src="https://www.dropbox.com/s/gzxn9pq3698vh32/summary_figure.png?raw=1" width="800" height="600" />


