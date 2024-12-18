# price-optimization-and-analysis
This Python script analyzes pricing and sales data for a retail store and its competition, applying data visualization techniques, statistical analysis, and dynamic pricing simulations. It utilizes the pandas library for data manipulation and matplotlib for plotting the results. The main goal of the code is to evaluate pricing strategies, sales performance, and potential improvements through dynamic pricing based on customer purchasing behavior.

What the Code Does

Data Import and Exploration:
The script begins by importing the pricing data from a CSV file and exploring its structure by displaying the first few rows (head()) and basic information about the columns (info()).

Price Distribution Visualization:
It creates histograms for the price distribution of both the store's prices and the competition's prices, allowing for a visual comparison of how the prices are spread.

Price vs Sales Analysis:
The code generates scatter plots to compare how the store's prices and the competition's prices correlate with sales amounts. This helps to identify any patterns or trends in the relationship between price and sales.

Weekly Price Trends:
The script calculates the average prices for both the store and the competition over time (weekly), plotting these price trends to observe how they fluctuate across fiscal weeks.

Price Elasticity Calculation:
Price elasticity of demand is computed by analyzing how changes in price (percentage change) relate to changes in quantity sold. The results are visualized to observe elasticity trends over time, which indicates how sensitive customers are to price changes.

Sales Comparison Between Store and Competition:
The code computes total sales and quantities sold for both the store and the competition, comparing the two by aggregating the sales and item quantities.
Price Bracket Analysis:
The script defines price brackets (ranges) for both the store's and the competition's prices. It then calculates total sales within these price ranges, helping to understand which price ranges generate the most sales.

Customer Segmentation Based on Price:
The items are categorized into price segments (low, medium, high). The script calculates price elasticity for each segment, indicating how different customer segments respond to price changes.

Dynamic Pricing Simulation:
The code applies dynamic pricing rules, adjusting prices for medium and high-price segments (increasing prices for medium and decreasing for high) to simulate how dynamic pricing might affect sales. It then compares the total sales under existing pricing and dynamic pricing.

Comparison Summary:
The final analysis compares the total sales and quantities sold between the current pricing and the dynamic pricing scenarios, providing insights into the impact of changing prices on overall performance.

What Is the Use of the Code?

Pricing Strategy Optimization: This code can be used by businesses to analyze the effectiveness of their pricing strategy by comparing their prices to those of competitors and evaluating the impact of different pricing on sales.

Sales and Profitability Analysis: It helps businesses understand how pricing affects demand, allowing them to identify price-sensitive customers or products.
Dynamic Pricing Simulation: The script allows businesses to test dynamic pricing strategies and evaluate whether adjusting prices based on customer segments would improve sales.
Customer Behavior Insights: By calculating price elasticity and segmenting customers based on purchasing behavior, the code provides insights into how customers react to price changes, which can guide pricing decisions.

Concepts Used in the Code

Data Manipulation with Pandas:
Importing data, grouping data, aggregating statistics (like sum, mean), and creating new columns based on calculations.

Price Elasticity of Demand:
A measure of how sensitive the quantity demanded is to changes in price. The formula used is the percentage change in quantity divided by the percentage change in price.

Dynamic Pricing:
Adjusting prices in real-time based on factors like demand elasticity, market conditions, and customer behavior. The code demonstrates how dynamic pricing could be applied in different customer segments.

Data Visualization:
Using histograms, scatter plots, and line charts to visualize distributions and trends in the data. Visualization helps in understanding patterns and correlations in large datasets.
Segmentation:
Categorizing items into different price segments based on their price, which helps in analyzing how each segment responds to price changes.

Time Series Analysis:
The code performs time-based analysis to track how prices change over weeks and how those changes correlate with sales.

Takeaways

Price Sensitivity: The relationship between price and sales is not straightforward, and understanding price elasticity is crucial for businesses looking to optimize their pricing strategy.
Dynamic Pricing Can Be Beneficial: Implementing dynamic pricing, where prices are adjusted based on market conditions and customer behavior, has the potential to increase sales and profitability.

