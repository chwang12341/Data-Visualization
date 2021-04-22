# 資料視覺化(Data Visualization) - Python 套件 - 互動式繪圖 - 各種類型圖的繪製 - Plotly筆記(六)



一起努力繼續學習Plotly!!







## 15. 箱型圖 plotly.graph_objs.Box()



+ 使用plotly.graph_objs.Box()函數來實現
+ 參數介紹

1. boxpoints: 可以傳入'all', 'outliers', 'suspectedoutliers', False，就會在箱型圖旁邊呈現用點描繪的箱型圖
2. jitter: 傳入0~1的值，代表抖動的程度，如舉例3，我將中間的jitter改為1，數據點的抖動幅度就變大了，數據點的分離程度也就越大
3. pointpos: 點與箱型圖的相對位置，可以傳入介於-2~2之間的數值
4. quartilemethod: 計算四分位數的演算法，有'exclusive', 'inclusive', 'linear'演算法，預設為'linear'
5. marker_color: 箱型圖的顏色
6. boxmean: 傳入True表示顯示平均值，傳入sd表示顯示平均值和標準差


+ 舉例1: boxpoints = 'all'用法

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 創建數據集
y = [2,4,6,6,6,6,6,6,8,8,8,14,14,18,18,28,28,28,28,38]

## 構建箱型圖
data = go.Box(
    y = y,
    boxpoints = 'all',
    pointpos = -1.9,
    jitter = 0.6
)

fig = go.Figure(data = data)

## 顯示圖像
fig.show()
```

**執行結果**

![1](images\1.PNG)





+ 舉例2: 水平方向的箱型圖

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 創建數據集
x = [2,4,6,6,6,6,6,6,8,8,8,14,14,18,18,28,28,28,28,38]

## 構建箱型圖
data = go.Box(
    x = x,
    boxpoints = 'all',
    pointpos = -1.9,
    jitter = 0.6
)

fig = go.Figure(data = data)

## 顯示圖像
fig.show()
```

**執行結果**

![2](images\2.PNG)



+ 舉例3: 調整marker_color與boxmean，顏色與顯示的數據(平均值或平均值與標準差)

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 創建數據集
y = [2,4,6,6,6,6,6,6,8,8,8,14,14,18,18,28,28,28,28,38]

## 創建大圖
fig = go.Figure()

## 堆疊箱型圖
fig.add_trace(go.Box(
    y = y,
    name = "Mean and Blue color",
    marker_color = 'blue',
    boxmean = True
))


fig.add_trace(go.Box(
    y = y,
    name = "Mean & SD and  Red color",
    marker_color = 'red',
    boxmean = "sd"
))


## 顯示圖像
fig.show()
```

**執行結果**

![3](images\3.PNG)








+ 舉例4: 組別箱型圖

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建數據集
x = ['group1', 'group1', 'group1', 'group2', 'group2', 'group2']

## 創建大圖表
fig = go.Figure()

## 堆疊箱型圖

fig.add_trace(go.Box(
    x = x,
    y = [2, 2, 6, 4, 8, 6],
    name = 'Data1',
    marker_color = "#9F35FF"

))


fig.add_trace(go.Box(
    x = x,
    y = [4, 2, 8, 5, 7, 2],
    name = 'Data2',
    marker_color = "#A5A552"

))

fig.add_trace(go.Box(
    x = x,
    y = [7, 9, 3, 6, 1, 6],
    name = 'Data3',
    marker_color = "#005AB5"

))

## 更新介面
fig.update_layout(
    yaxis_title = "Data",
    boxmode = 'group' ## 可以傳入'group'或'overlay'
)


## 顯示圖像
fig.show()
```

**執行結果**

![5](images\5.PNG)







**小筆記: 其實就是原本的傳入Bar()的是y值，改成x值即可**

**更多的箱型圖用法可以參考官網https://plotly.com/python/box-plots/**





## 16. 等高線圖 plotly.graph_objs.Histogram2dContour



