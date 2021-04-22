# 資料視覺化(Data Visualization) - Python 套件 - 互動式繪圖 - 各種類型圖的繪製 - Plotly筆記(四)





那就讓我們繼續學習Plotly吧!!





## 7. 3D圖



+ 使用plotly.graph_objs.Scatter3d()來繪製3D圖，並且在函式中增加一個維度(一軸: z 軸)的值
+ 數據集: 這邊我想使用鼎鼎大名的iris dataset的來繪製3D圖，並取其中的SepalLength、PetalLength與PetalWidth來當三個軸

```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go

import pandas as pd

## 設定為離線
pof.init_notebook_mode(connected = True)

## 導入iris 數據集
iris_data = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/iris.csv")

## 查看Name欄位有哪些標籤
print(iris_data.groupby('Name').count().index.values)

## 構建一個字典，使Name裡的類別對應數值
label_trans_color = {
    'Iris-setosa': 0,
    'Iris-versicolor': 1,
    'Iris-virginica': 2
}

## 增加一列，目的是根據Name來分辨顏色
iris_data['color'] = iris_data['Name'].map(label_trans_color)

## 構建3D圖
data = go.Scatter3d(
    x = iris_data['SepalLength'],
    y = iris_data['PetalLength'],
    z = iris_data['PetalWidth'],
    mode = 'markers',
    marker = {
        'color': iris_data['color'],
        'size': 6
    }
)

## 介面
layout = go.Layout(
    title = 'Iris',
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 10,
        'family': 'fantasy',
        'color': '#D9006C'
        
    }
    
)


## 放入Figure
figure = go.Figure(data = data, layout = layout)

## 繪圖
pof.iplot(figure)
```

**執行結果**

![1](images\1.PNG)





## 8. 3D的面 plotly.graph_objs.Surface()



+ 使用plotly.graph_objs.Surface()，其填入參數x, y, z軸的值
+ 舉例1: 用mt_bruno_elevation數據集來繪圖，它是裝載海拔資訊的數據集，x軸與y軸分別對應數據集中最上面的columns與最左邊的index，裡面的數值為對應x與y的z軸值


```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go

import pandas as pd

## 設定為離線
pof.init_notebook_mode(connected = True)

## 導入數據集
surface_data = pd.read_csv('https://raw.githubusercontent.com/plotly/datasets/master/api_docs/mt_bruno_elevation.csv')


## 移除掉index欄位
# surface_data = surface_data.drop(['index'], axis = 1)

## 獲取z軸值
height = surface_data.values

## 構建3D平面圖
data = go.Surface(z = height)

## 介面
layout = go.Layout(
    title = 'Iris',
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 10,
        'family': 'fantasy',
        'color': '#D9006C'
        
    }
    
)


## 放入Figure
figure = go.Figure(data = data, layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Surface', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)

```

**執行結果**

![2](images\2.PNG)





+ 舉例2: 客制化一個自己的3D平面

```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go

import pandas as pd
import numpy as np

## 設定為離線
pof.init_notebook_mode(connected = True)

## 構建數據集
x = np.arange(-6, 7)
y = np.arange(-6, 7)

## 組成np.array
xv, yv = np.meshgrid(x, y)
print('xv: ', xv)
print('yv: ', yv)

## 計算z軸值
z = xv**2 + yv**4
print('z: ', z)


## 構建3D平面圖
data = go.Surface(x = xv, y = yv, z = height)

## 介面
layout = go.Layout(
    title = 'Iris',
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 10,
        'family': 'fantasy',
        'color': '#D9006C'
        
    }
    
)


## 放入Figure
figure = go.Figure(data = data, layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Surface', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)

```

**執行結果**

