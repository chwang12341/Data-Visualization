# 資料視覺化(Data Visualization) - Python 套件 - 互動式繪圖 - Plotly介紹與構圖介面的函數詳解 - Plotly筆記(ㄧ)



嗨嗨，相信大家想到Python視覺化套件，就會想到Matplotlib、Seaborn、Plotly、Bokeh等等，由於我過去要繪圖的時候都是採用Matpllotlib，所以想說來學點不一樣的，於是看了許多網路上寫得非常厲害的教學資源，整理了各高手的教學與自己實作後的心得和理解，並記錄下來成為筆記檔，介紹大家一個非常強大的視覺化框架 - Plotly，它真的非常華麗，繪出來的圖不但具有互動效果，還非常的絢麗



## 1. Plotly 是什麼?



+ Plotly是Python的一個非常厲害且開源的數據可視化框架，是一款基於D3.js框架的數據可視化庫
+ 生成的互動圖以web的形式呈現於瀏覽器上，這個特點讓它非常適用於 jupyter notebook 上進行程式開發
+ 可以透過在線模式或離線模式進行繪圖，同樣都具有畫各類型圖的能力


Plotly能繪製的圖:可以參考https://imagesplot.ly/plotly-documentation/images/python_cheat_sheet.pdf



## 2. 環境建置




**安裝Plotly套件**

```
pip install plotly
```

**更新指令**

```
pip install plotly -- upgrade

```





## 3. 模式介紹



使用plotly有兩種模式 - 在線(online)模式 與 離線(offline)模式，在我們使用plotly的時候，需要設定好模式，不然會報錯


+ 在線模式: 簡單來說就是可以連結plotly的線上資源庫，可以將local端繪製好的圖上傳到plotly網站上，就可以透過瀏覽器線上進行編輯或修改，也可以直接於plotly網站上進行資料視覺化，上傳的圖片可以根據自己的需求設置為公開(public)、私人(private)或秘密(secret)，這樣就能設置觀看權限

官網教學: https://plotly.com/chart-studio-help/tutorials/


+ 離線模式: 在 Local端進行plotly繪圖，沒有圖片的限制





## 4. 模式設定



### 1. 在線模式設定

