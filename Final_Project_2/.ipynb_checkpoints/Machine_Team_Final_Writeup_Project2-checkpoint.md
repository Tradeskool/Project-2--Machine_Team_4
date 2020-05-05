

# **Analysis of online shoppers' intention: What drives potential customers’ 'conversion'?**
## BA545 Final Project for Machine Team 4
#### Brian Nicholls, Dawn W. Massey & Michael DiSanto

<img src="https://i.ytimg.com/vi/CRKn-9gVNBw/maxresdefault.jpg" width=50%/>

**Note:** This work was completed using the CRISP-DM Framework shown above; accordingly, it will serve as an organizing framework for this report.

<br>

## Part I: Business Issue Understanding
> **Industry/Company/Products:** Columbia Sportswear Company <br>
  **Geography:** Turkey 

<br>


**Research Question:** Overall, this project's research question is: What drives potential customers’ 'conversion'? <br>
<br>

**Background:** 
- Online shopping is an important revenue source for many retail businesses, such as our client
- According to Sakar et al. (2019), desipte increases in e-commerce traffic in the recent past, 'conversion' (of browsers to purchasers) has not increased proportionately
- Global sales conversion rate = ~12.7%
- Our sales conversion rate = 15.5%

**Motivation:** Conversion increases are impactful: +10% conversions --> +\$150,000 in new revenue
- 12,330 customers in 10 months --> ~41 customers/day
- An increase of 10% in conversions --> ~4.1 customers/day 
- At ~365 internet selling days/year --> ~1,500 sales/year
- At ~100 dolllars/sale --> \$150,000 increase in revenue

**What else do we know?**
<br>    - 50% of Turkey’s population is under 30
<br>    - Turkish people, on average, spend 30 hours a week online


<br>

## Part II: Data Understanding & Exploratory Data Analysis

#### Overview of Dataset: [Online Shoppers' Intention Data](./online_shoppers_intention.csv)
**Time Period:** <br>
- Data collection is between 2004 and July 2017 <br>
- Data from 10 months only  -- January/April excluded


**Data source:** Online shopper browsing session data from unique visitors <br>

**Features:** <br>
   - Continuous Features - Metrics from Google Analytics <br>
   - Categorical Features - Data from URL information

The dataset that has been gathered for purposes of this analysis contains 12,330 unique visitors and 18 variables (features): Revenue, which is the target variable (where Revenue = TRUE if the customer visiting the website made a purchase - i.e., Class 1; and Revenue = FALSE if the customer visiting the website did not make a purchase - i.e., Class 0); and 17 predictor variables, including 10 continuous features and 7 categorical features, each of which are listed below and then delineated within our [Data Dictionary.](./images/Dictionary.PNG) <br>

_**Note:** See the data profile report here..._ [link.](./Customer_Intentions_Profile.html)

#### Questions revealed via EDA:

**Do BounceRates v. ExitRates influence No Sale/Sale the same?**

<img src="./images/BounceRates_ExitRates_Scatter.png"> <br>
**Do PageValues v. ExitRates influence No Sale/Sale the same?**

<img src="./images/ExitRates_PageValues_Scatterplot.png"> <br>
**Do PageValues v. ProductRelated influence No Sale/Sale the same?**

<img src="./images/ProductRelated_PageValues_scatterplot.png"> <br> 
**Do PageValues v. Administrative influence No Sale/Sale the same?**

<img src="./images/Administrative_PageValues_Scatterplot.png"> <br>

_**Note:** See the Initial Data Audit Reoprt here..._ [link.](./MachineTeam4_Data_Audit_Report_Final.ipynb)

<br> 

## Part III: Data Preparation
#### Overview of Steps (after csv imported):
  
 **[Preliminary base models/preliminary process:](./Model_1.6_Recall.ipynb)**
   - Encode non-numeric (object/boolean) features
   - Run 7 models with all features & rank using repeated K-Fold cross-validation
   - Address RQ: What drives conversion Positive Predictive Value (PPV) for ‘sale’ (not overall F1-Score [F1] or Area Under the Curve [AUC])
<br> 

**Resuts:** Preliminary base models
<br> <img src="./images/Model_1_results.PNG">
<br>
_**Note:** lr = logistic regression; xgb = extreme gradient boost; tree = decision tree; svm = support vector machines; nn = neural network; bayes = naive bayes; rforest = random forest_
<br>

<br>

**[Initial base models/preparation process:](./Model_2.6_Recall.ipynb)**
   - Impute, encode, bin (categorical features) – e.g., Bin 9 Regions to 5
   - Transform, engineer features, standardize, normalize(continuous features) 
   – e.g., Engineer PageValues v ExitRates
   - Run models & rank; evaluate via PPV for ‘sale’ 
<br> 

**Resuts:** Initial base models
<br> <img src="./images/Model_2_results.PNG">

    
_**Note:** See the Initial Data Models report here..._ [link.](./Initial_Data_Models_Final.ipynb)


## Part IV: Analysis/Modeling/Evaluation
#### Overview of Steps (after data preparation): 

**[Revised base model process:](./Model_3.8_Recall.ipynb)**
- Remove duplicative/highly correlated features
- Use 3 best models & determine top features for each
- Run 3 'best features' models (NB, SVM, XGB)
- Evaluate results via PPV for ‘sale’ & rank models
- Use Tree-based Pipeline Optimization Tool (TPOT) to confirm optimal state of our models
<br> 

