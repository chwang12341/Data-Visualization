# 資料視覺化(Data Visualization) - Python 套件 - 互動式繪圖 - 各種類型圖的繪製 - Plotly筆記(六)



大家已經會用Plotly繪製許多圖形了，這篇我們再來學習其他圖形的繪製方法！！







## 19. Treemap 圖



+ 使用plotly.graph_objs.Treemap()函式來實現
+ 參數介紹

1. labels: 傳入一個串列(list)，為每個區域的標籤名稱
2. parents: 傳入一個串列(list)來對應labels串列，代表每個標籤的依屬
3. values: 傳入一個串列，為每個區塊的值
4. branchvalues: 分支值，有"remainder", "total"可以選擇傳入
5. textinfo: 區塊顯示的資訊
6. pathbar: 傳入字典(Dict)形式，Treemap圖的主要附加功能，用於顯示層次結構圖可見部分的當前路徑
+ visible: 傳入True/False，是否呈現
7. marker_colors: 傳入串列(list)，指定顏色
8. treemapcolorway: 用在fig.update_layout()中，傳入串列(list)，指定顏色，它會根據繼承的父節點的顏色來變化
9. marker_colorscale: 傳入顏色條


+ 舉例1: 基本樹狀圖

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 創立數據集
charactors = ['Jack', 'Tom', 'Jenny', 'Amy', 'Angel', 'Ken', 'Ben', 'James', 'Jessica', 'Frank']
parents = ['', 'Ben', 'Ben', '', '' , 'James', '', '', '', '']
values = [18, 28, 30, 28, 58, 66, 78, 66, 76, 66]

## 構建圖像
fig = go.Figure(go.Treemap(
    labels = charactors,
    parents = parents,
    values = values
))



## 顯示Sunburst圖
fig.show()
```

**執行結果**

![1](images\1.PNG)








+ 舉例2: 使用許多參數和使用marker_colors
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 導入子圖套件
from plotly.subplots import make_subplots


## 創立數據集
charactors = ['Jack', 'Tom', 'Jenny', 'Amy', 'Angel', 'Ken', 'Ben', 'James', 'Jessica', 'Frank']
parents = ['', 'Ben', 'Ben', '', '' , 'James', '', '', '', '']
values1 = [18, 28, 30, 28, 58, 66, 78, 66, 76, 66]
values2 = [28,10,24,8,83,97,78,66,58,66]


## 繪製子圖
fig = make_subplots(
   rows = 1, cols = 2,
   column_width = [0.5, 0.5],
   subplot_titles = ('branchvalues: <b>total</b>',  'branchvalues: <b>remainder</b> '),
   specs = [[{'type': 'treemap', 'rowspan': 1}, {'type': 'treemap'}]]
)

fig.add_trace(go.Treemap(
    branchvalues = "total",
    labels = charactors,
    parents = parents,
    values = values1,
    textinfo = "label+percent parent+value+percent entry+percent root",
    pathbar = dict(visible = False)
), row = 1, col = 1)


fig.add_trace(go.Treemap(
    branchvalues = "remainder",
    labels = charactors,
    parents = parents,
    values = values2,
    textinfo = "label+percent parent+value+percent entry+percent root",
    outsidetextfont = dict(size =20, color = "gold"),
    marker = dict(line = {'width': 2, 'color': 'blue'}),
    pathbar = dict(visible = False),
    marker_colors = ["gold", "pink", "cyan", "darkblue", "orange", "gray", "brown", "lightblue", "blue"]
), row = 1, col = 2)




## 顯示圖像
fig.show()
```

**執行結果**

![2](images\2.PNG)




+ 舉例3: 使用colorway

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 創立數據集
charactors = ['Jack', 'Tom', 'Jenny', 'Amy', 'Angel', 'Ken', 'Ben', 'James', 'Jessica', 'Frank']
parents = ['', 'Ben', 'Ben', '', '' , 'James', '', '', '', '']
values = [18, 28, 30, 28, 58, 66, 78, 66, 76, 66]

