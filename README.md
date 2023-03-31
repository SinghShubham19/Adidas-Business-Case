![Screenshot (493)](https://user-images.githubusercontent.com/121576163/228964585-b3c0617a-5bf3-490b-959c-9a7a87df9cc5.png)
# Adidas Business Case

Adidas AG is a German multinational corporation, founded and headquartered in Herzogenaurach, Bavaria, that designs and manufactures shoes, clothing and accessories. 

## Business Problem

**Generate sales and profit insight of Adidas products across USA and thereby suggest methods to increase operating margin (overall profit).**


## Final Suggestions

1. **Increasing online sales** : For a given sales, online sales method produces the maximum profit as it has highest operating margin. Thus, focus must be on increasing online sales for generating max profit per unit sales done.

2. **Increase units sold in southern region** : For a given sales, southern region produce the maximum profit as it has highest operating margin. Thus, focus must be on increasing units sold there.

3. **Target female population for footwear (Increase units sold of female footwear)** : Although female footwear does not has maximum operating margin, it is still appreciable. 



If varying price per unit is allowed:

1. **If possible, avoid discount in southern region**, as it already has the minimum price per unit. Further reducing it will counteract suggestion 1.

2. **Increase per unit cost of online products** : As unit solds online are already maximum (37.8%), hiking the price even by a small margin will bring good profit. Operation costs will be low too as operating margin is highest online.

## Overall Approach to problem

For this case, I chose Tableau and python to analyze the data with the overall framework described below:

- Cleaned the data using python pandas.
- Removed outliers using z-score and inter quartile range methods.
- Used data to choose the most appropriate business metric to optimize for profit.
- **Suggested optimal sales method, product, region, gender and retailer to target to increase profits**
- Conducted **ANOVA** to ensure generalizability of above steps to wider population.
- Attested an interactive [Tableau Dashboard](https://public.tableau.com/app/profile/shubham.singh1195/viz/AdidasUSASalesDashboard/Dashboard1).

## Data Snapshot

Source: [Kaggle](https://www.kaggle.com/datasets/heemalichaudhari/adidas-sales-dataset)

The dataset has 13 columns described below: 

![Screenshot (487)](https://user-images.githubusercontent.com/121576163/228961607-605d4627-5876-41b4-8d77-378947ba9cba.png)

### Data Attributes
- Retailer
- Retailer ID
- Invoice Date	
- Region
- State 
- City
- Product
- Price per Unit
- Units Sold
- Total Sales
- Operating Profit
- Operating Margin : **It is the net profit made per unit sales.**
- Sales Method

## Choosing the business metric to model
### Relationships

- Price per Unit * Units Sold = Total Sales
- Total Sales * Operating Margin = Operating Profit

To increase operating profit, we can model Price Per Unit, Units Sold or Operating Margin. Price per unit is competitively set, while operating margin is constrained by business model, product and sales method. Thus, we have maximum freedom to increase **units sold** in order to increase profits.

## Data Preparation

- Dropped 1st column (all nulls).
- Renamed columns to apt fields.
- Converted numeric object columns to float.
- Splitted Categorical and Numerical features.

### Outlier Removal

The following features had normal like distribution:
1. Operating Margin

![Screenshot (488)](https://user-images.githubusercontent.com/121576163/228962405-bfec9ae3-d08b-4241-bd48-207347150b82.png)

2.Price per Unit

![Screenshot (489)](https://user-images.githubusercontent.com/121576163/228962744-6e071d40-99f2-4e1d-85b2-e4f30beed195.png)

Used the following z score function to remove outliers from above columns:

```
# Function to remove outliers from dataframe, y is dataframe and x is feature name list
def z_removal(y,x):
    a=np.mean(y[x])
    b=np.std(y[x])
    upper=a+(2*b)
    lower=a-(2*b)
   
    for i in range(0,len(y)):
        
        if (y[x][i]<=lower or y[x][i]>=upper):
            y.drop(i,axis=0,inplace=True)

    y.reset_index(inplace=True)
    y.drop('index',axis='columns',inplace=True)
```

For rest of the numerical features used **IQR method** to remove outliers:

```
def iqr_removal(y,x):
    a=np.percentile(y[x],25)
    b=np.percentile(y[x],75)
    IQR = b - a
    thold=1.5
    outlier_step = IQR * thold
    print(a,b,IQR,thold,outlier_step)
    for i in range(0,len(y)):
        #print(i)
        if (y[x][i]<=a-outlier_step or y[x][i]>=a+(outlier_step)):
            y.drop(i,axis=0,inplace=True)   
    y.reset_index(inplace=True)
    y.drop('index',axis='columns',inplace=True)    

```

## Exploratory Data Analysis
### Key Insights
- **Sales spikes during change of seasons**: Spring (April-May) and Autumn (August-September).

![Screenshot (490)](https://user-images.githubusercontent.com/121576163/228963285-06735c30-07d1-4041-b558-8a8e9c29fd62.png)

- The table below summarizes key takeaways:

![Screenshot (485)](https://user-images.githubusercontent.com/121576163/229158325-c8fee5a7-6a77-430e-9f27-af779a4978cc.png)
## Key Visualizations
- **Numerical Features by Sales Methods**

![Screenshot (496)](https://user-images.githubusercontent.com/121576163/229159423-f04323ec-0648-4e4e-8f5c-5e9c1d5ec952.png)

- **Numerical Features by Region**

![Screenshot (498)](https://user-images.githubusercontent.com/121576163/229160163-dc6f077a-ae22-4d55-9787-0669b4fd0df6.png)

- **Numerical Features by Retailer**

![Screenshot (499)](https://user-images.githubusercontent.com/121576163/229160663-0438dc37-323b-44f1-915e-2d25d4e04af7.png)

## ANOVA and Suggestion

### 1. **Increase online sales**: For a given sales, online sales method produces the maximum profit as it has highest operating margin. Thus, focus must be on increasing online sales for generating max profit per unit sales done.

**Validation of action :**
The suggested action relies on the fact that operating margin is highest in online mode. But is this statistically significant? ,i.e. is our sample genralizable to wider population? We will carry out Anova Test to make sure.

**H0: Null Hypothesis**: The sales method has no effect on Operating Margin.

**H1: Alternate Hypothesis**: Sales Method has a effect on Operating Margin
![Screenshot (494)](https://user-images.githubusercontent.com/121576163/228965402-c7c69dbc-ac3d-459e-906e-8410ee09dd8a.png)
![Screenshot (495)](https://user-images.githubusercontent.com/121576163/228965548-0047ed13-a389-4f6b-b08e-c593f5db9654.png)

    
**Interpretation: Clearly, we see that the p value<0.05. Thus we reject the null hypothesis.**


    
**Conclusion: The sales method does has an effect on operating margin, we can thus move forward with our suggested actions.**

### 2. **Increase units sold in southern region** : For a given sales, southern region produce the maximum profit as it has highest operating margin. Thus, focus must be on increasing units sold there.
**Validation of action:** The suggested action relies on the fact that operating margin is highest in southern region. But is this statistically significant? ,i.e. is our sample genralizable to wider population? We will carry out Anova Test to make sure.

**H0: Null Hypothesis**: The region has no effect on Operating Margin.

**H1: Alternate Hypothesis**: Region has a effect on Operating Margin.

![Screenshot (500)](https://user-images.githubusercontent.com/121576163/229161779-d1482550-547a-4e5f-9dde-73040a7a6543.png)
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


    
**Interpretation: Clearly, we see that the p value<0.05. Thus we reject the null hypothesis.**


    
**Conclusion: The region does has an effect on operating margin, thus our suggestion holds for wider population.**

### 3. **Target female population for footwear** (Increase units sold of female footwear) : Although female footwear does not has maximum operating margin, it is still appreciable. 

**Validation of action:** The suggested action relies on the fact that product influences operating margin. But is this statistically significant? ,i.e. is our sample genralizable to wider population? We will carry out Anova Test to make sure.

**H0: Null Hypothesis:** The product has no effect on Operating Margin.

**H1: Alternate Hypothesis:** Product has an effect on Operating Margin.
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

    
**Interpretation: Clearly, we see that the p value<0.05. Thus we reject the null hypothesis.**


    
**Conclusion: The product does has an effect on operating margin, thus our suggestion holds for wider population.**

### 4. **Increase units sold by sports direct** : For a given sales, sports direct produce the maximum profit as it has highest operating margin. Thus, focus must be on increasing units sold there.
**Validation of action:** The suggested action relies on the fact that retailer influences operating margin. But is this statistically significant? ,i.e. is our sample genralizable to wider population? We will carry out Anova Test to make sure.

**H0: Null Hypothesis:** The retailer has no effect on Operating Margin.

**H1: Alternate Hypothesis:** Retailer has an effect on Operating Margin.
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


    
**Interpretation: We see that the p value<0.05 only when West Gear and Sports Direct are excluded. Thus we can't reject the null hypothesis.**

    
**Conclusion: Our suggestion does not generalizes for wider population.**

## Tableau Dashboard

[Link](https://public.tableau.com/app/profile/shubham.singh1195/viz/AdidasUSASalesDashboard/Dashboard1)
- Snapshot
- ![Screenshot (492)](https://user-images.githubusercontent.com/121576163/228964054-f4647580-6dc6-4b88-bb6b-2e39ba1ee099.png)









