# 針對賣二手手機販賣文章內容估計對應賣價
### Index

* [Data格式介紹](#Data格式介紹)
* [估計價格準確度](#估計價格準確度)
* [結論](#結論)
----

<a name="Data格式介紹"/>

### Data格式介紹

![](/pic/data_format.png)

1. Article Date : 文章發布日期
1. Article Title : 文章標題 (`內容通常是欲販賣物品品牌及規格`)
1. Article : 文章內容，原始內容`大部分`涵蓋底下(當然也有只有部分的文章)，這裡已經前處理過將欲售價格拿掉 (`欲售價格就是底下的Sell Price`)
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

### 估計價格準確度

*  筆者原先是使用LSTM來預估，但誤差( `|實際賣價 - 預估賣價 |` )的平均值大概在1500左右，改用BERT Model經過長時間的調校，誤差已從1500降至1000上下
*  底下第一張圖為誤差Status

   > 1. 這裡Test Sample數497與我給資料中Valid只有478筆不同，原因是因為少的19筆裡面有些資料格式謬誤及含一些我認為敏感資訊所以拿掉 
   > 2. 紫色線為Error P05/P95的位置
   > 3. 下圖左邊Trend的X軸為實際賣價，Y軸為`|實際賣價 - 預估賣價 |`，可發現誤差隨著實際賣價越高，誤差也就越大，而且預估賣價常大於實際賣價 (`紅色水平線以下`)
   > 4. 下圖右邊Density Plot為Error機率分布，大致上呈現鐘形分布，Error = 786.27為Mean Absolute Error(MAE)

![](/pic/Error_Status.png)

*  底下第二張圖為`實際賣價`與`預估賣價` Correlation Status

   > 第一張Trend X軸為發表文章時間 (`應該吧??`)，黑色線為實際賣價，紅色線為預估賣價
   > 第二張Scatter Plot X軸為實際賣價，Y軸為預估賣價，可發現實際賣價1萬元以下，誤差較小，超過1萬元以上，誤差逐漸變大且預估多數大於實際(紅色對角線左邊)
   
![](/pic/Corr_Status.png)

----

<a name="結論"/>

### 結論

* 二手賣價其實是由市場機制決定，雖然對相同品項每個人賣價不盡相同，但在市場機制下勢必會收斂到某區間範圍內，本次實驗目的主要在於證明，當資料夠多時，我們是能藉由NLP憑販賣文章內容有著不錯的估計賣價貼近市場機制，後續有時間會再逐漸Release Source Code及Model，不過筆者也將所使用資料Share供有興趣者研究(在Data Folder有兩份，第一份為[big5_secondhand_mobile_sales.zip](/data/big5_secondhand_mobile_sales.zip)供Window電腦使用，第二份為[secondhand_mobile_sales.zip](secondhand_mobile_sales.zip)供Linux電腦使用)。