```

xv:  [[-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]
 [-6 -5 -4 -3 -2 -1  0  1  2  3  4  5  6]]
yv:  [[-6 -6 -6 -6 -6 -6 -6 -6 -6 -6 -6 -6 -6]
 [-5 -5 -5 -5 -5 -5 -5 -5 -5 -5 -5 -5 -5]
 [-4 -4 -4 -4 -4 -4 -4 -4 -4 -4 -4 -4 -4]
 [-3 -3 -3 -3 -3 -3 -3 -3 -3 -3 -3 -3 -3]
 [-2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2]
 [-1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1]
 [ 0  0  0  0  0  0  0  0  0  0  0  0  0]
 [ 1  1  1  1  1  1  1  1  1  1  1  1  1]
 [ 2  2  2  2  2  2  2  2  2  2  2  2  2]
 [ 3  3  3  3  3  3  3  3  3  3  3  3  3]
 [ 4  4  4  4  4  4  4  4  4  4  4  4  4]
 [ 5  5  5  5  5  5  5  5  5  5  5  5  5]
 [ 6  6  6  6  6  6  6  6  6  6  6  6  6]]
z:  [[1332 1321 1312 1305 1300 1297 1296 1297 1300 1305 1312 1321 1332]
 [ 661  650  641  634  629  626  625  626  629  634  641  650  661]
 [ 292  281  272  265  260  257  256  257  260  265  272  281  292]
 [ 117  106   97   90   85   82   81   82   85   90   97  106  117]
 [  52   41   32   25   20   17   16   17   20   25   32   41   52]
 [  37   26   17   10    5    2    1    2    5   10   17   26   37]
 [  36   25   16    9    4    1    0    1    4    9   16   25   36]
 [  37   26   17   10    5    2    1    2    5   10   17   26   37]
 [  52   41   32   25   20   17   16   17   20   25   32   41   52]
 [ 117  106   97   90   85   82   81   82   85   90   97  106  117]
 [ 292  281  272  265  260  257  256  257  260  265  272  281  292]
 [ 661  650  641  634  629  626  625  626  629  634  641  650  661]
 [1332 1321 1312 1305 1300 1297 1296 1297 1300 1305 1312 1321 1332]]
```



圖片我的電腦突然跑不太出來，所以就不貼了 喔







## 9. 地圖 plotly.graph_objs.Densitymapbox()



+ 使用plotly.graph_objs.Densitymapbox()來繪製地圖，需要傳入的參數lat: 經度，lon: 緯度，z: 傳入欲觀察的數值，radius: 調整圖片中顏色區域的半徑值
+ 補充: 如果想要變換地圖樣式，可以調整mapbox_style的值，官網目前有'open-street-map', 'carto-positron', 'carto-darkmatter','stamen-terrain', 'stamen_toner' or 'stamen-watercolor'免費的選項 - 參考官網 https://plotly.com/python/mapbox-layers/
+ 小提醒: 一定要加入mapbox_style這個參數到Layout()中，地圖才會呈現喔
+ 舉例: 使用地震的數據來呈現


```Python
## 導入套件
import plotly
import plotly.offline as pof
import plotly.graph_objs as go

import pandas as pd
import numpy as np


## 設定為離線
pof.init_notebook_mode(connected = True)

## 載入數據集
earthquakes_data = pd.read_csv('https://raw.githubusercontent.com/plotly/datasets/master/earthquakes-23k.csv')


## 構建地圖
data = go.Densitymapbox(
    lat = earthquakes_data['Latitude'],
    lon = earthquakes_data['Longitude'],
    z = earthquakes_data['Magnitude'],
    radius = 2    
)

## 介面
layout = go.Layout(
    title = 'Earthquakes',
    plot_bgcolor = "#B9B9FF",
    paper_bgcolor = "#ACD6FF",
    font = {
        'size': 10,
        'family': 'fantasy',
        'color': '#D9006C'
        
    },
    mapbox_style = "carto-positron"
    
)


## 放入Figure
figure = go.Figure(data = data, layout = layout)

## 繪圖
pof.iplot(figure, filename = 'Plotly-Surface', show_link = True, link_text = "Plotly Links", image_height = 800, image_width = 900)

```

**執行結果**

![3](images\3.PNG)