## 構建Treemap
fig = go.Figure(go.Treemap(
    labels = charactors,
    parents = parents,
    values = values
    #treemapcolorway = ["gold", "pink", "cyan", "darkblue", "orange", "gray", "brown", "lightblue", "blue"]
))

## 更新介面
fig.update_layout(treemapcolorway = ["gold", "pink", "cyan", "darkblue", "orange", "gray", "brown", "lightblue", "blue"])


## 顯示圖像
fig.show()
```

**執行結果**

![3](images\3.PNG)






+ 舉例4: 使用marker_colorscale

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 創立數據集
charactors = ['Jack', 'Tom', 'Jenny', 'Amy', 'Angel', 'Ken', 'Ben', 'James', 'Jessica', 'Frank']
parents = ['', 'Ben', 'Ben', '', '' , 'James', '', '', '', '']
values = [18, 28, 30, 28, 58, 66, 78, 66, 76, 66]

## 構建Treemap
fig = go.Figure(go.Treemap(
    labels = charactors,
    parents = parents,
    values = values,
    marker_colorscale = 'Ice_r'
))



## 顯示圖像
fig.show()
```

**執行結果**

![4](images\4.PNG)





**更多Treemap的寫法可以直接參考官網https://plotly.com/python/treemaps/**









## 20. 雷達散點圖



+ 使用plotly.graph_objects.Scatterpolar()函式來實現
+ 參數介紹
1. r: 數值的大小，在雷達圖中用距離的方式呈現
2. theta: 雷達圖的維度
3. mode: 模式，可以傳入"markers", "lines"繪製不同雷達圖

+ 舉例1: 散點
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建雷達散點圖
fig = go.Figure(go.Scatterpolar(
    r = [0.8,2,5.8,2,6,8,6,8,6.6,7.8],
    theta = [25, 50, 75, 100, 180, 200, 260, 280],
    mode = "markers"
))


## 更新介面
fig.update_layout(showlegend = True)

## 顯示圖像
fig.show()
```

**執行結果**

![5](images\5.PNG)





+ 舉例2: 線圖


```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建雷達散點圖
fig = go.Figure(go.Scatterpolar(
    r = [0.8,2,5.8,2,6,8,6,8,6.6,7.8],
    theta = [25, 50, 75, 100, 180, 200, 260, 280],
    mode = "lines"
))


## 更新介面
fig.update_layout(showlegend = True)

## 顯示圖像
fig.show()
```

**執行結果**

![6](images\6.PNG)





**更多雷達散點圖的寫法可以直接參考官網https://plotly.com/python/polar-chart/**









## 21. 雷達圖



+ 使用plotly.graph_objects.Scatterpolar()函數來達成
+ 參數介紹
1. r: 數值的大小，在雷達圖中用距離的方式呈現
2. theta: 雷達圖的維度
3. fill: 可以傳入'toself', 'tonext', 'none'，'toself': 就會將內部區域塗滿顏色

+ fig.update_layout()參數介紹
1. polar

     a. radialaxis: 控制雷達圖的裡面的刻度
     + visible: 傳入True/False，是否呈現刻度
     + color: 刻度字體顏色
     + range: 刻度的範圍
     

**更多的參數可以參考https://plotly.com/python/reference/scatterpolar/**

+ 舉例1

```Python
## 導入Plotly套件
import plotly.graph_objects as go


import pandas as pd
## 構建數據
df = pd.DataFrame(dict(

    r = [9,8,8,3,2],
    theta = ["Coding", "Math", "Exercise", "Science", "Chemical"]

))

## 構建雷達圖
fig = go.Figure(go.Scatterpolar(
    r = df['r'],
    theta = df['theta'],
    fill = 'toself'
))

## 更新介面
fig.update_layout(
    polar = dict(
        radialaxis = dict(visible = True, color = 'Blue')
    ),
    showlegend = True
)