Price Bracket Analysis: Grouping prices into brackets can reveal which price ranges are more competitive and profitable, helping businesses identify pricing opportunities.
Customer Segmentation: Different customer segments may exhibit different price sensitivities, and tailoring pricing strategies to these segments can lead to better sales outcomes.

Data-Driven Decision Making: The code emphasizes the importance of data analysis in making informed decisions about pricing, sales strategies, and market positioning.
Overall, the code provides a comprehensive framework for analyzing and optimizing pricing strategies, helping businesses make data-driven decisions that improve profitability and competitive positioning.

code

import pandas as pd
pricing_data = pd.read_csv("D:\\ms excel\\Competition_Data.csv")
print(pricing_data.head())
pricing_data.info()
import matplotlib.pyplot as plt
plt.figure(figsize=(12, 6))
plt.subplot(1, 2, 1)
plt.hist(pricing_data['Price'], bins=30, alpha=0.7, label='Your Store')
plt.xlabel('Price')
plt.ylabel('Frequency')
plt.title('Price Distribution - Your Store')
plt.subplot(1, 2, 2)
plt.hist(pricing_data['Competition_Price'], bins=30, alpha=0.7, color='orange', label='Competition')
plt.xlabel('Price')
plt.ylabel('Frequency')
plt.title('Price Distribution - Competition')
plt.tight_layout()
plt.show()
plt.figure(figsize=(14, 6))
plt.subplot(1, 2, 1)
plt.scatter(pricing_data['Price'], pricing_data['Sales_Amount'], alpha=0.6, label='Your Store')
plt.xlabel('Price')
plt.ylabel('Sales Amount')
plt.title('Price vs Sales Amount - Our Store')
plt.subplot(1, 2, 2)
plt.scatter(pricing_data['Competition_Price'], pricing_data['Sales_Amount'], alpha=0.6, color='orange', label='Competition')
plt.xlabel('Competition Price')
plt.ylabel('Sales Amount')
plt.title('Competition Price vs Sales Amount')
plt.tight_layout()
plt.show()
pricing_data['Fiscal_Week_ID'] = pd.to_datetime(pricing_data['Fiscal_Week_ID'] + '-1', format='%Y-%U-%w')
weekly_prices = pricing_data.groupby('Fiscal_Week_ID').agg({
    'Price': 'mean',
    'Competition_Price': 'mean'
}).reset_index()
plt.figure(figsize=(12, 6))
plt.plot(weekly_prices['Fiscal_Week_ID'], weekly_prices['Price'], label='Our Store', marker='o')
plt.plot(weekly_prices['Fiscal_Week_ID'], weekly_prices['Competition_Price'], label='Competition', marker='o', color='orange')
plt.xlabel('Fiscal Week')
plt.ylabel('Average Price')
plt.title('Price Changes Over Time')
plt.legend()
plt.grid(True)
plt.show()
pricing_data['price_change'] = pricing_data['Price'].pct_change()
pricing_data['qty_change'] = pricing_data['Item_Quantity'].pct_change()
pricing_data['elasticity'] = pricing_data['qty_change'] / pricing_data['price_change']
pricing_data.replace([float('inf'), -float('inf')], float('nan'), inplace=True)
pricing_data.dropna(subset=['elasticity'], inplace=True)
plt.figure(figsize=(12, 6))
plt.plot(pricing_data['Fiscal_Week_ID'], pricing_data['elasticity'], marker='o', linestyle='-', color='purple')
plt.axhline(0, color='grey', linewidth=0.8)
plt.xlabel('Fiscal Week')
plt.ylabel('Price Elasticity of Demand')
plt.title('Price Elasticity of Demand Over Time')
plt.grid(True)
plt.show()
total_sales_your_store = pricing_data['Sales_Amount'].sum()
total_sales_competition = (pricing_data['Competition_Price'] * pricing_data['Item_Quantity']).sum()

total_qty_your_store = pricing_data['Item_Quantity'].sum()
total_qty_competition = pricing_data['Item_Quantity'].sum()  # assuming quantities sold are the same for comparison

