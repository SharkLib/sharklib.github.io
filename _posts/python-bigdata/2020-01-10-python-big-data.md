---
date: 2020-01-01 11:48:05
layout: post
title: Python and big data
subtitle: Learn big data python
description: >-
  A very useful collection for learn Python&Spyder in Chinese.
image: >-
  /assets/img/bigdata.jpeg
optimized_image: >-
  /assets/img/bigdata.jpeg
category: Python
tags:
  - blog
  - Python
author: Ron
paginate: true
---

 ## Python and spider

   Excel is a popular and powerful spreadsheet.Pandas module allows your Python programs to read and modify Excel spreadsheet files, and do data science research.
   Here is a smaple to use Pandas to deal with data efficiently.
```
  #import the pandas to env.
  import pandas as pd
  #get thie excel file and the worksheet name.
  path = input("Enter file name : ") 
  st_name = input("Enter sheet name : ") 
  print(path, st_name)
  #open file and read the worksheet.
  df =  pd.read_excel(path,sheet_name = st_name,index=False)
  #list some sample
  print(df.head())
  #list all columns
  print(df.keys())
  #preview new excel container  
  print (df.shape)
  row , col = df1.shape
  print(row,col)

  #get the key words from one column
  col_name = input("Enter column name : ") 
  print(df[col_name].unique())

  #get cell content is blank worksheet and cell content is not blank worksheet.
  col_name = input("Enter blank column name : ") 
  dfs = df[ (df[col_name].astype(str) == '')]
  df_blank = df[(df[col_name].astype(str)  !='')]
```
  ## sample to do big data science.
  Let's image we have a excel like:<br>
<img src="../image/salary.jpg"><br>
and we want to get all avery salary for each position. How could we do?
<br>Here is the sample.
```
  import pandas as pd
  df =  pd.read_excel(path,sheet_name = st_name,index=False)
  positions = df['Position'].unique()
  print(positions)
  for p in positions:
    df_p = df[df['Position'] == p]
    print (df.mean(axis = 1) )
    print(p, df["Salary"].mean())
```

