---
layout: page
permalink: /ref/coding/data/
title: data
description: Intro to Data Analysis
nav: false
toc: true
---
Data analysis involves collecting, visualizing, and analyzing the data. Below are tools that I have tried out. There are obviously many more options (ie google data studio) these are just the tools I've found easy to learn and most useful.  

# Measuring Data
Data collection can be made through sensors or other readings  
 ADC, ina219, temp, encoder (motor rpm), etc

# Storing Data
​Data can then be stored in a text (csv) file or sent to a db  
influxdb (time series for monitoring measurements over time)  
mysql (relation based) Can be used to collect a set of measurements over varying conditions.  
Text, csv, excel file  
* For time series data I usually use node-red (with its charting options) connected to influxdb
* For experimental data I read a csv using Jupyter Notebook or dash plotted by plotly  

## mysql

* ```$ sudo apt-get install mysql-server```  
* ```$ sudo mysql_secure_installation``` utility  
(options to allow for remote access on port 3306)  
* ```$ sudo ufw allow mysql```  
* ```$ sudo systemctl start mysql```  
* ```$ sudo systemctl enable mysql```  
Config file is at /etc/mysql/mysql.conf.d/mysqld.cnf  

The first time you login you likely will get "MySQL ERROR"  
Run as sudo  
* ```$ sudo mysql```  
Then run alter command at mysql prompt  
```sql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';
```
CTRL-D to exit  
Then can get into mysql with normal command (with user/password)  
* ```$ mysql -u root -p```  

To import a csv (can also save a xls file as csv)  
Create a database and empty table with fields then import with the LOAD command.  
--local-infile may be needed  
* ```$ mysql -u root -p --local-infile```  
```sql
mysql> CREATE DATABASE testdb;
mysql> USE testdb;
```
Create the table/define schema using options below  
```sql
mysql> CREATE TABLE mytable (
field1 INT NOT NULL PRIMARY KEY,
field2 VARCHAR(20),
field3 DATE,
field4 FLOAT);
```
Now import the data with LOAD  
```sql 
mysql> LOAD DATA LOCAL INFILE "/path/to/data.csv" INTO TABLE testdb.mytable FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' IGNORE 1 LINES (field1, field2, @field3, field4) set field3 = NOW();
```

To verify  
```sql
mysql> SELECT * FROM mytable ORDER BY field1;
```
or  
```sql
mysql> SELECT * FROM mytable limit 10;
```

# ​Data Visualization Tools  
**Basic tools**  
* Node-red 
    * can store/retrieve data from a db like influxdb
    * has basic line graphs and charts for monitoring data over time. 
    * options to read/write text files, too  
* Grafana has more advanced charting features. 
    * Data from the influxdb can be accessed thru grafana (geared for time series data)

**Advanced tools**
* Jupyter notebook with pandas, numpy, matplotlib, seaborn. 
* Power BI (microsoft windows based). Can import data from databases, csv, JSON, etc and do basic plotting with microsoft tools. Can add R/Python scripts to do more advanced boxplots, historgrams, heatmaps, etc with plotly. Only supports importing from pandas (df). Plotly is currently not supported in Power BI Python but may be useable in the R script.
* dash. Python based dashboard using plotly to do advanced boxplots, historgrams, heatmaps.
* streamlit. Quick and clean data visualizations.

For time series data I usually use node-red (with its charting options) connected to influxdb
For experimental data I read a csv using Jupyter Notebook or dash plotted by plotly

## Jupyter Notebook
A Jupyter Notebook flow
```python
from IPython.display import display 
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns

df = pd.read_csv("data.csv")

pivoted = pd.pivot_table(df, values='data', index=['file', 'sample', 'sensor'], columns = ['type']).reset_index()

pivoted.groupby(['file']).mean().round(1).drop(['sensor', 'ave', 'delta', 'raw'], axis=1)

pivoted.groupby(['file']).describe().round(1).drop(['sensor', 'delta', 'raw', 'ave'], axis=1)

# Use melt to unpivot
comparedf = pd.melt(pivoted, id_vars=['file', 'sample', 'sensor', 'delta', 'time'], var_name='stat')

a = sns.catplot(x='stat', y='value', col='file', data=comparedf, kind='box', ci='sd')
plt.show()

b = sns.catplot(x='stat', y='value', data=comparedf, hue='sensor', col='file', kind='strip')

# More seaborn options
fig,axs = plt.subplots(1,2, sharex=False, sharey=False, squeeze=True)
g = sns.boxplot(x='file', y='time', data=pivoted, ax=axs[0])
#g.set(ylim=(1500, 2250)) g.set_title('ADC Time') g.set_ylabel('time (ms)')
#plt.title('ADC Average')
#plt.ylim(1500, 2250)
#h = sns.boxplot(x='file', y='raw', data=pivoted, ax=axs[1])
#h.set_title('ADC Raw') #plt.title('ADC Raw')
#plt.ylim(1500, 2250)
#h.set(ylim=(1500, 2250))
#h.set_ylabel('raw')
plt.show()

# Histogram

pi0df = pivoted[pivoted['file'] == 'pi0']
esp32df = pivoted[pivoted['file'] == 'esp32']
sns.distplot(pi0df[pi0df['sensor'] == 0].raw, label='pi0-ch0')
sns.distplot(pi0df[pi0df['sensor'] == 1].raw, label='pi0-ch1') sns.distplot(esp32df[esp32df['sensor'] == 0].raw, label='esp32-ch0') sns.distplot(esp32df[esp32df['sensor'] == 1].raw, label='esp32-ch1')
#fig.legend(labels=['pi0-ch0','pi0-ch1','esp32-ch0','exp32-ch1'])
plt.legend()
plt.show()

​# Seaborn categorical plot. kind="box"
i = sns.catplot(x="file", y="time", data=pivoted, ci='sd', kind="box")
```

## Dash - plotly
Dash gives more GUI type functions letting a user toggle fields on/off while plotting.  
```$ python3 -m pip install dash```  

You will create a file called app.py that contains the dash/plotly code.  
When you run  
```$ python3 app.py```  
It will create a link to view the charts in your browser  

**Primary components**  
(initial chart can be plotted with steps 1-3 alone)  
1. Load css and create dash object
2. Define figures (charts. ie fig = px.histogram(df, x="values", nbins=1 ), optional text string (markdown), functions(ie to show the data/df)
3. Layout (see below.. will use html and core components)
4. Callbacks (not required but can be used for more interactive charts)

**More details**  
* Layout is a hierarchical tree (dash_html_components)
* Use html components (classes for HTML tags) to create the layout (ie html.DIV, style, etc). 
* To place charts use dash_core_components:  dcc.Graph(figure=fig)

**Callbacks**  
Callbacks allow for input/output. Makes the charts dynamic and gives you more options for control in the web interface (from dash.dependencies import Input, Output)
```python
@app.callback(Output(output_args), Input(input_args))
def generateChart(x,y):
   fig = px.box(df, x=x, y=y)
   return fig
```

--------------------------------

Packages to import  
```python
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
import plotly.express as px
import pandas as pd
```

-----------------------------  
-----------------------------  

(../../../ref/coding/starting-up/)

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row justify-content-center float-right">
    <div class="col-4-auto mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

----------------------------
Images
can you col-#  col-sm-#   col-md-#   col-lg-#
Use auto to auto size around image
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).