+ 使用plotly.graph_objs.Histogram2dContour()函數來達成
+ 參數介紹

1. colorscale: 顏色條
2. contours: 設定等高線，可以傳入字典(Dict)格式
+ showlabels: 傳入True/False，是否顯示標籤
+ labelfont: 傳入字典(Dict)格式，設定標籤樣式
3. hoverlabel: 設定鼠標移入的標籤樣式
+ bgcolor: 背景顏色
+ bordercolor: 邊界顏色
+ font: 傳入字典(Dict)格式，設定字體樣式



+ 舉例

```Python
## 導入Plotly套件
import plotly.graph_objs as go


import numpy as np
## 創建數據集
## uniform 均勻分佈，unifom(low, high, size)
x = np.random.uniform(-1, 1, size = 1000)
y = np.random.uniform(-1, 1, size = 1000)

## 構建等高線圖
fig = go.Figure(go.Histogram2dContour(
    x = x,
    y = y,
    colorscale = 'Viridis',
    contours = dict(
        showlabels = True,
        labelfont = {'family': 'Raleway', 'color': 'purple'}
    ),
    hoverlabel = dict(
        bgcolor = 'yellow',
        bordercolor = 'darkblue',
        font = {'family': 'Raleway', 'color':'blue'}    
    )
))

## 顯示等高線圖
fig.show()
```

**執行結果**

![6](images\6.PNG)







## 17.  圓餅圖與fig.update_traces和fig.update_layout參數介紹



+ 使用plotly.graph_objs.Pie()函式來達成
+ 參數介紹

1. labels: 圓餅圖各面積代表的特徵類別值，鼠標移入的資訊框顯示名稱，傳入一組特徵欄位值
2. values: 傳入一組特徵欄位值，代表圓餅圖內各特徵面積的大小
3. hole: 中間有個洞的比例，繪製甜甜圈圖使用
4. pull: 拉離圓餅圖的距離，製作將某個區域拉離圓餅圖使用，傳入一個串列(list)格式，其中每個數值代表每個區域分離圓餅圖的程度

+ fig.update_traces() 參數介紹

1. textposition: 顯示於圓餅圖上各面積資訊的位置，可以傳入'outside', 'inside'等位置值
2. textinfo: 顯示於圓餅圖上各面積的資訊，可以傳入"percent", "label", "value", 兩兩組合(ex. "percnt+label"), "percent+label+value"
3. textfont_size: 字體大小
4. insidetextorientation: 裡面的字體方向，有"horizontal", "radial", "tangential"和"auto"可以傳入
5. marker: 可以以字典的格式傳入，控制圓餅圖的樣式
+ colors: 區塊顏色
+ line: 邊界線條顏色


+ fig.update_layout()參數介紹
1. title_text: 傳入字串(string)格式，主題名稱
2. annotations: 加入一些註解
+ text: 文字
+ x, y: 文字在圈圈內的x軸與y軸相對位置
+ font_size: 字體的大小
+ showarrow: 傳入True/False，是否呈現箭頭



+ 舉例1: 圓餅圖

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建顏色選擇列表
colors = ['red', 'gold', 'darkblue', 'purple']

## 構建圓餅圖
fig = go.Figure(go.Pie(
    labels = ["T-shirts", "Pants", "Jeans", "Shoes"],
    values = ["580000", "280000", "660000", "200000"],
    insidetextorientation = 'tangential'
))

## 更新圖形樣式
fig.update_traces(
    textinfo = 'value',
    textfont_size = 20,
    hoverinfo = 'percent+label',
    marker = dict(colors = colors, line = dict(color = 'yellow', width = 6))
    
)

## 更新介面樣式
fig.update_layout(title = "Monthly Earns per Shop")

## 顯示圓餅圖
fig.show()
```

**執行結果**

![7](images\7.PNG)




+ 舉例2: 圓餅圖變形 - 甜甜圈圖

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建數據集
labels = ["T-shirts", "Pants", "Jeans", "Shoes"]
values = ["580000", "280000", "660000", "200000"]

## 構建圓餅圖
fig = go.Figure(go.Pie(
    labels = labels,
    values = values,
    hole = 0.3
))

## 顯示圖像
fig.show()
```

