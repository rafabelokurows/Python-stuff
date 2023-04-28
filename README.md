# Python-stuff

## Bypassing SSL certificate verification
Wrap the function with error in this:
```
with no_ssl_verification():
    bp = geolocator.geocode("Balboa Park, San Diego, US")
```
```
import warnings
import contextlib

import requests
from urllib3.exceptions import InsecureRequestWarning

old_merge_environment_settings = requests.Session.merge_environment_settings

@contextlib.contextmanager
def no_ssl_verification():
    opened_adapters = set()

    def merge_environment_settings(self, url, proxies, stream, verify, cert):
        # Verification happens only once per connection so we need to close
        # all the opened adapters once we're done. Otherwise, the effects of
        # verify=False persist beyond the end of this context manager.
        opened_adapters.add(self.get_adapter(url))

        settings = old_merge_environment_settings(self, url, proxies, stream, verify, cert)
        settings['verify'] = False

        return settings

    requests.Session.merge_environment_settings = merge_environment_settings

    try:
        with warnings.catch_warnings():
            warnings.simplefilter('ignore', InsecureRequestWarning)
            yield
    finally:
        requests.Session.merge_environment_settings = old_merge_environment_settings

        for adapter in opened_adapters:
            try:
                adapter.close()
            except:
                pass

```


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
string = [1,2,3,5,8,9]
string[::-1]
```

## Compare two DFs
[<img src="https://media-exp1.licdn.com/dms/image/C4E22AQFMthtTMSqclw/feedshare-shrink_1280/0/1663873171762?e=1666828800&v=beta&t=QbmZERwbNCC7-Gd9g4Z3rHw0mcMzWoUBCY4IhQYBo8A" width="350" height="750">](https://www.linkedin.com/posts/jessramosmsba_python-programming-data-activity-6978882593601585152-gzrg?utm_source=share&utm_medium=member_desktop)

## Display data in data.table format in Google Colab
```
from google.colab import data_table
from vega_datasets import data
data_table.enable_dataframe_formatter()
data.airports()
```

## Creating column based on mapping another column
```
monthmap = {
    'Jan':1,
    'Feb':2,
    'Mar':3,
    'Apr':4,
    'May':5,
    'Jun':6,
    'Jul':7,
    'Aug':8,
    'Sep':9,
    'Oct':10,
    'Nov':11,
    'Dec':12,
}
df['MonthNum'] = df['Month'].apply(lambda x: monthmap[x])
```

## Converting data from one format to another 
```
from datetime import datetime
date='2019-Jan-01'
date_object = datetime.strptime(date, "%Y-%b-%d")
date_object
```

## Iterating through columns showing number of uniques and unique values
```
for col in df.columns:
  print(col, len(df[col].unique()),df[col].unique())
```

## Descrevendo DF incluindo colunas object
```
df.describe(include="object")
```

## Error handling - Try -> Except
```
new_set = {1,2,3,4,5,6}
try:
  print(new_set)
  new_set[0] = 4
except Exception as e:
  print('Deu merda:\n',e)
```

## Writing file
```
with open('teste123.txt','w') as f:
  f.write('Testando para ver como fica o ficheiro que estou gravando \n Teste')
```

## Reading file
```
with open('teste.txt','r') as f:
  file = f.read()
```

## Writing file in Colab
```
%%writefile teste.txt
bla bla bla
```

## Writing Python Script
```
%%writefile teste.py
def launch_codes():
  return 12345
```

## Reading Python Script
```
from google.colab import files
files.view('teste.py')
%run teste.py
```



## API Request
```
header = {'Accept':'application/json'}
teste = requests.get('https://icanhazdadjoke.com',headers=header)
json_result = teste.json()
json_result['joke']
```

# Functions
* pandas.melt() = dataframe wide to long
* str.casefold
* str.upper
* str.lower
* str.capitalize
* str.title
* str.swapcase


# Packages
* Faker: generating fake data and anonymizing data
*  