summary = pd.DataFrame({
    'Metric': ['Total Sales Amount', 'Total Quantity Sold'],
    'Your Store': [total_sales_your_store, total_qty_your_store],
    'Competition': [total_sales_competition, total_qty_competition]
})
print(summary)
# define price brackets
bins = [0, 50, 100, 150, 200, 250, 300, 350, 400, 450, 500]
labels = ['0-50', '51-100', '101-150', '151-200', '201-250', '251-300', '301-350', '351-400', '401-450', '451-500']
# create price brackets for both your store and competition
pricing_data['price_bracket'] = pd.cut(pricing_data['Price'], bins=bins, labels=labels, right=False)
pricing_data['competition_price_bracket'] = pd.cut(pricing_data['Competition_Price'], bins=bins, labels=labels, right=False)
# calculate sales amount by price bracket for your store
sales_by_bracket_your_store = pricing_data.groupby('price_bracket')['Sales_Amount'].sum().reset_index()
sales_by_bracket_your_store.columns = ['Price Bracket', 'Your Store Sales Amount']
# calculate sales amount by price bracket for competition
pricing_data['competition_sales_amt'] = pricing_data['Competition_Price'] * pricing_data['Item_Quantity']
sales_by_bracket_competition = pricing_data.groupby('competition_price_bracket')['competition_sales_amt'].sum().reset_index()
sales_by_bracket_competition.columns = ['Price Bracket', 'Competition Sales Amount']
sales_by_bracket = pd.merge(sales_by_bracket_your_store, sales_by_bracket_competition, on='Price Bracket')
print(sales_by_bracket)
# dividing customers based on purchasing behavior
# calculate average price and total quantity sold for each item
item_summary = pricing_data.groupby('Item_ID').agg({
    'Price': 'mean',
    'Item_Quantity': 'sum'
}).reset_index()
# merge the item summary back to the main dataset
pricing_data = pd.merge(pricing_data, item_summary, on='Item_ID', suffixes=('', '_avg'))
# define segments based on average price
pricing_data['segment'] = pd.cut(pricing_data['Price_avg'], bins=[0, 50, 150, 300], labels=['Low', 'Medium', 'High'])
# calculate price elasticity for each segment
segments = pricing_data['segment'].unique()
elasticity_data = []
for segment in segments:
    segment_data = pricing_data[pricing_data['segment'] == segment]
    segment_data['price_change'] = segment_data['Price'].pct_change()
    segment_data['qty_change'] = segment_data['Item_Quantity'].pct_change()
    segment_data['elasticity'] = segment_data['qty_change'] / segment_data['price_change']
    segment_data.replace([float('inf'), -float('inf')], float('nan'), inplace=True)
    avg_elasticity = segment_data['elasticity'].mean()
    elasticity_data.append({'segment': segment, 'avg_elasticity': avg_elasticity})
elasticity_df = pd.DataFrame(elasticity_data)
print(elasticity_df)
# create a copy of the dataset for simulation
dynamic_pricing_data = pricing_data.copy()
# apply dynamic pricing rules
dynamic_pricing_data.loc[dynamic_pricing_data['segment'] == 'Medium', 'dynamic_price'] = dynamic_pricing_data['Price'] * 1.05
dynamic_pricing_data.loc[dynamic_pricing_data['segment'] == 'High', 'dynamic_price'] = dynamic_pricing_data['Price'] * 0.90
# calculate new sales amounts based on dynamic prices
dynamic_pricing_data['dynamic_sales_amt'] = dynamic_pricing_data['dynamic_price'] * dynamic_pricing_data['Item_Quantity']
# compare total sales amount between existing and dynamic pricing
total_sales_existing = pricing_data['Sales_Amount'].sum()
total_sales_dynamic = dynamic_pricing_data['dynamic_sales_amt'].sum()
# compare total quantity sold between existing and dynamic pricing
total_qty_existing = pricing_data['Item_Quantity'].sum()
total_qty_dynamic = dynamic_pricing_data['Item_Quantity'].sum()  # quantity sold remains the same for comparison
comparison_summary = pd.DataFrame({
    'Metric': ['Total Sales Amount', 'Total Quantity Sold'],
    'Existing Pricing': [total_sales_existing, total_qty_existing],
    'Dynamic Pricing': [total_sales_dynamic, total_qty_dynamic]
})
print(comparison_summary)
pricing_data['dynamic_price'] = dynamic_pricing_data['dynamic_price']


