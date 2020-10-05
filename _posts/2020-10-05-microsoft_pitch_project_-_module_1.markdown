---
layout: post
title:      "Microsoft Pitch Project - Module 1"
date:       2020-10-05 00:33:49 -0400
permalink:  microsoft_pitch_project_-_module_1
---


I have just pushed the last part of my Module 1 Project. I am, by and large, pleased with the outcome. There are a few points I wish I had pursued further but I went into this with a focus on quality over quantity. More importantly, I must say that I thoroughly enjoyed this assignment. Every time I solved a problem or made a cool graph, I couldn't help but smile and do a round of fist bumping. I did my best to gain some real insight into the Film industry and to make the whole process as visually appealing as possible. To that end, I would like to go through some of the most important parts of this Project, for me. 


### Brief Overview
This project was centred on a hypothetical pitch to Microsoft, identifying the best road on which to join the film industry. The answers were to come from cleaning, shaping and then analysing data against set questions and benchmarks. All this was intended to culminate in recommendations for Microsoft. 
One of the things I knew I wanted to focus on before I wrote a single line of code was relative return. Absolute returns are irrelevant in so many scenarios, a film can make hundreds of millions and still have lost money (see the new X-Men). It is about the ratio of investment to return that shows real value. If you could invest $20,000 in a movie and gain more than a 500,000% return that would be a better choice than making a 100% return on a $100 million movie. Those first two numbers are the real return of the first Paranormal Activity! Keep that in mind when we get to Question 3.

There were two business statements that formed the philosophy of my approach to this research:

1. The first of its kind for any development will incur specific sunk costs, such as hiring new staff, doing research into the field that is being entered, creating new business relationships, marketing, and a million other things. This means that entering a new industry will be more expensive than just the budget of a single movie. 
2. A measurement of Gross return is largely irrelevant; it is the ratio of the revenue to the costs incurred that indicates financial success.

Based on the above, I have focused on measuring success with Return on Investment (ROI). This means that low budget movies (which often have the highest ROI's) will not be lost under blockbuster movies that cost a fortune to make.

##### ROI = (Total Revenue - Initial Investment) / Initial Investment


### Questions and their bottom-line findings 

1. Is there correlation between IMDb Ratings and Return on Investment? Should we be concerned with receiving critical acclaim?

There was negligible correlation between IMDb Ratings ROI. The focus of this project should be the profits generated and the factors that lead to that rather than critical acclaim. 


2. Which Studio should we partner with? This question was calculated with consideration of both a studio's past success and its production numbers. For a first-time entrance, it is important the potential studio partners have the availability to take on new projects as well as have a history of high returns. 

Universal Studios proved to be the best potential studio partner, making solid returns across a large number of films.


3. Which Genre is the most likely to be profitable? This question was also set against different budget thresholds. 

With low budget films the answer is clearly Horror/Mystery/Thriller. They outperform everything else on the board by nearly 1000%. The main reason for this is that a lot of movies in the horror and mystery genre have low production costs, making it easier to outperform in terms of ROI.  It becomes less differentiated for higher budget productions: the best pick for movies with a budget above $60 million appears to be Musicals followed closely by Animation.

Overall, low budget horror mystery movies made in partnership with Universal seem the best place to start. They offer astounding ROI figures and are resilient even in slightly higher budget groups. Despite my personal distaste for anything scary this seems the most prudent decision for Microsoft. 

Now, time for something I am more excited about:

### Code Outtakes 

```productionbudgetdf = genredf.sortvalues('productionbudget')
productionbudgetdf['genres'] = productionbudgetdf['genres'].map(lambda x: x.split(","))
```

The first part of this cell was loading a previously altered dataframe ordered by budget to allow me to seperate the table into four quartiles with just the length of the datafram. I also split each 'genre' on ',' to create a list.  

```length = len(productionbudgetdf)
q0 = productionbudgetdf['productionbudget'].min()
q1 = productionbudgetdf['productionbudget'][:int(length/4)].max()
q2 = productionbudgetdf['productionbudget'][int(length/4):int(2length/4)].max()
q3 = productionbudgetdf['productionbudget'][int(2length/4):int(3length/4)].max()
q4 = productionbudgetdf['productionbudget'].max()*
```

This part identified the thresholds of each quarter of the dataframe. These became the boundaries of Low, Medium-Low, Medium-High, and High Budget movies.

```df1 = productionbudgetdf[:int(length/4)]
ef1 = df1.explode('genres')
df2 = productionbudgetdf[int(length/4):int(2length/4)]
ef2 = df2.explode('genres')
df3 = productionbudgetdf[int(2length/4):int(3length/4)]
ef3 = df3.explode('genres')
df4 = productionbudgetdf[int(3length/4):]
ef4 = df4.explode('genres')
```

To separate each genre into its own row I used pd.explode to create a row for each genre in a movie. This added significantly to the length of the table but made the graphing later on much easier. Each number is for one of the quarters of budget.

```a1 = []
for i in df1['genres']:
    a1 += i
a1 = set(a1)
a2 = []
for i in df2['genres']:
    a2 += i
a2 = set(a2)
a3 = []
for i in df3['genres']:
    a3 += i
a3 = set(a3)
a4 = []
for i in df4['genres']:
    a4 += i
a4 = set(a4)
```

Created a list of the unique genres in each quarter.

```b1 = []
for i in a1:
    b1.append(ef1.loc[ef1['genres'] == i]['roi%'].mean())
b2 = []
for i in a2:
    b2.append(ef2.loc[ef2['genres'] == i]['roi%'].mean())
b3 = []
for i in a3:
    b3.append(ef3.loc[ef3['genres'] == i]['roi%'].mean())
b4 = []
for i in a4:
    b4.append(ef4.loc[ef4['genres'] == i]['roi%'].mean())
		```
		
	The above created a list to match the unique genre list generated with a1 - a4.  Now that I have all this data I can create a new dataframe containing both the budget, average roi, and genre. 

```c1 = pd.DataFrame({
    'Genre' : [i for i in a1],
    'Budget' : "budget < 10000000",
    'AverageROI' : [i for i in b1]
})
c2 = pd.DataFrame({
    'Genre' : [i for i in a2],
    'Budget' : "10000000 < budget < 25000000",
    'AverageROI' : [i for i in b2]
})
c3 = pd.DataFrame({
    'Genre' : [i for i in a3],
    'Budget' : "25000000 < budget < 60000000",
    'AverageROI' : [i for i in b3]
})
c4 = pd.DataFrame({
    'Genre' : [i for i in a4],
    'Budget' : "budget > 60000000",
    'AverageROI' : [i for i in b4]
})
```

Finally, I merged all of them into one table that I was able to graph with relative ease using seborn, merely setting data to equal d. 

```d = c1.merge(c2, how = 'outer')
d = d.merge(c3, how = 'outer')
d = d.merge(c4, how = 'outer')
```

This code is not overly complicated, but it took a lot of effort and creativity on my part to figure out a method to use the seaborn 'hue' operator. It is also, probably, the mots organized code I have written ever. It followed a prescribed structure and I never once forgot what I had named something. I did not use for loops to avoid nesting but I do not think it would have been much easier anyway. 

### I leave you with this

If I knew how to include visuals onto this page I certainly would, certainly taking the time to critique one of them. Overall, however, I must say I have thoroughly enjoyed this project despite being up till very early in the morning. This has kindled a certain passion for data science within me, in a way that is so pleasantly surprising. 