**Resuts:** Revised base models
<br> <img src="./images/Model_3_results.PNG">
    
**[Final model process (NB, SVM, XGB):](./Model_4.6_SMOTE_Recall.ipynb)**
- Tune hyperparameters (HPs) in 2 advanced machine learning models (SVM, XGB)
- Use Synthetic Minority Over-sampling Technique (SMOTE) to adjust for imbalanced data
- Run optimized models with ensemble; evaluate via PPV for ‘sale’
<br>

**Resuts:** Final models (NB, SVM, XGB)
<br> <img src="./images/Model_4_results.PNG">



# Part V: Insights/Deployment
#### Naïve Bayes (NB) = best model to predict ‘conversion’
<br> <img src="./images/Confusion_Matrix_NB.png">


_**Note:** TPOT confirms Naïve Bayes as best model; see the Exported TPOT Pipeline here..._ [link.](./tpot_exported_pipeline.py) <br>


#### NB model for ‘conversion’ utilizes 17 features 

**Top 3 categorical features with recommendations:** <br>

- Timing of ‘sale’
    - Later Quarters
    - Weekends
    - **Recommendation:** Increase yearend/weekend marketing <br>
<br>
- Customer location
    - Certain Regions (1,2,3,4)
    - **Recommendation:** Targeted regional marketing <br>
<br>

**Top 3 continuous features with recommendations:**
- Exit rates
    - Fewer exit-prone pages are associated with conversion
    - **Recommendation:** Use exit rates to identify pages to improve <br>
<br>
- Page values
    - Higher page values are associated with conversion
    - **Recommendation:** Identify what influence high page values <br>
<br>
- Product-related pages
    - Fewer product-related page views are associated with conversion
    - **Recommendation:** Identify what reduces product-related page views <br>
<br>
- Administrative
    - Fewer administrative page views may be associated with conversion
        - In bottom half of features in NB model; minor feature in NB
        - Not in SVM or XGB models
    - **Recommendation:** Be cautious in expending resources to further investigate <br>
<br> 

#### How can we improve?
**Expand domain expertise/business knowledge:**
- Search engine optimization experts
- Sportswear retail segment
- Turkish economy/geography/culture/customs
    - Preferred sports? Seasonality?
    - Valentine’s Day? Mother’s Day? <br>
<br>

**Obtain additional information/data:**
- Timing of data collection
- Uncollected data
    - January/April data; additional year(s)
    - Holidays other than Valentine’s Day/Mother’s Day
    - Other unidentified features (e.g., device type) <br>
<br>

#### What can we study next?
**Abandonment:**
- What drives online shoppers to abandon:
    - The site? the shopping cart?
    - Admininitrative (i.e., Admin v PageValues) may be important here
- What will reduce abandonment?
<br>

**Recommended Models:** 
- **Use XGBoost model to uniquely predict ‘abandonment’:** Best performance for ‘abandonment’
<br> <img src="./images/Confusion_Matrix_XG_Abandon.png">

- **Use Naïve Bayes model for ‘abandonment’ _and_ ‘conversion’:** Balanced performance (for ‘abandonment & ‘conversion’) & efficient use of resources
<br> <img src="./images/Confusion_Matrix_NB_Abandon.png">


**Overall Observation:** KISS - Keep It Super Simple
- More sophisticated models:
    - Don’t add much performance
    - Require additional resources
        - Data preparation
        - Computing time
- **Possible explanation:** Simple models (e.g., Naieve Bayes) perform better as they more closely define the probabilistic relationship between the target and features for our dataset

<br>

_**Note:** See the PowerPoint Presentaion here..._ [link.](./BA545-MachineTeam4_Project2_Presentation_FINAL.pdf)

<br>

## Appendix: References
- Berstein, D. 2014. Ecommerce research chart: Industry benchmark conversion rates for 25 retail categories. Marketingsherpa.com. Sept. 15. 18. Available at: https://www.marketingsherpa.com/article/chart/conversion-rates-retail-categories

- Berstein, D. 2018. Ecommerce marketing chart: Median conversion rates for 25 industries. Marketingsherpa.com. Jun. 18. Available at: https://www.marketingsherpa.com/article/chart/median-conversion-rates-25-industries

- Cxl.com. n.d. Bounce rate vs. exit rate: What’s the difference? Blog post. Available at: https://cxl.com/guides/bounce-rate/bounce-rate-vs-exit-rate/

- Gleason, D. 2019. Long sales cycle? Here’s how to set the value of a conversion. Cxl.com. Jan 10. Updated Oct 25. Available at: https://cxl.com/blog/analytics-long-sales-cycles/

- Sakar, C., S. Polat, M. Katircioglu &   Y. Kastro. 2019. Real-time prediction of online shoppers’ purchasing intention using multilayer perceptron and LSTM recurrent neural networks. _Neural Computing and Applications. 31_ [10, October]: 6893-6908

- Taylor, E. 2014. eTail Turkey - E-Commerce Landscape. LinkedIn Slide Share Presentation. Dec. 17. Available at: https://www.slideshare.net/etailevents/etail-turkey-infographic 

- Ystats.com. 2016. Infographic: Turkey B2C E-Commerce Market. LinkedIn Slide Share Presentation. Apr. 20. Available at: https://www.slideshare.net/yStats/infographic-turkey-b2c-ecommerce-market-2016


<br>
<br>
<br>
<br>