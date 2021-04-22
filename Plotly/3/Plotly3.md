# 資料視覺化(Data Visualization) - Python 套件 - 互動式繪圖 - 各種類型圖的繪製 - Plotly筆記(三)



這篇會直接帶大家使用Plotly來進行各種圖形的繪製喔！！





## 1. 折線圖 & 散點圖




+ 上一篇中我們有實作過這部分，兩種類型圖都可以使用plotly.graph_objs.Scatter()函式來實現，實作方式的差別在於mode，是傳入markers、lines、text、兩倆配對或三個組合使用
+ 接下來我們使用的數據集來自於Plotly放在github上的各種數據集，非常適合大家拿來練習使用 - https://gihub.com/plotly/datasets
+ 舉例: 這邊使用股市資料來demo，我將IBM以折線的方式，SBUX以折線加散點的方式，AAPL用散點的方式來呈現，也透過這張圖帶大家感受一下這三種搭配組合的差別





**STEP 1:** 導入數據集與Plotly所需的套件

```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go
import pandas as pd


## 設定為離線
pof.init_notebook_mode(connected = True)

## 導入數據集
stock_df = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/stockdata.csv")

## 取數據集中的前40筆
stock_df = stock_df.head(40)

## 顯示數據
stock_df
```

**執行結果**

![1](images\1.PNG)





**STEP 2:** 繪圖

```Python
## 數據集 折線圖
data1 = go.Scatter(
    x = stock_df['Date'],
    y = stock_df['IBM'],
    mode = "lines",
    name = 'IBM'
)

## 數據集 折線+點圖
data2 = go.Scatter(
    x = stock_df['Date'],
    y = stock_df['SBUX'],
    mode = "lines+markers",
    name = 'SBUX'
)


## 數據集 點圖
data3 = go.Scatter(
    x = stock_df['Date'],
    y = stock_df['AAPL'],
    mode = "markers",
    name = 'AAPL'
)


## 數據集 折線+點+文字圖
data4 = go.Scatter(
    x = stock_df['Date'],
    y = stock_df['MSFT'],
    mode = "lines+markers+text",
    name = 'MSFT'
)


## 介面
layout = go.Layout(
    title = 'Stock',
    xaxis = {'title': 'Date'},
    yaxis = {'title': 'Price'},
    showlegend = True,
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 28,
        'family': 'fantasy',
        'color': '#D9006C'
    },
)

## 組合成Figure
figure = go.Figure(data = [data1, data2, data3, data4], layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Scatter', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)
```

**執行結果**

![2](images\2.PNG)



**補充: 如果想要自動保存圖片，只要在iplo()裡面加上image參數，可以有的的選項: 'png', 'jpeg', 'svg', 'webp'，就會自動保存成圖擋喔**







## 2. 柱狀圖 plotly.graph_objs.Bar()



+ 柱狀圖的話，我們使用的是plotly.graph_objs.Bar()來實現，其參數與Scatter幾乎一樣，但沒有mode參數
+ 舉例: 一樣拿股市數據集進行繪圖，但這次我想要把資料縮減，我們只要2007年2月份的數據集，而且只想針對IBM和MSFT就好，但我在下面的程式碼中會將四間公司都放上喔，只是會註解掉其他兩間，如果想要四間都呈現，大家可以自行將註解打開，並在Figure()中data參數添加這兩筆數據即可


```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go
import pandas as pd


## 設定為離線
pof.init_notebook_mode(connected = True)

## 導入數據集
stock_df = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/stockdata.csv")

## 取數據集中的前40筆
stock_df = stock_df.head(40)

## 將數據設定在2007年二月份
stock_df = stock_df[(stock_df['Date'] < '2007-03-01') & (stock_df['Date'] > '2007-01-31')]

## 數據1
data1 = go.Bar(
    x = stock_df['Date'],
    y = stock_df['IBM'],
    name = 'IBM'
)


### 數據2
# data2 = go.Bar(
#  x = stock_df['Date'],
# y = stock_df['SBUX'],
# name = 'SBUX'
# )


# ## 數據3 
# data3 = go.Bar(
#  x = stock_df['Date'],
#  y = stock_df['AAPL'],
#  name = 'AAPL'
# )


## 數據4
data4 = go.Bar(
    x = stock_df['Date'],
    y = stock_df['MSFT'],
    name = 'MSFT'
)



## 介面
layout = go.Layout(
    title = 'Stock',
    xaxis = {'title': 'Date'},
    yaxis = {'title': 'Price'},
    showlegend = True,
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 28,
        'family': 'fantasy',
        'color': '#D9006C'
    },
)

## 組合成Figure
figure = go.Figure(data = [data1, data4], layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Scatter', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)
```

**執行結果**

![3](images\3.PNG)









## 4. 直方圖 plotly.graph_objs.Histogram()



+ 直方圖所使用的是plotly.graph_objs.Histogram()，它有一個特點，就是只傳入一維數據，也就是只需要傳入一組數據，它是統計這個一維數據中，不同區間的值出現的次數，它有特別的參數xbins，指的是統計區間值，以xbins = {'size': 1}為例，就是區間的距離為1，以下面的例子來說77~77.9, 78~78.9，77.9~77的相差值就是我們設定的xbins
+ 調整Layout的參數bargap: 傳入值介於0~1之間，代表直方圖之間的距離差
+ 舉例1: 我們一樣拿股市數據集中的IBM來傳入看看，發現它會統計77~77.9, 78~78.9等的區間次數



