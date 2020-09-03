# Data Visualization資料視覺化- Python -Plotly進階 - 強大的Dash教學 



哈囉哈囉，今天想跟大家介紹一個非常厲害的視覺化web框架-Dash，之前有跟大家介紹過Plotly，簡單來說就是超強大的Python視覺化套件，而Dash就是基於這樣的視覺化能力搭配更多樣的互動功能，並呈現於網頁上，讓使用者可以在網頁上操作多樣的互動功能，不管是想更了解數據的狀況，還是要跟主管或朋友講述自己的分析結果，都是非常非常方便且強大的利器



## Dash是什麼?

+ 簡單來說，它就是可以讓我們在網頁上呈現我們的數據視覺化結果，並搭配著網頁前端的功能，使其更具互動性與美觀

+ 它是一個flask輕量級的web框架，圖形的繪製使用的是Plotly，而前端的網頁是由React與html來進行功能與架構的撰寫
  + html: 為標準標示語言，用標籤(tag)來建立網頁架構
  + flask: 建立輕量型的Web框架
  + Plotly: 繪製互動視覺化圖，這邊我都會使用Python來撰寫喔!!



## 環境建置

安裝: 我這邊是使用Anaconda Prompt進行安裝指令喔

```
## Dash核心後端
pip install dash
## Dash核心前端
pip install dash-renderer
## HTML組件
pip install dash-html-components
## Dash核心組件
pip install dash-core-components
## Dash 圖形套件
pip install plotly
```



## Dash 實作的基本架構

1. **Step 1:** 導入Dash所需的套件

   + dash.layout: 佈局整個網頁架構
   + dash_core_components:  操作React模組，控制網頁元素的功能
   + dash_html_components:  操作HTML語法，設定網頁標籤(tag)架構
   + dash_dependencies: Callback用來傳遞資料

   **示意圖:**

   ![image1](images\image1.png)

2. Step 2: 啟用Dash函式

3. Step 3: 設定網頁介面

   + 小技巧: 如果不知道html有哪些tag可以選擇(像是範例中的html.Div)，可以在打完html後按一下tab鍵，jupyter notebook![image2](images\image2.png)中就會跑出所有選項供選擇

     

4. Step 4: 在介面裡面繪製圖形或實作其他功能，使用dash_core_components套件來達成

   