## 10. 補充: 折線圖的變化: connectgaps 與 line_shape實作



+ connectgaps應用: 傳入True/False

```Python
## 導入Plotly套件
import plotly.graph_objects as go

## 構建數據
x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]


## 構建大圖
fig = go.Figure()

## 繪製圖形1
fig.add_trace(
    go.Scatter(
        x = x,
        y = [5, 10, None, 20, 14, None, 6, 10,9, 18],
        name = '<b>No</b> Gaps between the data',
        connectgaps = True
        
        
    ))
    
    
## 繪製圖形2
fig.add_trace(
    go.Scatter(
        x = x,
        y = [2, 6, 8, None, 12, 10, 4, 6, None, 7, 16],
        name = 'Gaps between the data', 
    ))
    
## 顯示圖像
fig.show()
```

**執行結果**

![4](images\4.PNG)






+ line_shape應用: 線的形狀樣式，可以傳入'linear', 'spline', 'hv', "vh", "vhv", "hvh"，我會用下面的舉例來大家瞭解他們的差別


```Python
## 導入Plotly套件
import plotly.graph_objects as go

import numpy as np

## 創建數據集
x = np.array([0, 1, 2, 4, 5, 6])
y = np.array([2, 6, 8, 4, 3, 7, 14])

## 構建大圖
fig = go.Figure()

## 繪製圖形1
fig.add_trace(go.Scatter(x = x, y = y, line_shape = 'linear', name = 'Linear'))

## 繪製圖形2
fig.add_trace(go.Scatter(x = x, y = y + 5, line_shape = 'spline', name = 'Spline', text = ["<b>Spline</b><br>spline makes the line more smoothly"], hoverinfo = "text+name"))


## 繪製圖形3
fig.add_trace(go.Scatter(x = x, y = y + 10, line_shape = 'vh', name = 'vh', text = ["Vertical first, then Horizontal"], hoverinfo = "text+name"))


## 繪製圖形4
fig.add_trace(go.Scatter(x = x, y = y + 15, line_shape = 'hv', name = 'hv', text = ["Horizontal first, then Vertical"], hoverinfo = "text+name"))

## 繪製圖形5
fig.add_trace(go.Scatter(x = x, y = y + 20, line_shape = 'hvh', name = 'hvh'))


## 繪製圖形6
fig.add_trace(go.Scatter(x = x, y = y + 25, line_shape = 'vhv', name = 'vhv'))

## 顯示圖像
fig.show()
```

**執行結果**

![5](C:\Users\user\Desktop\Data-Visualization\Plotly\4\images\5.PNG)





**更多小提琴圖的寫法可以參考官網https://plotly.com/python/violin/**

當然除了這些還有更多類型的圖，大家有興趣可以直接參考官網https://plotl.com/python/教學喔，也可以跟著我在之後的文章中一起繼續學習





## Reference

https://www.youtube.com/watch?v=ifYugIP0pPQ
htts://plotly.com/python/
https://www.cnblogs.com/feffery/p/9293745.html
https://blogs.csdn.net/u012897374/article/details/77857980
https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf
https://www.jianshu.com/p/57bad75139ca







# 資料視覺化(Data Visualization) - Python 套件 - 互動式繪圖 - 各種類型圖的繪製 - Plotly筆記(五)



大家一起繼續努力!!





## 11. 面積圖



+ 面積圖採用的方式與散點圖一樣，但要多設定一個參數fill，接下來的Plotly Express教學文中，我會教大家怎麼使用別的函數來畫面積圖，雖然Express可以快速繪圖，但沒有Plotly來得彈性，詳細的區別可以參考官網https://plotly.com/python/filled-area-plots/
+ 參數介紹

1. fill: 填滿面積，傳入"tozeroy":往下填滿至X軸，"tonexty":與另外一條數據之間的垂直空隙填滿，"tozerox": 往左邊填滿至Y軸，"tonextx":與另外一條數據之間的水平空隙補滿
2. mode: "none":把邊界的線與點拿掉，以前我們常用到的是mode = "lines", "markers", "text"與它們的組合等，這邊則要使用的是把邊界的線與點拿掉的方法

