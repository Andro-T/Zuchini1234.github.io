# Time Share? No Thanks

<img src='https://github.com/Andro-T/dsc-phase-4-project/blob/main/images/Oahu.jpg'>

For my penultimate project with Flatiron, I chose to work with Time Series, specifically, finding Zip Codes in Hawaii that are likely to have the best Return on Investment. The business case is that I am working for an Investment Consultancy firm focused on Real Estate. We have been approached by a buyer looking for property in Hawaii as an investment. Absolute value of the property is not as important as the potential return it might offer. To this end, we focused on the relative return metric Return on Investment (ROI). ROI is used to measure the profitability of an event or investment and works on a relative basis (Amount Gained minus Amount Spent) divided by Amount Spent.

To perform this operation I used Time Series models to predict the potential returns of different zip codes. This turned out to be significantly more challenging than expected, reflecting the stock markets sentiment I assumed prices could only ever go up. The reality turned out to be much more nuanced. 

**Breakind Down Average Property Values**

<img src='https://github.com/Andro-T/dsc-phase-4-project/blob/main/images/boxplot.png'>




### Recommendation

Having created a model for every Zip Code, these are the recommendations we ended with: both those that buyers should consider, and those to avoid: 

Best Four Zipcodes:
1. 96771 - Mean ROI: 35.9%	Bear Case: -15.45%	Bull Case: 87.25%
2. 96763 - Mean ROI: 31.1%	Bear Case: 3.12%	Bull Case: 59.08%
3. 96772 - Mean ROI: 28.65%	Bear Case: 1.33%	Bull Case: 55.97%
4. 96743 - Mean ROI: 21.11%	Bear Case: 0.77%	Bull Case: 41.46%

Worst Four Zipcodes:
1. 96760 - Mean ROI: -18.33%	Bear Case: -61.3%	Bull Case: 24.64%
2. 96779 - Mean ROI: -5.44%	Bear Case: -35.73%	Bull Case: 24.86%
3. 96785 - Mean ROI: -4.63%	Bear Case: -47.13%	Bull Case: 37.87%
4. 96752 - Mean ROI: -0.24%	Bear Case: -22.13%	Bull Case: 21.64%


The first choice (96771) represents a high risk high reward opportunity, with the significantly highest Bull Case ROI but with a potential loss of value. The second choice, 96763, represents a significant return while being less likely to lose money. 

**Potential Additional Work**

If I had more time these are some things I would’ve liked to incorporate into the analysis:

Inflation: The property values in the chart are not inflation adjusted, this means that the properties further down the line are not as valuable relative to older dates. Adding this element would show the real value of owning property relative to the economy. 
Facebook’s Prophet Library: I was not able to install this library (which may have been because Prophet is only supported for Linux and Apple now), but I think it would have been a very powerful tool to use to predict future values for our dataset. 
Calculating values for every state! The model, in theory, should allow us to break down data for every zip code so that we can find the best potential ROI across the whole country!

If you want to take a look at the jupyter notebook containing the code referenced in this blog please check out: https://github.com/Andro-T/dsc-phase-4-project.