5. Step 5: 啟動Local Server

   + run_server()裡面的參數，可以使用help()來查看參數解釋或查詢官網(https://dash.plotly.com/devtools)

     ```
     run_server(host='127.0.0.1', port='8050', proxy=None, debug=False, dev_tools_ui=None, dev_tools_props_check=None, dev_tools_serve_dev_bundles=None, dev_tools_hot_reload=None, dev_tools_hot_reload_interval=None, dev_tools_hot_reload_watch_interval=None, dev_tools_hot_reload_max_retry=None, dev_tools_silence_routes_logging=None, dev_tools_prune_errors=None, **flask_run_options) 
     ```

     

**程式碼架構**

```Python
## 導入dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動dash
app = dash.Dash()

## 設定網頁介面
app.layout = html.Div([
    
    ## 把要遷入於HTML中的內容寫在這
    
   ## 功能操作 
   ## 繪圖
    dcc.Graph(
    
    )
    
])


## 啟動Local Server
if __name__ == '__main__':
    ## port: 設定開出來的網路窗口，通常會有預設的，可以透過help()查看
    ## debug = True: 打開所有開發工具
    app.run_server(port = 5000, debug = False)
```







## 實作



#### 實作 1: 用Dash開啟Local Server，建立基本頁面

+ 這邊我將實作用dash開啟Local Server網頁，然後用html套件來寫入一些內容

```Python
## 導入dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動dash
app = dash.Dash()

## 設定網頁介面
app.layout = html.Div([
    ## 設定一個h1標籤(tag)，寫下標題
    html.H1("哈囉~~ Dash!!!!"),
    html.Div('Plotly 網頁框架 - Dash應用'),
    html.P('HTML tag(p)應用')
    
])


## 啟動Local Server
if __name__ == '__main__':
    ## port: 設定開出來的網路窗口，通常會有預設的，可以透過help()查看
    ## debug = True: 打開所有開發工具
    app.run_server(port = 5000, debug = False)

```



![image3](images\image3.PNG)





#### 實作 2: 繪製柱狀圖、改變字體樣式、繪圖和改變介面樣式

+ 改變字體樣式: 在欲改變樣式的html套件屬性函式裡，設定style來實現
+ 這邊跟大家介紹 dash_core_components 套件
  + dash_core_components: 控制網頁上的功能，像是繪圖、製作下拉選單與滑桿等等，它擁有哪些屬性，可以參考https://dash.plotly.com/dash-core-components，或使用tab快捷鍵查詢可以用的函式，然後用help()查看函式的參數細節

+ 在介面中繪圖與改變介面樣式: dash_core_components.Graph()中的figure參數，而figure需要數據(data)與介面(layout)來構建，layout就是我們要改變介面樣式的參數:
  + layout: 傳入字典(dict)格式
    + 'title': 介面標題名稱
    + 'plot_bgcolor': 圖表的背景顏色
    + 'paper_bgcolor': 圖形後面的背景顏色
    + 'font'; 設定字體樣式

+ 程式碼: 這邊我想繪製一個Bar圖，我要使用的是dash_core_components套件裡的Graph()

  ```Python
  ## 導入dash套件
  import dash
  import dash_core_components as dcc
  import dash_html_components as html
  
  ## 啟動dash
  app = dash.Dash()
  
  
  ## 介面設定
  app.layout = html.Div(children = [
      html.H1(children = "哈囉~~ Dash!!!!",
             style = {
                 'textAlign': 'center',
                 'color': '#46A3FF'
             }
             ),
      html.Div('Plotly 網頁框架 - Dash 的應用',
              style = dict(textAlign = 'center', color = 'blue')
              ),
      html.P('HTML tag(p)應用'),
      
      dcc.Graph(
          id = 'Dash Chart',
          ## figure 是由data與layout所構成
          figure = {
              'data' : [
                  {'x': [6,8,14,18,28,30,36], 'y': [2,6,6,8,20,18,38], 'type': 'bar', 'name': 'Dash Bar Chart1'},
                  {'x': [7,8,16,18,26,30,36], 'y': [4,8,10,18,28,28,36], 'type':'bar', 'name': 'Dash Bar Chart2'}
              ],
              'layout': {
                  'plot_bgcolor': '#66B3FF',
                  'paper_bgcolor': '#D8D8EB',
                  'font': dict(color = '#484891'),
                  'title': 'Dash Bar Plots'
              }
          }
      )
  ])
  
  
  if __name__ == '__main__':
      app.run_server()
  ```

  

![image4](images\image4.PNG)





#### 實作 3: 結合Plotly來繪製基本圖形

+ 這邊跟大家介紹Plotly套件: plotly.graph_objects，它和dash_core_components部署於Python的封裝套件
  + plotly.graph_objects: Plotly繪圖的套件，如果不知道它的寫法，可以參考我寫的Plotly文章

+ 程式碼 1: 這邊我想繪製一個Bar圖，我要使用的是dash_core_components套件裡的Graph()與plotly.graph_objects裡的Bar()來實作

  ```Python
  ## 導入dash套件
  import dash
  import dash_core_components as dcc
  import dash_html_components as html
  ## Plotly繪圖套件
  import plotly.graph_objects as go
  
  
  ## 啟動dash
  app = dash.Dash()
  
  ## 設定網頁介面
  app.layout = html.Div([
      html.H1("哈囉~~ Dash!!!!"),
      html.Div('Plotly 網頁架構 - Dash的應用'),
      html.P('HTML tag(p) 應用'),
      
      
      dcc.Graph(
          id = 'First Dash Graph',
          ## Plotly的 Figure是由data與graph構成的
          figure = {
              'data': [go.Bar(
                  x = [6,8,14,18,28,36],
                  y = [2,6,6,8,20,38],
                  text = ['a','b','c','d','e','f'],
                  textposition = 'auto'
              )],
              'layout': go.Layout()   
          }
      )
  ])
  
  if __name__ == '__main__':
      app.run_server(port = 5000)
  ```



![image5](images\image5.png)



+ 程式碼 2: 這邊我想繪製一個折線圖，我要使用的是dash_core_components套件裡的Graph 和 plotly.graph_objects裡的Scatter()來實作

```Python
## 導入dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html
## Plotly繪圖套件
import plotly.graph_objects as go


## 啟動dash
app = dash.Dash()

## 設定網頁介面
app.layout = html.Div([
    html.H1("哈囉~~ Dash!!!!"),
    html.Div('Plotly 網頁架構 - Dash的應用'),
    html.P('HTML tag(p) 應用'),
    
    
    dcc.Graph(
        id = 'First Dash Graph',
        ## Plotly的 Figure是由data與graph構成的
        figure = {
            ## 繪圖
            'data': [
                go.Scatter(
                    x = [6,8,14,18,28,36],
                    y = [2,6,6,8,20,38],
                    text = ['a','b','c','d','e','f'],
                    mode = 'markers+lines+text'
            ),
                go.Scatter(
                    x = [6,8,14,18,28,36],
                    y = [16,18,26,28,34,38],
                    text = ['a','b','c','d','e','f'],
                    mode = 'markers+lines+text'
            )
                    
                    ],
            ## 設定圖形介面
            'layout': go.Layout(
                title = "Lines and Markers Plots",
                xaxis = {'title': 'X Axis Values'},
                yaxis = {'title': "Y Axis Values"}
            )   
        }
    )
])

if __name__ == '__main__':
    app.run_server(port = 5000)
```



![image6](images\image6.png)





#### 實作 4: 從外部導入資料集，轉成DataFrame後，繪製圖形

+ 剛剛教大家的都是從程式中去建立數據集，但是我們總不能把公司的數據集，手動寫入程式，那真的會很可怕呀，所以我們需要用Pandas套件來將數據導入，並且它會自動將數據集轉成DataFrame格式
+ 小筆記: 這邊跟大家介紹一下plotly.graph_objects.layout裡面的參數
  + title: 介面主題的名稱
  + xaxis: 設定x軸
  + yaxis: 設定y軸
  + hovermode: 鼠標移入數據點呈現的模式，有'x'、'y'、'closest'、False、'x unified', 'y unified'的參數可以選擇



```Python
## 導入dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html
## Plotly繪圖套件
import plotly.graph_objects as go

## Python數據處理套件
import pandas as pd

## 啟動dash
app = dash.Dash()

## 載入數據集
sales_data = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/sales_success.csv")


## 設定網頁介面
app.layout = html.Div([

    dcc.Graph(
        id = 'First Dash Graph',
        ## Plotly的 Figure是由data與graph構成的
        figure = {
            ## 繪圖
            'data': [
                go.Scatter(
                    x = sales_data.calls,
                    y = sales_data.sales,
                    text = ['a','b','c','d','e','f'],
                    mode = 'markers'
            )
                    ],
            ## 設定圖形介面
            'layout': go.Layout(
                title = "Sales & Calls",
                xaxis = {'title': 'Calls'},
                yaxis = {'title': "Sales"},
                hovermode = 'closest'
            )   
        }
    )
])

if __name__ == '__main__':
    app.run_server(port = 5000)
```



![image7](images\image7.png)

**補充: 我這邊所使用的數據集來自https://github.com/plotly/datasets這裡喔，大家可以下載或直接在線上存取，可以練習使用不同的數據及繪圖**





**我們學會了Dash的基本運用，搭配過去我們學習的Plotly就能製作出許多web架構的視覺化圖形喔，下一篇我會跟大家一起學習如何使用dash_core_components的其他功能，製作出下拉選單、滑桿等等的功能，豐富我們視覺化圖的互動性**



**感謝您的閱讀，如果覺得我還可以，哈哈，幫我拍拍手鼓勵一下喔，感恩**



## Reference

https://www.cnblogs.com/xjnotxj/p/10725268.html

https://medium.com/@auwit0205/dash-by-plotly-%E7%B0%A1%E5%96%AE%E4%BB%8B%E7%B4%B9-ba81f5ef4e29