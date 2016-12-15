# PreMidterm review Friday 10/21

## These are topics we reviewed o Friday in sight of the midterm.

### The midterms are Tue and Thu in regular class hours. You are allowed to use internet and printed material. You are not allowes to collaborate or ask questions to your classmates. Delivety will be through NYUClasses.
### Make sure your computer has all python packages we have used in class, and that you are able to access compute and jupyterhub as needed, although you are allowed to work on your local machine if you prefer. 

## - Data access

You may be required to access data in any of the way we have accessed data in the class: 
1. By direct url access.
2. Through an API.
3. From the DataFacility. 

You will not be required to access data from databases, of course, since we have not covered that yet in PUI.

### 1. Direct URL access: if you are given the link to a dataset, or directed to find such a link,
may be able to access the data directly :
```
import pandas as pd
c = pd.read_csv("https://raw.githubusercontent.com/cs109/2014_data/master/countries.csv")
print(c)
```
or you may need to download the date (e.g. if it is a zipped csv, like the citibike data). 
You can download data with bash line commands. For example to download the data you can use the 
```
curl
```
or
```
wget
```
commands. You can run those from the python code as 
```
!curl -O http://some.url.here
```
or 
```
url = "http://some.url.here"
os.system("curl -O " + url)
```

In this case you MUST ALSO move the data to a directory pointed to by an environmental variable $PUIDATA. The directory MUST EXIST and BE A DIRECTORY before you move data to it with the command mv. Look at https://github.com/fedhere/PUI2016_fb55/blob/master/BashCommands.md if you have doubts about how to run these commands: cp and mv take 2 arguments, origin (the file you want to move) and destination (what you want to move it to). 

Notice how I used the "!curl" command when I hard-coded the url link in the command (spelled it directly in there), and I used "os.system" instead when the url was generated from a variable. If you know all the pieces of the command you can use "!", otherwise you can use "os.system" passing it as argument the string you want to be executed on the shell. e.g.:

```
os.system("mv data.csv " + os.getenv("PUIDATA"))
```
*If the destination is NOT an **existing** directory the file gets renamed to destination. *

Note also that since you **always** want to refer to the directory where the data is stored using the environmental variable PUIDATA, which is a variable, every time you want to use it you do not have all the pieces of the command: you need to create the command from the variable!

**Recommandations:** before running a command with os.system() print the string you want to pass it to verify you have created it properly! It is easy to miss spaces, or "/" in the construction of the strings! e.g.

```
cmdstr = "mv data.csv " + os.getenv("PUIDATA")
print (cmdstr)
```
then if you like what you see
```
os.system(cmdstr)
```

### 2. API data access

APIs allow us to access data interactively. The trick is to create the link that requires the sepcific data you want. 

Review HW2 where you used the API for MTA and the solution of HW5 
https://github.com/fedhere/PUI2016_fb55/blob/master/HW5_fb55/solutions/citibikes_goodness_of_fit_solution.ipynb
where I *use the google API to **reversegeocode** coordinates and get zip codes out of citibike data.*

Two things to be noticed: if you are asked to use an API you may need an API key. **Never hard-code your API key in the code**. Set an environmental variable (like I did in HW5, and like you did in your first homework) and use that to create the url. E.g.:
```
url = ("https://maps.googleapis.com/maps/api/geocode/json?latlng=" +
           "%f,%f&key=%s"%(
            latlon[0], latlon[1], os.getenv('GOOGLEAPI')))
```
look in detail at the link above: typical the link is static (the same for all requests) up to a "?" character. 
then begins the customization: here I want to get the reversegeocodes coordinates latitude and longitude (i.e. I know the coordinats and want a zipcode). URLs that start with "https://maps.googleapis.com/maps/api/geocode/json?latlng=" are designed for that, then the long and latitude values are the next piece of the url, for example: "https://maps.googleapis.com/maps/api/geocode/json?latlng=40.750020,-73.969053" .
Next (and last) the API wants the key. Variables passed to the API are generally separated by "&", followed by the name of the variable. In this case the name for the API key is "key" so the rest of the url goes: 
"https://maps.googleapis.com/maps/api/geocode/json?latlng=40.750020,-73.969053&key="
and then you pass it your key, which is a variable, so 
```
url = ("https://maps.googleapis.com/maps/api/geocode/json?latlng=" +
           "%f,%f&key=%s"%(
            latlon[0], latlon[1], os.getenv('GOOGLEAPI')))
```
returns 
```
https://maps.googleapis.com/maps/api/geocode/json?latlng=40.750020,-73.969053&key=XXXX-XXXX-XXXX
```
(where I replaced by key with XXXX-XXXX-XXXX because one should &**never share API keys** since if they are used illegally the owner of the key is legally responsible!)