## 顯示圖像
fig.show()
```

**執行結果**

![7](images\7.PNG)





+ 舉例2
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建數據
skills = ["Coding", "Math", "Exercise", "Science", "Chemical"]
r1 = [9,8,8,3,2]
r2 = [9,6,9,8,6]

## 繪製大圖
fig = go.Figure()

## 繪製子圖
fig.add_trace(go.Scatterpolar(
    r = r1,
    theta = skills,
    fill = 'toself',
    name = 'Person A'
))


fig.add_trace(go.Scatterpolar(
    r = r2,
    theta = skills,
    fill = 'toself',
    name = 'Person B'
))

## 更新介面
fig.update_layout(
    polar = dict(
        radialaxis = dict(visible = True, range = [0, 10])
    ),
    ##showlegend = False
    
)

## 顯示圖像
fig.show()
```

**執行結果**

![8](images\8.PNG)





## 22. 雷達柱狀圖



+ 使用plotly.graph_objects.Barpolar()函式來實現
+ 參數介紹
1. r: 數值的大小，在雷達圖中用距離的方式呈現
2. name: 柱狀圖的名稱
3. marker_color: 柱狀圖的顏色

+ fig.update_layout() 參數介紹
1. legend_font_size: 小圖列的字體大小
2. polar_radialaxis_ticksuffix: 刻度字體的後面單位
3. polar_angularaxis_rotation: 刻度字體的旋轉角度

+ 舉例

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建數據
skills = ["Coding", "Math", "Exercise", "Science", "Chemical"]
r1 = [98,80,80,30,20]
r2 = [90,67,96,80,69]
r3 = [60,20,90,27,80]
r4 = [99,40,20,80,93]


## 繪製大圖
fig = go.Figure()

## 繪製子圖
fig.add_trace(go.Barpolar(
    r = r1,
    theta = skills,
    name = 'Person A',
    marker_color = '#97CBFF'
))


fig.add_trace(go.Scatterpolar(
    r = r2,
    theta = skills,
    name = 'Person B',
    marker_color = '#66B3FF'
))

fig.add_trace(go.Scatterpolar(
    r = r3,
    theta = skills,
    name = 'Person C',
    marker_color = '#0080FF'
))

fig.add_trace(go.Scatterpolar(
    r = r4,
    theta = skills,
    name = 'Person D',
    marker_color = '#003060'
))

## 更新圖形樣式
fig.update_traces(text = ['C', 'M', 'E', 'S', 'CH'])


## 更新介面
fig.update_layout(
    title = 'Learning Skills',
    font_size = 20,
    legend_font_size = 16,
    polar_radialaxis_ticksuffix = '%',
    polar_angularaxis_rotation = 90
    
    
)



## 顯示圖像
fig.show()
```

**執行結果**

![9](images\9.PNG)





## 23. 3D圖



+ 使用plotly..graaph_objects.Scatter3d()函數來達成
+ 參數介紹
1. z: 3D圖會多一條z軸的值
2. mode: 模式，有'marker', 'lines', 'text', 兩兩組合或三個組合
3. marker: 調整點的樣式
+ size: 大小
+ color: 顏色
+ colorscale: 顏色條
+ opacity: 透明度


+ fig.update_layout()參數
1. margin: 設定邊界
    + l、r、b、t: 表示左、右、上、下，傳入一個數值

+ 舉例
```Python
## 導入Plotly套件
import plotly.graph_objs as go

import numpy as np

## 構建三個軸的值
v = np.linspace(0, 60, 100)

x, y, z = np.sin(v), np.cos(v), v

## 構建3D圖
fig = go.Figure(go.Scatter3d(
    x = x, y = y, z = z,
    mode = "markers",
    marker = dict(
        size = 12,
        color = z,
        colorscale = 'Plasma',
        opacity = 0.8
      
    )
))

## 更新介面
fig.update_layout(margin = (dict(r = 0, b = 0, l = 0, t = 0)))

## 顯示圖像
fig.show()
```

**執行結果**

![10](images\10.PNG)





大家辛苦了!! 到目前為止我們已經學了非常多的類型圖實作方法，當然官網上還有更豐富的圖形等著大家去學習學習













## Reference

https://www.youtube.com/watch?v=ifYugIP0pPQ
htts://plotly.com/python/
https://www.cnblogs.com/feffery/p/9293745.html
https://blogs.csdn.net/u012897374/article/details/77857980
https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf
https://www.jianshu.com/p/57bad75139ca