+ 舉例1: 一般的面積圖
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建大圖
fig = go.Figure()

## 創建數據
x = [0, 1, 2, 3, 4, 5, 6]

## 堆疊數據圖

## 圖1
fig.add_trace(go.Scatter(x = x, y = [4, 6, 8, 3, 9, 10, 18], fill = 'tonexty'))

## 圖2
fig.add_trace(go.Scatter(x = x, y = [1, 3, 2, 6, 4, 8, 16], fill = 'tozeroy'))

## 顯示圖像
fig.show()
```

**執行結果**

![6](images\6.PNG)





+ 舉例2: 去掉邊界線與點的圖


```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建大圖
fig = go.Figure()

## 創建數據
x = [0, 1, 2, 3, 4, 5, 6]

## 堆疊數據圖

## 圖1
fig.add_trace(go.Scatter(x = x, y = [4, 6, 8, 3, 9, 10, 18], fill = 'tonexty', mode = 'none'))

## 圖2
fig.add_trace(go.Scatter(x = x, y = [1, 3, 2, 6, 4, 8, 16], fill = 'tozeroy', mode = 'none'))

## 顯示圖像
fig.show()
```
**執行結果**

![7](images\7.PNG)





+ 舉例3: 只留交集地方的面積圖


```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建大圖
fig = go.Figure()

## 創建數據
x = [0, 1, 2, 3, 4, 5, 6]

## 堆疊數據圖

## 圖1
fig.add_trace(go.Scatter(x = x, y = [4, 6, 8, 3, 9, 10, 18], fill = None, mode = 'lines', line_color = 'blue'))

## 圖2
fig.add_trace(go.Scatter(x = x, y = [1, 3, 2, 6, 4, 8, 16], fill = 'tonexty', mode = 'lines', line_color = 'blue'))

## 顯示圖像
fig.show()
```

**執行結果**

![8](images\8.PNG)





## 12. 熱力圖 plotly.graph_objs.Histogram2d()



+ 也可以稱為2D直方圖(Histograns)，使用plotly.graph_objs.Histogram2d()來實作
+ 參數介紹

1. histfunc: 設定圖的每個階段(一格一格的)使用的計算方法，預設是"count"，有"sum": 總和、"avg": 求平均值、"min": 最小值、"max": 最大值
2. histnorm: 設定每個階段統計結果的呈現方式，預設是不給值，代表計算數量(count)，有'percent': 百分比、'probability': 佔比、'density': 密度
3. nbinsx與nbinsy: 設定每個階段的數量，不設定的話會自行分配多少個階段
4. xbins與ybins: 設定x軸或y軸的階段數量，如果有此參數，nbinx與nbinsy就不會執行，也就是它有較高的優先權，傳入字典(Dict)格式，可以設定起始與結束，和每個階段的大小，像是{'start': 0, 'end': 200, 'size': 40}，這樣的話就會有五個階段，而其中每個階段為40
5. bingroup: 設定每個階段的分組，以字串形式傳入


+ 舉例1


```Python
## 導入所需套件
import plotly.graph_objs as go

import numpy as np

## 構建數據集
x = np.random.randn(600)
y = np.random.randn(600) + 2

## 創建熱力圖
heatmap = go.Histogram2d(
    x = x, y = y, histnorm = 'density',
    autobinx = False,
    xbins = {'start': '-4', 'end': '5', 'size': '0.2'},
    autobiny = False,
    ybins = {'size':'-3', 'end': '6', 'size': '0.2'},
    colorscale = [[0, 'rgb(253,240,118)'], [0.2, 'rgb(90, 204, 246)'], [0.4, 'rgb(13,106,212)'], [0.6, 'rgb(74,68,250)'], [0.8, 'rgb(196,5,171)'], [1, 'rgb(255,0,0)']]
    )

fig = go.Figure(heatmap)