```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go
import pandas as pd


## 設定為離線
pof.init_notebook_mode(connected = True)

## 導入數據集
stock_df = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/stockdata.csv")

## 取數據集中的前40筆
stock_df = stock_df.head(40)

## 將數據設定在2007年二月份
stock_df = stock_df[(stock_df['Date'] < '2007-03-01') & (stock_df['Date'] > '2007-01-31')]

## 數據1
data1 = go.Histogram(
    x = stock_df['IBM'],
    name = 'IBM'
)


## 介面
layout = go.Layout(
    title = 'Stock',
    xaxis = {'title': 'Date'},
    yaxis = {'title': 'Price'},
    showlegend = True,
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 28,
        'family': 'fantasy',
        'color': '#D9006C'
    },
    bargap = 0.1
)

## 組合成Figure
figure = go.Figure(data = [data1], layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Scatter', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)
```

**執行結果**

![4](images\4.PNG)



+ 舉例2: 帶入參數xbins = {'size': 2}


```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go
import pandas as pd


## 設定為離線
pof.init_notebook_mode(connected = True)

## 導入數據集
stock_df = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/stockdata.csv")

## 取數據集中的前40筆
stock_df = stock_df.head(40)

## 將數據設定在2007年二月份
stock_df = stock_df[(stock_df['Date'] < '2007-03-01') & (stock_df['Date'] > '2007-01-31')]

## 數據1
data1 = go.Histogram(
    x = stock_df['IBM'],
    xbins = {'size': 2},
    name = 'IBM'
)


## 介面
layout = go.Layout(
    title = 'Stock',
    xaxis = {'title': 'Date'},
    yaxis = {'title': 'Price'},
    showlegend = True,
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 28,
        'family': 'fantasy',
        'color': '#D9006C'
    },
    bargap = 0.1
)

## 組合成Figure
figure = go.Figure(data = [data1], layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Scatter', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)
```

**執行結果**

![5](images\5.PNG)



## 5. 堆疊圖



+ 一樣使用plotly.graph_objs.Bar()，但要於layout中添加一個參數barmode = 'stack'，就可以繪製成堆疊圖囉

```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go
import pandas as pd


## 設定為離線
pof.init_notebook_mode(connected = True)

## 數據1
data1 = go.Bar(
    x = ['cap', 't-shirt', 'pants'],
    y = ['200', '100', '500'],
    name = 'A-Mart'


)


## 數據2
data2 = go.Bar(
    x = ['cap', 't-shirt', 'pants'],
    y = ['200', '100', '500'],
    name = 'B-Mart'


)


## 數據3
data3 = go.Bar(
    x = ['cap', 't-shirt', 'pants'],
    y = ['200', '400', '600'],
    name = 'C-Mart'


)


## 數據4
data4 = go.Bar(
    x = ['cap', 't-shirt', 'pants'],
    y = ['200', '400', '600'],
    name = 'D-Mart'


)


## 介面
layout = go.Layout(
    title = 'Stock',
    xaxis = {'title': 'Date'},
    yaxis = {'title': 'Price'},
    showlegend = True,
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 28,
        'family': 'fantasy',
        'color': '#D9006C'
    },
    barmode = 'stack'
)

## 組合成Figure
figure = go.Figure(data = [data1, data2, data3, data4], layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Scatter', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)
```

**執行結果**

![6](images\6.PNG)







## 6. 圓餅圖 Pie plotly.graph_objs.Pie()



+ 使用plotly.graph_objs.Pie()來繪製圓餅圖
+ plotly.graph_objs.Pie 參數 

1. labels: 傳入類別的標籤名稱，以串列(list)的形式傳入
2. values: 標籤類別名稱對應的數值，也是以串列的形式傳入
3. hoverinfo: 當鼠標移上去出現的訊息，可以傳入'label+percent'，就會顯示標籤名稱和百分比資訊
4. textinfo: 圓餅圖上的文字，傳入'value'就會顯示對應圖形區域的標籤值


```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go
import pandas as pd


## 設定為離線
pof.init_notebook_mode(connected = True)

## 構建數據集
group_student = ['Technology', 'Design', 'Management', 'Medicine']
student_amount = [100, 80, 60, 60]
colors = ['blue', 'red', 'purple', 'grey']


## 數據1
data1 = go.Pie(
    labels = group_student,
    values = student_amount,
    hoverinfo = 'label+percent',
    textinfo = 'value',
    textfont = {'size': 10},
    marker = {
        'colors': colors,
        'line': {'color': 'black', 'width': 4}
    }
    
)


## 介面
layout = go.Layout(
    title = 'Stock',
    xaxis = {'title': 'Date'},
    yaxis = {'title': 'Price'},
    showlegend = True,
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 28,
        'family': 'fantasy',
        'color': '#D9006C'
    },
)

## 組合成Figure
figure = go.Figure(data = [data1], layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Scatter', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)
```

**執行結果**

![7](images\7.PNG)





當然除了這些還有更多類型的圖，大家有興趣可以直接參考官網https://plotl.com/python/教學喔，也可以跟著我在之後的文章中一起繼續學習


大家是否已經迫不及待想拿手邊的數據試試了，Plotly繪出來的圖真的好美~~ 希望這篇有幫助到您!!







## Reference

https://www.youtube.com/watch?v=ifYugIP0pPQ
https://plotl.com/python/https://www.cnblogs.com/feffery/p/9293745.html
https://blogs.csdn.net/u012897374/article/details/77857980
https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf
https://www.jianshu.com/p/57bad75139ca








