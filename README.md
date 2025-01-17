# Overview

>Thousands of people make a living selling pre-owned items on sites like EBay and finding the best items to sell is the key.  A good place to locate items for sale is the Texas Facilities Commission which collects left behind possessions, salvage, and surplus from Texas state agencies such as DPS, TXDOT, TCEQ, and Texas Parks & Wildlife. Examples of commonly available items include vehicles, furniture, office equipment and supplies, small electronics, and heavy equipment. The goal of this project is to create a predictive model in order to determine the resale value of knives from the Texas State Surplus Store on eBay. Descriptive analysis of over 70K sold knives on eBay in the last 2 years will also be used to examine the profitability of investing in knives from the surplus store. 


# BUSINESS PROBLEM

>My family has been running a resale shop and selling on Ebay and other sites for years and lately the business has picked up. We are interested in exploring if the most common item sold at the Texas Surplus Store, pocket knives, would be a safe investment. On the surface they seem great for reselling, as they are oftentimes collectible and small enough to be easily shipped. 

>I have been experimenting with low cost used knives for resale but have not risked a large capital investment in the higher end items. Analyzing past listings on eBay for the top brands available at the Surplus Store could prove useful for gaining insight on whether a larger investment would pay off. Understanding the risks involved in investing capital into different brands of knives and their potential returns will help narrow down what brands to invest in and help reduce excess inventory.

>It has been very time consuming and inaccurate trying to find the correct value to list an item for on eBay. Currently when listing we try to identify the specific knife by Google search, and then try to find the same or similar items sold on Ebay or other sites. This “guess and check” method often results in inventory not moving due to overpricing or being sold at a price lower than its true potential profit. Building a model that predicts the value of a pocket knife on eBay could help to easily determine the correct value of the item before a listing is live on the website.



# Data Understanding
### Cost Breakdown
- padded envelopes: \$0.50 per knife
- flatrate shipping: \$4.45 per knife
- brand knife at surplus store: 15, 20, 30, or 45 dollars per knife
- overhead expenses (gas, cleaning suplies, sharpening supplies, etc): $3.00
- Ebay's comission, with 13\% being a reasonable approximation

| Brand       | Cost at Surplus Store | Cost Adjusted for Expenses |
| :---        |        :----:         |                       ---: |
| Benchmade   | $45.00                | $52.95 + 13% comission     |
| Buck        | $20.00                | $27.95 + 13% comission     |
| Case        | $20.00                | $27.95 + 13% comission     |
| CRKT        | $15.00                | $22.95 + 13% comission     |
| Kershaw     | $15.00                | $22.95 + 13% comission     |
| SOG         | $15.00                | $22.95 + 13% comission     |
| Spyderco    | $30.00                | $37.95 + 13% comission     |
| Victorinox  | $20.00                | $27.95 + 13% comission     |


