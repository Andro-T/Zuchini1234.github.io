# Time Share? No Thanks

<img src='https://github.com/Andro-T/dsc-phase-4-project/blob/main/images/Oahu.jpg'>

For my penultimate project with Flatiron, I chose to work with Time Series, specifically, finding Zip Codes in Hawaii that are likely to have the best Return on Investment. The business case is that I am working for an Investment Consultancy firm focused on Real Estate. We have been approached by a buyer looking for property in Hawaii as an investment. Absolute value of the property is not as important as the potential return it might offer. To this end, we focused on the relative return metric Return on Investment (ROI). ROI is used to measure the profitability of an event or investment and works on a relative basis (Amount Gained minus Amount Spent) divided by Amount Spent.

To perform this operation I used Time Series models to predict the potential returns of different zip codes. This turned out to be significantly more challenging than expected, reflecting the stock markets sentiment I assumed prices could only ever go up. The reality turned out to be much more nuanced. 

**Breakind Down Average Property Values**

<img src='https://github.com/Andro-T/dsc-phase-4-project/blob/main/images/boxplot.png'>

###The Main Functions Used  for Finding ROI

**Function 1** - Creating a dict of all the zipcodes and their subset dataframe.

'''HI_dict = {}
for zipc in hi_zips.columns:
    HI_dict[zipc] = hi_zips[zipc]
p = d = q = range(0, 2)
pdq = list(itertools.product(p, d, q))
seasonal_pdq = [(x[0], x[1], x[2], 12) for x in list(itertools.product(p, d, q))]'''

**Function 2** - The second set of functions finds the AIC score for every parameter combination for each of the dataframes in HI_dict above. 
'''
AIC = []
for zipcode in HI_dict.keys():
    for param in pdq:
        for param_seasonal in seasonal_pdq:
            try:
                mod = sm.tsa.statespace.SARIMAX(HI_dict[zipcode], order=param, 
                                                seasonal_order=param_seasonal, enforce_stationarity=False, 
                                                enforce_invertibility=False)
                results = mod.fit()
                AIC.append([zipcode, param, param_seasonal, np.abs(results.aic)])
            except:
                continue'''

**Function 3** - Turn the list output from Function Number 2 into a dataframe so we can plug it into the next function. 
'''
AIC_df = pd.DataFrame(AIC, columns = ["zip","pdq", "pdqs", 'aic'])

AIC_df.sort_values('aic', ascending = True).groupby('zip')

AIC_dict = {}
for i, g in AIC_df.sort_values('aic', ascending = True).groupby('zip'):
    AIC_dict[i] = g
    AIC_dict[i] = AIC_dict[i].iloc[0]'''

**Function 4** - Return a dataframe containing the ROI’s for each zipcode. 

'''def find_roi(ts, params, train_size=0.8):
    ROI_df = pd.DataFrame(columns=['zipcode', 'mean_roi', 'lower_roi', 'upper_roi', 'std_roi'])
    
    for zipc in ts.columns:

        # Train test split
        split_idx = round(len(ts[zipc].dropna()) * train_size)
        
        # Split
        train = ts[zipc].dropna().iloc[:split_idx]
        test = ts[zipc].dropna().iloc[split_idx:]
        
        #Best model using params input
        best_model = tsa.SARIMAX(train,
                         order = params[zipc].pdq,
                         seasonal_order = params[zipc].pdqs, 
                      maxiter=500, enforce_invertibility=False).fit()
        
        # Forecast Test Data
        forecast = best_model.get_forecast(steps=len(test))
        pred_df_test = pd.DataFrame([forecast.conf_int().iloc[:, 0], forecast.conf_int().iloc[:, 1], forecast.predicted_mean]).T
        pred_df_test.columns = ["lower", 'upper', 'prediction']
        
        # Future Best Model
        best_model_future = tsa.SARIMAX(ts[zipc],
                                 order = params[zipc].pdq,
                                 seasonal_order = params[zipc].pdqs, 
                                 maxiter=500, enforce_invertibility=False).fit()
        forecast = best_model_future.get_forecast(steps=24)
        pred_df = pd.DataFrame([forecast.conf_int().iloc[:, 0], forecast.conf_int().iloc[:, 1], forecast.predicted_mean]).T
        pred_df.columns = ["lower", 'upper', 'prediction']
        
        # Calculate ROI
        mean_roi = (pred_df[-1:]['prediction'][0] - test[-1:][0])/test[-1:][0]
        lower_roi = (pred_df[-1:]['lower'][0] - test[-1:][0])/test[-1:][0]
        upper_roi = (pred_df[-1:]['upper'][0] - test[-1:][0])/test[-1:][0]
        std_roi = np.std([lower_roi, upper_roi])
        
        temp_dict = {
            'zipcode': str(zipc),
            'mean_roi': mean_roi,
            'lower_roi': lower_roi,
            'upper_roi': upper_roi,
            'std_roi': std_roi}
        
        ROI_df = ROI_df.append(temp_dict, ignore_index = True)
        
    return ROI_df'''


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
