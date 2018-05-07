---
layout: post
title: Getting started with analyze - Retrieving data
tags: [ Data Science, csv, Beginner ]
output:
  pdf_document:
    highlight: tango
    toc: yes
  html_notebook:
    toc: yes
---

In Data Science data preprocessing(data retrieving, cleaning, transforming, etc) is so important. We will start analyze with retrieving our data from csv file and converting it to a list. This is the first touch for analyzing data.

I will use UCI Machine Learning Repository Dataset which you can reach out from the link below.

<a href="https://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset">UCI ML Rep - Bike sharing dataset</a> 

This dataset contains the hourly and daily count of rental bikes between years 2011 and 2012 in Capital bikeshare system with the corresponding weather and seasonal information.

Inculdes these fields:

- instant: record index
- dteday : date
- season : season (1:springer, 2:summer, 3:fall, 4:winter)
- yr : year (0: 2011, 1:2012)
- mnth : month ( 1 to 12)
- hr : hour (0 to 23)
- holiday : weather day is holiday or not (extracted from [Web Link])
- weekday : day of the week
- workingday : if day is neither weekend nor holiday is 1, otherwise is 0.
+ weathersit : 
- 1: Clear, Few clouds, Partly cloudy, Partly cloudy
- 2: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist
- 3: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds
- 4: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog
- temp : Normalized temperature in Celsius. The values are derived via (t-t_min)/(t_max-t_min), t_min=-8, t_max=+39 (only in hourly scale)
- atemp: Normalized feeling temperature in Celsius. The values are derived via (t-t_min)/(t_max-t_min), t_min=-16, t_max=+50 (only in hourly scale)
- hum: Normalized humidity. The values are divided to 100 (max)
- windspeed: Normalized wind speed. The values are divided to 67 (max)
- casual: count of casual users
- registered: count of registered users
- cnt: count of total rental bikes including both casual and registered

Note: If you download dataset to your computer, you can work from your local machine. I also highly recommend you to work with Jupyter Notebook. It provides cell-based and clean work.

First is first, let me read data from csv file.

{% highlight python %}
  # open csv file in read mode
  f = open("Bike-Sharing-Dataset/day.csv", "r") 
  # read csv file into data variable
  data = f.read()
{% endhighlight %}

When you execute your data, you will see output like below.

{% highlight python %}

'instant,dteday,season,yr,mnth,holiday,weekday,workingday,weathersit,temp,atemp,hum,windspeed,casual,registered,

cnt\n1,2011-01-01,1,0,1,0,6,0,2,0.344167,0.363625,0.805833,0.160446,331,654,985\n2,2011-01-02,1,0,1,0,0,0,2,0.363478,

0.353739,0.696087,0.248539,131,670,801\n3,2011-01-03,1,0,1,0,1,1,1,0.196364,0.189405,0.437273,0.248309,120,1229,

1349\n4,2011-01-04,1,0,1,0,2,1,1,0.2,0.212122,0.590435,0.160296,108,1454,1562\n5,2011-01-05,1,0,1,0,3,1,1,0.226957,

0.22927,0.436957,0.1869,82,1518,1600\n6,2011-01-06,1,0,1,0,4,1,1,0.

{% endhighlight %}

As you can see there are newline characters("\n") inside our data. We need to change the display using these new line characters in order to be able to view our data more clean. In fact, our whole goal is to bring our data to the tabular shape to work with columns and rows easily. The magic keyword is 'split'. If you give any character to 'split' function, it breaks up according to that character and gives you back a new list. Let's do it.

{% highlight python %}
  rows = data.split("\n") # this will return a list
{% endhighlight %}

With this line of code we get list type of data. Let's get just 10 rows from it.

{% highlight python %}
  rows[0:11] # except first row
{% endhighlight %}

when you execute, the output will be...

{% highlight python %}
['instant,dteday,season,yr,mnth,holiday,weekday,workingday,weathersit,temp,atemp,hum,windspeed,casual,registered,cnt',
 '1,2011-01-01,1,0,1,0,6,0,2,0.344167,0.363625,0.805833,0.160446,331,654,985',
 '2,2011-01-02,1,0,1,0,0,0,2,0.363478,0.353739,0.696087,0.248539,131,670,801',
 '3,2011-01-03,1,0,1,0,1,1,1,0.196364,0.189405,0.437273,0.248309,120,1229,1349',
 '4,2011-01-04,1,0,1,0,2,1,1,0.2,0.212122,0.590435,0.160296,108,1454,1562',
 '5,2011-01-05,1,0,1,0,3,1,1,0.226957,0.22927,0.436957,0.1869,82,1518,1600',
 '6,2011-01-06,1,0,1,0,4,1,1,0.204348,0.233209,0.518261,0.0895652,88,1518,1606',
 '7,2011-01-07,1,0,1,0,5,1,2,0.196522,0.208839,0.498696,0.168726,148,1362,1510',
 '8,2011-01-08,1,0,1,0,6,0,2,0.165,0.162254,0.535833,0.266804,68,891,959',
 '9,2011-01-09,1,0,1,0,0,0,1,0.138333,0.116175,0.434167,0.36195,54,768,822',
 '10,2011-01-10,1,0,1,0,1,1,1,0.150833,0.150888,0.482917,0.223267,41,1280,1321']
{% endhighlight %}

We're even closer to our goal. :)
Now, we need to get rid of first line(header) of rows data because we will not need it when we play with the data. When we need it, we already have the rows variable above which has header.

{% highlight python %}
  # exclude first row. So slice from index 1 to length of our list. 
  data_with_no_header = rows[1:len(rows)]
{% endhighlight %}

{% highlight python %}
  ['1,2011-01-01,1,0,1,0,6,0,2,0.344167,0.363625,0.805833,0.160446,331,654,985',
 '2,2011-01-02,1,0,1,0,0,0,2,0.363478,0.353739,0.696087,0.248539,131,670,801',
 '3,2011-01-03,1,0,1,0,1,1,1,0.196364,0.189405,0.437273,0.248309,120,1229,1349',
 '4,2011-01-04,1,0,1,0,2,1,1,0.2,0.212122,0.590435,0.160296,108,1454,1562',
 '5,2011-01-05,1,0,1,0,3,1,1,0.226957,0.22927,0.436957,0.1869,82,1518,1600',
 '6,2011-01-06,1,0,1,0,4,1,1,0.204348,0.233209,0.518261,0.0895652,88,1518,1606',
 '7,2011-01-07,1,0,1,0,5,1,2,0.196522,0.208839,0.498696,0.168726,148,1362,1510',
 '8,2011-01-08,1,0,1,0,6,0,2,0.165,0.162254,0.535833,0.266804,68,891,959',
 '9,2011-01-09,1,0,1,0,0,0,1,0.138333,0.116175,0.434167,0.36195,54,768,822',
 '10,2011-01-10,1,0,1,0,1,1,1,0.150833,0.150888,0.482917,0.223267,41,1280,1321',
 '11,2011-01-11,1,0,1,0,2,1,2,0.169091,0.191464,0.686364,0.122132,43,1220,1263']
{% endhighlight %}

We finish this part. Congradulations!!!

Our job was easy because we had clean data. In real life, it's more likely to deal with dirty data. I want to talk about this in another article.

Next article we wil play with the result we obtained.

Good luck!