> The eBay Finding APIVersion 1.13.0 [findItemsAdvanced](https://developer.ebay.com/devzone/finding/callref/finditemsadvanced.html) and The eBay Shopping APIVersion 1247 [GetMultipleItems](https://developer.ebay.com/Devzone/shopping/docs/CallRef/GetMultipleItems.html) were used to collect data from listed eBay posts.

> All of the data gathered from eBay's public API is limited to listed data posted in the past 90 days and doesn't include a "sold" price. Sold data is only available to seller accounts on a webpapp known as Terapeak. The sold data goes back 2 years.

>A majority of the data was scraped from Terapeak, as this data goes back 2 years as compared to the API listed data that only goes back 90 days. It is assumed a large enough amount of listed data should approximate sold data well enough to prove useful for this project. 


<div class="alert alert-block alert-info">  
<span style="color:green">After cleaning the data to remove lots, groups, and most of the listings with multiple knives using regex, there were over 70,000 listings scraped from Terapeak, represting the final sale price of 70K different used knives and their associated titles and images.</span>   
 <span style="color:yellow">Around 10K listings were obtained using eBay's API, represnting current listed used knives and their asking price. A large enough sample size of knives currently listed for sale should approximate the true value of the knife. A lot more of the sold data was available and is a clear indicator of the value of knives over the past two years.</span>
 </div>
 
<div class="alert alert-block alert-warning">  
After combining the two datasets mentioned above, the data was then filtered to remove outliers using IQR filtering. The final dataset includes roughly 76K listings for knives between the 8 brands of interest.
 </div>
 
### feature creation
<ul>
    <li>converted price: Combined sold price with shipping in USD.</li>
    <li>profit: Combined sold price with shipping in USD minus the cost of the knife at the Surplus Store,Ebay's comission, flat shipping cost for a 0.5 lb item and cost of envelope, and overhead costs.</li>
     <li>Return On Investment(ROI): calculated by dividing the profit earned on an investment by the cost of that investment, expressed in %USD.</li>
</ul> 

## Methods

>This project utilizes descriptive analysis to compare the profitability and return on investment for the 8 different brands avaialable for sale at the Texas State Surplus store. By examining different measures of centrality for previous prices sold on eBay in the past 2 years, the measure of variability, percentiles and also the construction of tables & graphs, it can be inferred which brands of knives to invest more heavily into and which ones to ignore completely at just a glance.


> On top of descriptive analysis, the data will be used to train two different Neural Network models to predict the resale value of knives. The target feature for the model to predict is the total price (shipping included) that a knife should be listed on eBay. One model will use an RNN on titles in order to find potential listings that are undervalued and could be worth investing in. Another model will accept only images as input, as this is an input that can easily be obtained in person at the store. This model will use past sold data of knives on eBay in order to determine within an acceptable amount of error the price it will resell for on eBay (shipping included) using only an image. 


# Results


---------------------------------------------------
### Recurrent Neural Network (GRU)

The GRU Price Predictive Model is recommended for use when listing a pocket knife on sale to help list it appropriately.

**Best Mean Absolute Error: $14.28**

![GRU regPlot](https://github.com/ddey117/Neural_Network_Predicting_Reseller_Success_Ebay/blob/master/images/RNN/regPlot_GRU_performance.png?raw=true)

---------------------------------------------------

### Convoluted Neural Network on Grayscale Images

- The MAE when testing the CNN was roughly \\$25.00. That is an error of plus or minus about 50\% of the mean price of knives sold. Not acceptable yet as compared to the RNN with titles. Will address in future work.



# Business Recommendations
Overall Summary: Invest in Case brand and Spyderco knives at the Texas Surplus store. Benchmade knives will return high profits as well but lower return on investment. Deploy the GRU predictive modeling network when listing a pocket knife for sale on eBay to help balance time, excess inventory costs, and lost revenue. 

## Descriptive Analysis Conclusion

**Risk Tolerance/Available Capital**
![profit](https://github.com/ddey117/Neural_Network_Predicting_Reseller_Success_Ebay/blob/master/images/descriptive_analysis/profit_graph.png?raw=true)
Depending on how much upfront capital invested and if you can tolerate addition risk, Benchmade knives have returned the highest profit in the past 2 years when compared to the other brands avalaible at the Surplus Store. 

**Volume of Sales**
![Top 3](https://github.com/ddey117/Neural_Network_Predicting_Reseller_Success_Ebay/blob/master/images/descriptive_analysis/top3_ROI_graphs.png?raw=true)
It is important to consider the volume of knives sold in order to avoid excess inventory. Benchmade knives have high profit but are rarely listed/sold compared to other brands. They are more rare than the other knives and past data suggest they will likely be harder to find on average when compared to the other brands.

**Risk/Initial Cost continued**
![ROI](https://github.com/ddey117/Neural_Network_Predicting_Reseller_Success_Ebay/blob/master/images/descriptive_analysis/ROI_graph.png?raw=true)

On the other hand, Case knives have demonstrated the lowest risk and highest percentage for return on investment in the past 2 years on eBay and has had the highest daily volume of sales. 


**Balancing Act**

*Three pocket knives to invest in and their considerations*:

- Spyderco knives offer a great compromise between ROI and pure profit. Investing in this brand should return high profits without risking as much capital.


- Case knives have almost double the daily volume of sales compared to Spyderco, and they will likely be easier to find. Case knives are also worth investing in to maximize ROI and efficiency searching at the Surplus Store for more inventory to move. 


- Sale volume was low for Benchmade knives, but they have sold for high profit in the past 2 years on eBay. 
-------------------------------------

## Model Analysis Conclusion

 - The performance metrics for the GRU Price Predictive Model was the Mean Squared Error and Mean Absolute Error for the model when predicting for the correct price to list the knife (an approximation for the true value of the knife). The best performing Model exhibited a Mean Absolute Error of \\$14.28. I believe an error range of plus or minus \\$14.28 outperforms the current process of scrolling through webpages and having the lister try to guess themselves. Even given missing the correct value by about 14 dollars and a quarter for each knife listed on average, the time saved trying to figure out a correct price within an acceptable limit without the model has implicit cost that is hard to value on a spreadsheet.
 
 - Deploying the model helps balance excess inventory costs for too high of prices vs loss revenue.
    
**Summary: I reccomend deploying the Price Predicting Model before posting a pocket knife for sale on eBay.**

 
## Future Work
- Expand data to include other products readily purchasable at the Surplus Store. 

- Explode nested list of images as form of data augmentation to increase image input size by about 5-fold. Attempt other data augmentation techniques.

- Obtain more aspect related data for knives to understand most important cost altering features more thoroughly. Enough of this data could use a predictive model that isn’t a black box.


# THANK YOU



Dylan Dey

email: ddey2985@gmail.com