**Important!: API usually limits the access to data to a certein number of requests per minute, or per hour, or per day.** Do not simply put API requests in a for look and just hammer away till you get it right! Do not submit the same request over and over again! If you get shut down for an hour you have lost 1/3 of the midterm time!! For example, in the EC in HW5 I am only asking the zipcode of each station once, by identifying the rides that originate from the same station ahead of time. This frops the number of requests from *hundreds of thousannds to ~100!*


### 3. Data Facility: 
You will not be asked to access data that can only be accessed through the CUSP data facility (DF, https://datahub.cusp.nyu.edu/) since this would force you to work on compute. You can work on compute or on your laptop, as you prefer. You may be asked to access data that also exists on the CUSP data facility, and that should be welcome by you because accessing data from the DF is trivial from compute: all you need to do is find the *path* of the data and read it in as if it were on compute (cause it is on a disk which is mounted on compute). This was dome in HW 2 Assignment 3 (https://github.com/fedhere/PUI2016_fb55/blob/master/HW2_fb55/Assignment3_fb55_solution.ipynb): 

```
DFDATA = "/gws/open/NYCOpenData/nycopendata/data/"
df_gas = pd.read_csv(DFDATA + "/uedp-fegm/1414245967/uedp-fegm")
```


## -  Joining datasets.

Most of the interesting science we can do comes joining different datasets and relate the features of one to those of another, like in HW6 

Look at how pandas allows you to merge, and concatenate datasets. Take a very close look at the merge function! I am almost certain you will have to use it! That requires to have a common column (like the BBL in HW6) in both dataframes. Think about what the option "how" allows you to do: merge to get the *intersection* (only the rows in both dataframes), the *union* (all rows in both dataframes, with NaN where either one has missing values) or *left* and *righ* which import the values of the other dataframe on to the one you are merging on, but do not import rows that are only in the other dataframe.

Remember that most data wrangling techniques enabled by Pandas in python are described in Chapter 7 of Python for Data Analysis and I implemented (painfully) all examples in the chapter in an ipython notebook here https://github.com/fedhere/UInotebooks/blob/master/dataWrangling/PandasDataWrangling-Chap7.ipynb)


## -  Dropping and Reducing when possible! 

Do not carry around more data than you need! Do not create a dataframe every time you want to change a value! Try to be conservative with the amount of studd that is occupying memory in your code and in your computer. Replace values when possible (and when sure you won't need the old value), rename columns instead of creating duplicated, use the method .map ro .apply to change the values in columns. 
**Specifically, and particularly if you are under time pressure, it is a great idea to develope your code on a subset of the data** then rn t on the full dataset only at the end.

*If your kernel crashes there is 99% probability that you re doing something wrong or something unnecessary! Nothing that you will be asked to do requires sophisticated computing facilities.*


## -  Hypothesis testing 

### Remember all the rules in place for hypothesis testing: particularly 
1. set your significance level at the beginning of the work (alpha = ...)
2. State the Null and alternative hypothesis in words and in mathematical fornulae as well.
3. Make sure you are reading the appropriate table if you want to verify if the hypothesis can or cannot be rejected
4. Make sure you read the documentation of the python packages you are using and you know how to read the output of a sttistical test (in terms of p-value).



## -  Figures:
Every figure must have axis labels, title, and a Caption. It would be silly to lose points for this!! The caption should describe the plot first so that the readers know what they are reading, and then giudes the reader to the conclusions to be drawn from the plot. (This is less standard in journals and reports, but I require you to do it to make sure you make an effor to iterpret the visualizations you create, and that you only have meaningful figures in your work).

Look for example here (I just picked a figure among millions which has a good descriptive graphics): http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0165318

## -  General Presentation
Write in full sentences, in your best English. Describe what the aim of the project at thge beginning, as if the grders did not know that. Describe what your code does block by block, with a dedicated cell or a comment before a line or set of lines of code. Discuss what conclusions can be made, and what are the weaknesses of the analysis you are asked to do. Improve the analysis if possible! 

You are not bound to PEP8 requirements but you are bound to reasonable coding behavior!! The basic PEP8 rules are here if you wish to make an effort to adhere. They actually will help you coding better. https://github.com/fedhere/PUI2016_fb55/blob/master/PEP8MinimalRequirements.md

