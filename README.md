# Python-stuff

## Function to show distribution of numeric variable

* Histogram
* Boxplot
* Statistical summary (mean, median, min, max)

```
def show_distribution(var_data):
    '''
    This function will make a distribution (graph) and display it
    '''

    # Get statistics
    min_val = var_data.min()
    max_val = var_data.max()
    mean_val = var_data.mean()
    med_val = var_data.median()
    mod_val = var_data.mode()[0]

    print('Minimum:{:.2f}\nMean:{:.2f}\nMedian:{:.2f}\nMode:{:.2f}\nMaximum:{:.2f}\n'.format(min_val,
                                                                                            mean_val,
                                                                                            med_val,
                                                                                            mod_val,
                                                                                            max_val))

    # Create a figure for 2 subplots (2 rows, 1 column)
    fig, ax = plt.subplots(2, 1, figsize = (10,4))

    # Plot the histogram   
    ax[0].hist(var_data)
    ax[0].set_ylabel('Frequency')

    # Add lines for the mean, median, and mode
    ax[0].axvline(x=min_val, color = 'gray', linestyle='dashed', linewidth = 2)
    ax[0].axvline(x=mean_val, color = 'cyan', linestyle='dashed', linewidth = 2)
    ax[0].axvline(x=med_val, color = 'red', linestyle='dashed', linewidth = 2)
    ax[0].axvline(x=mod_val, color = 'yellow', linestyle='dashed', linewidth = 2)
    ax[0].axvline(x=max_val, color = 'gray', linestyle='dashed', linewidth = 2)

    # Plot the boxplot   
    ax[1].boxplot(var_data, vert=False)
    ax[1].set_xlabel('Value')

    # Add a title to the Figure
    fig.suptitle('Data Distribution')

    # Show the figure
    fig.show()


show_distribution(df_students['Grade'])
```

## Function to show density of numerical variable

```
def show_density(var_data):
    fig = plt.figure(figsize=(10,4))

    # Plot density
    var_data.plot.density()

    # Add titles and labels
    plt.title('Data Density')

    # Show the mean, median, and mode
    plt.axvline(x=var_data.mean(), color = 'cyan', linestyle='dashed', linewidth = 2)
    plt.axvline(x=var_data.median(), color = 'red', linestyle='dashed', linewidth = 2)
    plt.axvline(x=var_data.mode()[0], color = 'yellow', linestyle='dashed', linewidth = 2)

    # Show the figure
    plt.show()

# Get the density of StudyHours
show_density(df_students['Grade'])
```

## Convert 2 List into a dictionary

```
a = ['GSW', 'LAKERS', 'CLIPPERS']
b = [1, 2, 3]
dict(zip(a, b))
```
Result: {‘CLIPPERS’: 3, ‘GSW’: 1, ‘LAKERS’: 2}

## Flatten a list

```
flst = [[1,2,3],[8,9,12,17],[3,7]]
sorted(sum(flst,[]))
```
Result: [1, 2, 3, 3, 7, 8, 9, 12, 17]

## Saving a ML model to a pickle file

```
#Save the model using pickle
import pickle
# save the model to disk
pickle.dump(model, open(model_file_path, 'wb'))

#Load the model 
model = pickle.load(open(model_file_path, 'rb'))

#Saving a Keras model
# Calling `save('my_model')` creates a SavedModel folder `my_model`.
model.save("my_model")
#Load a Keras Model
# It can be used to reconstruct the model identically.
reconstructed_model = keras.models.load_model("my_model")
```

## Splitting string using a character

```
# import Pandas as pd 
df = pd.DataFrame({'Location': ['Cupertino,California', 'Los Angles, California', 'Palo Alto, California']
                   })   
df[['City','State']] = df.Location.str.split(',',expand=True) 
```

## Reverse a string
```
string = [1, 2, 3,5,8,9]
string[::-1]
```

## Display data in data.table format in Google Colab
```
from google.colab import data_table
from vega_datasets import data
data_table.enable_dataframe_formatter()
data.airports()
```

## Functions
* pandas.melt() = dataframe wide to long
* str.casefold
* str.upper
* str.lower
* str.capitalize
* str.title
* str.swapcase


## Packages
* Faker: generating fake data and anonymizing data
*  