**執行結果**

![8](images\8.PNG)




+ 舉例3: 圓餅圖變形 - 把一塊拉出來的圓餅圖
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建數據集
labels = ["T-shirts", "Pants", "Jeans", "Shoes"]
values = ["580000", "280000", "660000", "200000"]

## 構建圓餅圖
fig = go.Figure(go.Pie(
    labels = labels,
    values = values,
    pull = [0, 0.4, 0, 0]
))

## 顯示圖像
fig.show()
```

**執行結果**

![9](images\9.PNG)



+ 舉例4: 增加註解annotations於子圖(subplots)的中心

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 導入子圖套件
from plotly.subplots import make_subplots


## 構建數據集
labels = ["T-shirts", "Pants", "Jeans", "Shoes", "cap", "Socks"]
values1 = ["580000", "280000", "660000", "200000", "360000", "980000"]
values2 = ["600000", "270000", "390000", "260000", "600000", "580000"]

## 建立子圖
fig = make_subplots(rows = 1, cols = 2, specs = [[{'type': 'domain'}, {'type': 'domain'}]])

## 繪製子圖
fig.add_trace(go.Pie(
    labels = labels,
    values = values1,
    name = "Shop 1 Earning"
    
), row = 1, col = 1)

fig.add_trace(go.Pie(
    labels = labels,
    values = values2,
    name = "Shop 2 Earning"
    
), row = 1, col = 2)

## 更新圖形樣式
fig.update_traces(hoverinfo = "percent+label+name+value", hole = 0.58)

## 更新介面樣式
fig.update_layout(
    title_text = "Monthly Earns per Shop",
    annotations = [
        dict(text = 'Shop 1', x = 0.18, y = 0.5, font_size = 20, showarrow = False),
        dict(text = 'Shop 2', x = 0.82, y = 0.6, font_size = 20, showarrow = False)
    ]
    
)

## 顯示圖像
fig.show()
```

**執行結果**

![10](images\10.PNG)



**更多圓餅圖寫法可以直接參考官網https://plotly.com/python/pie-charts/**







## 18. Sunburst 圖



+ 使用plotly.graph_objs.Sunburst()函數來達成
+ 參數介紹
1. labels: 傳入一個串列(list)，為每個區域的標籤名稱
2. parents: 傳入一個串列(list)來對應labels串列，代表每個標籤的依屬
3. values: 傳入一個串列，為每個區塊的值
4. branchvalues: 分支值，有"remainder", "total"可以選擇傳入

+ 舉例
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建Sunburst圖
fig = go.Figure(go.Sunburst(
    labels = ['Jack', 'Tom', 'Jenny', 'Amy', 'Angel', 'Ken', 'Ben', 'James', 'Jessica', 'Frank'],
    parents = ['', 'Ben', 'Ben', '', '' , 'James', '', '', '', ''],
    values = [18, 28, 30, 28, 58, 66, 78, 66, 76, 66],
    branchvalues = "remainder"
))


## 更新介面樣式
fig.update_layout(margin = dict(t = 0, l = 0, b = 0, r = 0))

## 顯示Sunburst圖
fig.show()
```

**執行結果**

![11](images\11.PNG)





**更多的Sunburst圖寫法可以直接參考官網 https://plotly.com/python/sunburst-charts/**



**當然除了這些還有更多類型的圖，大家有興趣可以直接參考官網https://plotl.com/python/教學喔，也可以跟著我在之後的文章中一起繼續學習**









## Reference
https://www.youtube.com/watch?v=ifYugIP0pPQ
htts://plotly.com/python/
https://www.cnblogs.com/feffery/p/9293745.html
https://blogs.csdn.net/u012897374/article/details/77857980
https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf
https://www.jianshu.com/p/57bad75139ca