## 顯示熱力圖
fig.show()
```

**執行結果**

![9](images\9.PNG)









+ 舉例2: 調整xbins與ybins來看差別

```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 導入繪製子圖的套件
from plotly.subplots import make_subplots

## 建立子圖大小
fig = make_subplots(2, 2)

## 創建數據

## 第一組
x, y = [0,1,2,3,4], [0,1,2,3,4]

## 第二組
x1, y1 = [5,6,7,8,9], [5,6,7,8,9]

## 構建子圖
fig.add_trace(go.Histogram2d(
    x = x,
    y = y,
    #coloraxis = "coloraxis",
    xbins = {'start': 1, 'size': 1}
), row = 1, col = 1)


fig.add_trace(go.Histogram2d(
    x = x,
    y = y,
    #coloraxis = "coloraxis",
    xbins = {'start': 2, 'size': 2}
), row = 1, col = 2)


fig.add_trace(go.Histogram2d(
    x = x1,
    y = y1,
    #coloraxis = "coloraxis",
    xbins = {'start': 1, 'size': 1}
), row = 2, col = 1)


fig.add_trace(go.Histogram2d(
    x = x1,
    y = y1,
    #coloraxis = "coloraxis",
    xbins = {'start': 2, 'size': 2}
), row = 2, col = 2)

## 顯示圖像
fig.show()
```

**執行結果**

![10](images\10.PNG)





**更多關於熱力圖的寫法可以參考官網https://plotly.com/python/2D-Histogram/喔**







## 13. 圖像與熱力圖



+ 使用plotly.graph_objs.Image()函式來達成，傳入array來繪圖
+ 參數介紹
1. z: 將rgb的array傳入
2. zmin: 可以傳入一個整數，或三個數據串列(rgb的三個值)，來限制圖片rgb色彩的最小值
3. zmax: 可以傳入一個整數，或三個數據的串列(rgb的三個值)，來限制圖片rgb色彩的最大值



+ 舉例: 用rgb數據集來繪圖
```Python
## 導入Plotly套件
import plotly.graph_objs as go

## 構建數據
img_rgb = np.array([
    [[9,116,223], [223,9,109], [223,80,9]],
    [[223,80,9], [9,116,223], [223,9,109]]
])

## 構建圖像
fig = go.Figure(go.Image(z = img_rgb))

## 顯示圖像
fig.show()
```

**執行結果**

![11](images\11.PNG)





**更多圖像與熱力圖寫法可以參考官網https://plotly.com/python/imshow/**







## 14. 小提琴圖 plotly.graph_objs.Violin()



+ 使用plotly.graph_objs.Violin()函數來實現
+ 參數介紹
1. box_visible: 傳入True/False，是否呈現小提琴裡的箱型圖
2. line_color: 線條的顏色
3. meanline_visible: 傳入True/False，是否繪製平均線
4. fillcolor: 小提琴圖的顏色
5. opacity: 透明度

+ 舉例

```Python
## 導入Plotly套件
import plotly.graph_objs as go


import pandas as pd
## 載入數據集
df = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/violin_data.csv")

## 構建小提琴圖
data = go.Violin(x = df['sex'], y = df['total_bill'],
    box_visible = True, line_color = 'red',
    meanline_visible = True, fillcolor = '#9999FF', opacity = 0.6
)

fig = go.Figure(data = data)

## 顯示小提琴圖
fig.show()
```

**執行結果**

![12](images\12.PNG)





**更多小提琴圖的寫法可以參考官網https://plotly.com/python/violin/**


當然除了這些還有更多類型的圖，大家有興趣可以直接參考官網https://plotl.com/python/教學喔，也可以跟著我在之後的文章中一起繼續學習





## Reference

https://www.youtube.com/watch?v=ifYugIP0pPQ
htts://plotly.com/python/
https://www.cnblogs.com/feffery/p/9293745.html
https://blogs.csdn.net/u012897374/article/details/77857980
https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf
https://www.jianshu.com/p/57bad75139ca