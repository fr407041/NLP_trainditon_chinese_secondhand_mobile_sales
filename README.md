# Estimating Secondhand Mobile Phone Selling Price based on Sales Listing Content
### Index

* [Introduction to Data Format](#Data格式介紹)
* [Estimating Price Accuracy](#估計價格準確度)
* [Conclusion](#結論)
----

<a name="Data格式介紹"/>

### Introduction to Data Format

![](/pic/data_format.png)

1. Article Date: Date when the article was published
1. Article Title: Title of the article (usually includes the brand and specifications of the item for sale)
1. Article: Content of the article, which typically includes the following (although some articles may only have partial content). The sell price has been removed through preprocessing (Sell Price is mentioned below).

```
欲售物品：
物品狀況：全新/二手
購買來源：
盒裝內含：無盒/內含物品
購買日期：
保固期限：
購入價格：
欲售價格：       
```
1. Sell Price : 即原本Article中的`欲售價格`

----

<a name="估計價格準確度"/>

### Estimating Price Accuracy

*  The author originally used LSTM to estimate prices, but the average error (|actual selling price - estimated selling price|) was around 1500. After a long tuning process using the BERT model, the error has been reduced to around 1000.

*  The first image below shows the error status, where the number of test samples (497) differs from the 478 valid samples in the provided data. This is because some of the 19 missing samples had data format errors or contained sensitive information that was removed.

   > 1. The purple line represents the location of Error P05/P95, and the left side of the trend chart shows the actual selling price on the X-axis and the |actual selling price - estimated selling price| on the Y-axis. It can be observed that the error increases as the actual selling price increases, and the estimated selling price is often greater than the actual selling price (below the red horizontal line).
   > 2. On the right side of the trend chart, the density plot shows the probability distribution of errors, which roughly follows a bell curve. The Error = 786.27 represents the Mean Absolute Error (MAE).

![](/pic/Error_Status.png)

*  The second graph displays the correlation status of actual and predicted selling prices. The first graph presents the trend of actual and predicted selling prices over time. In the second scatter plot, the x-axis represents the actual selling price, and the y-axis represents the predicted selling price. It can be observed that the prediction error is smaller for prices below 10,000 dollars and increases as the price surpasses that threshold. Additionally, the predicted selling prices are often higher than the actual prices, as seen in the red dots located left of the diagonal line.

![](/pic/Corr_Status.png)

----

<a name="結論"/>

### Conclusion

* The second-hand selling price is determined by the market mechanism. Although the selling price for the same item may vary among individuals, it will inevitably converge to a certain range under the market mechanism. The purpose of this experiment is to demonstrate that with enough data, we can use NLP to estimate the selling price based on the content of the selling article and get close to the market mechanism. We will gradually release the source code and model later, and the author will also share the data used in the experiment for those who are interested in researching (there are two files in the Data Folder, the first one is [big5_secondhand_mobile_sales.zip](/data/big5_secondhand_mobile_sales.zip) for Windows computers, and the second one is [secondhand_mobile_sales.zip](secondhand_mobile_sales.zip) for Linux computers).