先到官網(https://chart-studio.plotly.com/feed/#/)申請帳號 -> 登入，進到settings -> 點API Keys 裡面的 generate key -> 複製好key，貼到程式碼中

提醒: 接下來我的實作都會以離線模式進行，也因為這樣我就不詳細介紹在線模式的用法囉




+ 範例:


```Python
## 在線使用設置##############
import plotly
from plotly import tools

## 登入帳號資訊
plotly.tools.set_credentials_file(username = '帳號填這裡喔', api_key = '產生的密碼填這裡')

## 設置圖片為公開 - public
plotly.tools.set_config_file(world_readable = True, sharing = 'public')

## 繪圖舉例 #################
import plotly.plotly as py
import plotly.graph_objs as go


data1 = go.Scatter(
    x = [1, 2, 3, 4],
    y = [14, 18, 26, 28]
)




data2 = go.Scatter(
    x = [1, 4, 6, 8],
    y = [18, 26, 27, 29]
)

## 組合兩組數據
data = [data1, data2]

## 繪圖
## filename: 檔名
py.plot(data, filename = 'first_plotly', auto_open = True)

```



### 2. 離線模式設定

首先: 初始化(plotly.offline.init_notebook_model(connected = True))

再來: 繪圖 - 兩種方法
+ iplot()是屬於jupyter notebook的方法，它會將產出的圖片嵌入ipynb
+ plot()則是產出html格式的圖片檔，保存於當前目錄下，並自動地開啟



#### plot() 參數介紹

|參數|說明|
|---|---|
|(第一個參數)|傳入數據，可以是字典(Dict)型態，或是下面實作會介紹到用plotly.graph_objs.Figure的方法，結合介面設定與多筆資料，不用打上任何參數名(像是data = data1)，直接輸入數據即可|
|auto_open|傳入True/False，是否自動開啟html檔|
|filename|圖片下載的檔名，可以指定目錄路徑|
|image|圖片的下載格式: 'png', 'jpeg', 'svg', 'webp'，預設為None，沒有指定下載格式，也不會自動下載圖片|
|image_filename|html檔裡面保存為圖案的檔名，會自動下載到資料夾(下載)|
|image_height|設置圖片的高度像素，預設為600|
|image_width|設置圖片的寬度像素，預設為800|
|show_link|bool型態(True/False)|
|link_text|如果show_link = True，右下角顯示的plotly連結文字，預設為'Export to plotly'|
|validate|傳入True/False，驗證|



#### iplot() 參數介紹


|參數|說明|
|---|---|
|(第一個參數)|傳入數據，可以是字典(Dict)型態，或是下面實作會介紹到用plotly.graph_objs.Figure的方法，結合介面設定與多筆資料，不用打上任何參數名(像是data = data1)，直接輸入數據即可|
|filename|圖片下載的檔名，可以指定目錄路徑|
|image|圖片的下載格式: 'png', 'jpeg', 'svg', 'webp'，預設為None，沒有指定下載格式，也不會自動下載圖片|
|image_height|設置圖片的高度像素，預設為600|
|image_width|設置圖片的寬度像素，預設為800|
|show_link|bool型態(True/False)|
|link_text|如果show_link = True，右下角顯示的plotly連結文字，預設為'Export to plotly'|


```Python
import plotly.graph_objs as go
import plotly.offline as pof

## 設定為離線
pof.iplot([{'x': [1, 7, 8], 'y': [3, 8, 14]}], filename = 'My First Plotly', show_link = True, link_text = "Plotly Links", image = 'png', image_hight = 800, image_width = 900)

```



#### plot() 與 iplot() 的差別:



+ iplot()會直接在jupyter notebook生成圖，但plot()不會，它會自動開啟一個html檔
+ plot()可以在filename設定自動儲存的html檔的儲存路徑，iplot()則會自動儲存到特定資料夾(下載)
+ plot()需要在html檔裡面，再轉換下載成圖檔



**補充: 實作差別**
由於下面為了方便大家可以直接在jupyter notebook上檢視結果，所以都採用iplot()，但這邊我想讓大家看看它們實作上的差別，所以我透過繪製一樣的圖，但是採用不同的呈現與下載方式來讓大家看出它們的差別




+ iplot()

```Python
import plotly.graph_objs as go
import plotly.offline as pof

## 設置為離線
pof.init_notebook_mode(connected = True)

## 繪圖
pof.iplot([{'x': [1, 7, 8], 'y': [3, 8, 14]}], filename = 'result/My First Plotly', show_link = True, link_text = "Plotly Links", image = 'png', image_height = 800, image_width = 900)
```

**執行結果**

![1](images\1.PNG)








+ plot()

```Python
import plotly.graph_objs as go
import plotly.offline as pof

## 設置為離線
pof.init_notebook_mode(connected = True)

## 繪圖
pof.iplot([{'x': [1, 7, 8], 'y': [3, 8, 14]}], filename = 'result/My First Plotly', show_link = True, link_text = "Plotly Links", image = 'png', image_height = 800, image_width = 900, validate = False)
```

**執行結果**

![2](images\2.PNG)



大家可以直接複製上面的程式碼到jupyter notebook實作感受一下差別

瞭解怎麼設定模式後，我們就要開始學習用plotly繪圖囉





## 5. 可視化圖形介紹



這邊我們來檢視一下Plotly能夠繪製的圖形類別，使用help(plotly.graph_objs)來查詢套件底下的資訊

```Python
import plotly

help(plotly.graph_objs)
```

**執行結果**

```
IOPub data rate exceeded.
The notebook server will temporarily stop sending output
to the client in order to avoid crashing it.
To change this limit, set the config variable
`--NotebookApp.iopub_data_rate_limit`.

Current values:
NotebookApp.iopub_data_rate_limit=1000000.0 (bytes/sec)
NotebookApp.rate_limit_window=3.0 (secs)
```





由於我的電腦沒辦法呈現出來，所以大家可以直接參考這個網址喔:https://plotly.com/python/



## 6. 圖形數據與介面設定函數L: Layout() & Figure()

### plotly.graph_objs.Layout()參數
用於繪製圖形介面

|參數|說明|
|---|---|
|title|圖形介面名稱，會顯示於左上角的位置|
|xaxis|x軸設定，以字典格式傳入，像是{'title': 'x軸名稱'}，來設定x軸名稱|
|yaxis|y軸設定，以字典格式傳入，像是{'title': 'y軸名稱'}，來設定y軸名稱|
|font|設定字體|
|showlegend|傳入True/False，設定是否繪製小圖，用以顯示圖片資料資訊|
|legend|設定繪製小圖，用以顯示圖片資料的資訊|
|width|設定圖片的像素寬度，預設為700|
|height|設定圖片的像素長度，預設為450|
|margin|設置圖片邊邊的距離寬度(傳入字典(Dict)格式)|
|paper_bgcolor|設置圖片背後的背景顏色|
|plot_bgcolor|設定繪製圖片區域的顏色|
|hidesource|設定是否顯示右下角的Plotly連結|
|hovermode|可以傳入'x','y','closet'或False，用來控制鼠標移動於數據上的互動呈現效果|
|hoverlabel|設定鼠標移動到數據上時，所呈現的數據資訊小圖的各種屬性操作，傳入字典格式|
|grid|設定當一張圖裡有很多子圖(subplots)時，網格的屬性資料|
|bargap|直方圖之間的距離，傳入值ru介於0~1之間|
|barmode|傳入'stack'或'group'，'stack'讓柱狀圖成為堆疊柱狀圖，而'group'會轉成群組柱狀圖|
|mapbox_style|變換地圖樣式，這是針對繪製地圖的方法，可以傳入的值，目前官方有'open-street-map', 'carto-positron', 'carto-darkmatter', 'stamen-terran' or 'stamen-watercolor'可以提供使用者免費做選擇|







+ xaxis 和 yaxis 裡面可用的參數

|參數|說明|
|---|---|
|color|設定座標上所有元素的顏色|
|title|設定軸的名稱|
|titlefont|設定字體大小、字形、樣式等等|
|type|設定坐標軸類型，'-':表示根據數據類型自動調整，'linear': 表示線性坐標，'log': 表示對數坐標，'date': 表示日期坐標，'categoy'表示類別坐標，預設為'-'|
|autorange|填入True/False，預設為True，表示會根據數據自動調整軸的範圍|
|range|傳入串列(list)，ex.[右邊值, 左邊值]，預設為根據資料自動調整軸的範圍|
|tickmode|設定座標軸刻度的格式，'auto': 表示根據數據自動調整，'linear':線性型態，'array': 根據自行定義的數組來呈現|
|tickvals|前提是tickmode = 'array'，自行定義軸的刻度名稱，傳入的格式可以有list, numpy.array 或 series|
|ticks|設定刻度文字標籤的位置，'outside': 位於外側呈現標籤，'inside': 位於內側呈現標籤|
|ticklen|設定刻度的標籤像素長度|
|tickwidth|設定刻度的標籤像素寬度|
|tickcolor|設定刻度的標籤顏色|
|tickfont|設定刻度的標籤字體大小、字型、樣式等等|
|tickangle|設定刻度標籤的旋轉角度|
|showline|設定是否繪出該座標的直線|
|linecolor|設定坐標軸線的顏色|
|linewidth|設定坐標軸線的像素寬度|
|showgrid|傳入True/False，設定否顯示網格|
|gridcolor|設置網格線的顏色|
|gridwidth|設定網格線的像素寬度|
|zeroline|傳入True/False，設定是否在0值上繪出0刻度線|
|side|設定x、y軸上的圖形位置，有'top'、'bottom'、'left'、'right'，來設置在圖的上下左右位置|


+ font 裡面的可用參數

|參數|說明|
|---|---|
|family|字體，預設是'Open Sans'，可以使用fantasy, cursive 或 serif等，其他可以參考這個網站https://www.oxxostudio.tw/articles/201811/css-font-family.html|
|size|字體大小|
|color|顏色|

+ legend 裡面可用的參數


|參數|說明|
|---|---|
|bgcolor|設置小圖的背景顏色|
|bordercolor|設置小圖的邊框顏色|
|font|傳入字典(格式)，用來設定字型、字體大小、顏色等|
|orientation|設置小圖中各元素的堆疊方式，'v': 表示垂直堆疊，'h': 表示水平堆疊|
|x|傳入數值，需要介於-2~3的數，用來設定小圖在水平方向的位置，預設是1.02|
|xanchor|也是用於設置小圖在水平方向的位置，但是有固定的位置，可以傳入的值為'left', 'center', 'right'和'auto'這些固定的位置|
|y|傳入數值，需要介於-2~3的數，用來設定小圖在垂直方向的位置，預設是1|
|yanchor|也是用於設置小圖在垂直方向的位置，但是有固定的位置，可以傳入的值為'top', 'middle', 'bottom'和'auto'這些固定的位置|


+ margin裡面的參數


|參數|說明|
|---|---|
|I|設置圖片與左邊邊界的寬度像素，預設是80|
|r|設置圖片與右邊邊界的寬度像素，預設是80|
|t|設置圖片與上邊邊界的寬度像素，預設是100|
|b|設置圖片與下邊邊界的寬度像素，預設是80|
|pad|設定座標軸與圖片的像素距離，預設為0|


+ hoverlabel裡面的參數


|參數|說明|
|---|---|
|bgcolor|設置數據資訊小圖的背景顏色|
|bordercolor|設定數據資訊小圖的邊框顏色|
|namelength|設置數據資訊小圖的文字長度限制，-1是全部顯示，預設為15，表示呈現前15個字|
|font|設定字體大小、字型、字體顏色等|


+ grid裡面的參數

|參數|說明|
|---|---|
|rows|控制劃分的網格行數|
|columns|控制劃分的網格列數|
|roworder|設置子圖是由上往下堆疊，還是下往上堆疊，可以傳入'top to bottom'或'bottom to top'，預設為'top to bottom'，只能設置上下堆疊子圖，不能設定左右，因為一定是左至右|
|pattern|設定子圖是否共用x軸與y軸，可以傳入'coupled':表示共用x軸與y軸，或'independent':表示子圖的x軸與y軸獨立|
|xgap|傳入值介於0.0~1.0，設置子圖之間的水平距離佔子圖寬度值的百分比|
|ygap|傳入值介於0.0~1.0，設置子圖之間的垂直距離佔子圖寬度值的百分比|
|domain|設定子圖上下左右邊界的距離(傳入字典(Dict)形式)|

+ x: 傳入list形式，格式: [距離左邊邊界的距離百分比, 距離右邊邊界的距離百分比]，距離是以百分比方式呈現，介於0.0~0.1之間，也就是子圖區域與邊界距離子圖寬度(子圖與邊界間空白的地方)的百分比

+ y: 傳入list形式，格式: [距離上邊邊界的距離百分比, 距離下邊邊界的距離百分比]，距離是以百分比方式呈現，介於0.0~0.1之間，也就是子圖區域與邊界距離子圖寬度(子圖與邊界間空白的地方)的百分比





### plotly.graph_objs.Figure()裡面的參數設定

用以結合多筆數據和圖形介面

|參數|說明|
|---|---|
|data|將透過設定好的數據傳入(ex. data1 = plotly.graph_objs.Scatter(...))，可以同時傳入多筆，像是data = [data1, data2, data3, data4]，就能一次呈現多筆資料於同一張圖上|
|layout|圖形介面，將plotly.graph_objs.Layout()設定好的介面傳入即可|





這篇主要帶大家瞭解Plotly和要使用它來建構視覺圖需要的各種參數，下一篇就會馬上帶大家來用散點圖來實作並用各種參數來調整圖形喔





## Reference

https://www.youtube.com/watch?v=ifYugIP0pPQ
htts://plotly.com/python/
https://www.cnblogs.com/feffery/p/9293745.html
https://blogs.csdn.net/u012897374/article/details/77857980
https://images.plot.ly/plotly-documentation/images/python_cheat_sheet.pdf
https://www.jianshu.com/p/57bad75139ca




