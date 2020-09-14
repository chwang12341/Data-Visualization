# Data Visualization資料視覺化- Python -Plotly進階視覺化- Callback方法教學 - Dash教學(三)



嗨嗨~~ 這篇也是延續上一篇，繼續進行我們的Dash實作教學喔~~ 如果不知道Plotly或Dash是什麼都可以參考我之前的文章喔!! 這篇會教大家如何使用Callback的方法，來實現更具互動效果的Plotly視覺化圖喔~~



## 實作



#### 實作11. 用RadioItems實作一個圓形單選的勾選清單



+ 基本上它與Checklist的參數是很相似的喔，差別在於它能製作出圓形的勾選單，而且它是單選的喔，所以預設的value不能像Checklist一樣能給很多值，它只能給一個值
+ 程式碼L 我們來同時比較一下RadioItems 與 Checklist的不同

```Python
## 導入Dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([
    
    html.Label("圓形單選的勾選清單"),
    ## 繪製單選圓形選單
    dcc.RadioItems(
        options = [
            {'label':'寫作業', 'value':'h'},
            {'label': '創作', 'value':'c'},
            {'label':'澆花', 'value': 'f'},
            {'label':'遛狗', 'value':'d'},
            {'label': '倒垃圾', 'value':'g'},
            {'label': '洗碗', 'value': 'w'},
            {'label':'洗衣服', 'value': 'ws'},
            {'label':'運動', 'value':'e', 'disabled':True}
        ],
        value = 'c'
    ),
    
    ## 空行
    html.Br(),
    html.Br(),
    
    html.Label("水平的複選清單"),
    ## 繪製多選勾選清單
    dcc.Checklist(
        options = [
            {'label':'寫作業', 'value':'h'},
            {'label': '創作', 'value':'c'},
            {'label':'澆花', 'value': 'f'},
            {'label':'遛狗', 'value':'d', 'disabled':True},
            {'label': '倒垃圾', 'value':'g'},
            {'label': '洗碗', 'value': 'w'},
            {'label':'洗衣服', 'value': 'ws'},
            {'label':'運動', 'value':'e'}
        ],
        value = ['c','e'],
        style = {'color':'darkblue'},
        labelStyle = {'display': 'inline-block','color':'gold'}
    )
])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

![image1](images\image1.png)

#### 實作12. 用DatePickerRange 與 DatePickerSingle來實現日期選單

+ 使用dash_core_components.DatePickerRange 與 dash_core_components.DatePickerSingle函式來製作日期選單
+ DatePickerRange參數介紹
  + calendar_orientation: 設定日曆方向，有'vertical':垂直、"horizontal":水平的選擇可以傳入
  + clearable: 傳入Boolean(True or False)，設定夏拉日曆選單的右側是否會出現"x"，用來刪除選擇的值
  + day_size: 預設為39，設定日曆的大小，越大包刮的日子越多
  + display_format: 設定日期格式，用"MM TT DD"來進行排列
  + disabled: 傳入Boolean(True or False)，預設為False，設定是否禁止日期選項的功能
  + start_date: 設定組件的開始日期，但要以datetime.datetime物件或"YYYY-MM-DD"的格式傳入
  + end_date: 設定組件的結束日期，但要以datetime.datetime物件或"YYYY-MM-DD"的格式傳入
  + start_date_placeholder_text: 使用者未選擇日期時，在第一個(開始)輸入框中顯示的文字
  + end_date_placeholder_text : 使用者未選擇日期時，在第二個(結束)輸入框中顯示的文字
  + show_outside_days: 傳入Boolean(True or False)，是否顯示過渡到下個月的日期
  + first_day_of_week: 設定日曆的第一天是星期幾，可以傳入介於0~6的數值
  + initial_visible_month: 設定初始化顯示設定初始化顯示的月份，要以"YYYY-MM-DD"個是傳入
  + is_RTL: 傳入Boolean(True or False)，設定日曆的日子呈現的方向，左到右或右到左
  + min_date_allowed: 最小(最久以前)可以選擇的日期，要以"YYYY-MM-DD"格式傳入
  + max_date_allowed: 最大(最遠)可以選擇的日期，要以"YYYY-MM-DD"格式傳入
  + minimum_nights: 選擇的兩個日期相減，不能小於的夜晚個數
+ 更多參數可以參考官網(http://dash.plotly.com/dash-core-components/datepickerrange)
+ 因為它們有許多的共同參數，這邊就不對dash_core_components.DatePickerSingle的參數進行詳解喔，可以參考官網(http://dash.plotly.com/dash-core-components/datepickersingle)
+ 程式碼:

```Python
## 導入Dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 導入日期時間套件
from datetime import datetime as dt

## 啟動Dash()
app.layout = html.Div([
    
    
    html.Label("選擇單一日期"),
    ## 繪製日期選單
    dcc.DatePickerSingle(
        id = 'Singel Date',
        date = dt(2020,9,4)
    
    ),
    
    ## 空行
    html.Br(),
    html.Br(),
    
    
    html.Label("選擇日期範圍"),
    
    ## 繪製選擇日期範圍
    dcc.DatePickerRange(
        id = 'Range_Date',
        start_date = dt(2020,9,4),
        end_date_placeholder_text = '選擇最後日期',
        show_outside_days = True,
        is_RTL = True
    )
])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

![image2](images\image2.png)

#### 實作13. 在網頁上寫Markdown語言

+ 使用dash_core_components_Markdown函式來撰寫Markdown語法

+ 如果對Markdown語言不熟悉的話，可以參考我的文章-Coding起來- 程式開發不可忽略的語言- Markdown 教學(https://matters.news/@CHWang/coding%E8%B5%B7%E4%BE%86-%E7%A8%8B%E5%BC%8F%E9%96%8B%E7%99%BC%E4%B8%8D%E5%8F%AF%E5%BF%BD%E7%95%A5%E7%9A%84%E8%AA%9E%E8%A8%80-markdown-%E6%95%99%E5%AD%B8-bafyreiddczjts6npc75mdo6ep7csiejm5aw2k7pqmvciwr47shsqtdwcpa)

+ 參數介紹:

  + className: 元件的類別名稱
  + dangerously_allow_html: 傳入Boolean值(True or False)，是否允許使用html語法，預設是False，如果設置True，會有XSS(cross-site scripting)攻擊的風險
  + dedent: 傳入Boolean值(True od False)，預設為True，是否刪除掉行中的前導空格
  + highlight_config: 設置強調語法
    + theme: 主題的顏色，有'dark'與'light'，'light'為預設

  + style: 樣式

+ 更多參數可以參考官網(http://dash.plotly.com/dash-core-components/markdown)

+ 程式碼:

```Python
## 導入Dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([
    
    html.Label("dedent設置為True"),
    
    ## Markdown語法撰寫
    dcc.Markdown(
    '''
    
        # Dash 與 Markdown 用法(h1 主題標籤)
        
        ## Markdown 可以參考
        ## [**Coding起來- 程式開發不可忽略的語言- Markdown 教學**](https://matters.news/@CHWang/coding%E8%B5%B7%E4%BE%86-%E7%A8%8B%E5%BC%8F%E9%96%8B%E7%99%BC%E4%B8%8D%E5%8F%AF%E5%BF%BD%E7%95%A5%E7%9A%84%E8%AA%9E%E8%A8%80-markdown-%E6%95%99%E5%AD%B8-bafyreiddczjts6npc75mdo6ep7csiejm5aw2k7pqmvciwr47shsqtdwcpa)
        
        ## h2標籤
        
        
        #### 感謝大家
    
    ''',
    dedent = True
    ),
    
    
    ## 空行
    html.Br(),
    html.Br(),
    html.Br(),
    html.Br(),
    
    html.Label("dedent設置為False，theme設置為dark"),
    ## Markdown語法撰寫
    dcc.Markdown(
    '''
    
        # Dash 與 Markdown 用法(h1 主題標籤)
        
        ## Markdown 可以參考
        ## [**Coding起來- 程式開發不可忽略的語言- Markdown 教學**](https://matters.news/@CHWang/coding%E8%B5%B7%E4%BE%86-%E7%A8%8B%E5%BC%8F%E9%96%8B%E7%99%BC%E4%B8%8D%E5%8F%AF%E5%BF%BD%E7%95%A5%E7%9A%84%E8%AA%9E%E8%A8%80-markdown-%E6%95%99%E5%AD%B8-bafyreiddczjts6npc75mdo6ep7csiejm5aw2k7pqmvciwr47shsqtdwcpa)
        
        ## h2標籤
        
        
        #### 感謝大家
    
    ''',
    dedent = False,
    highlight_config = {'theme':'dark'},
    dangerously_allow_html = False
    )
])


## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

![image3](images\image3.png)

#### 實作14. Callbacks回調功能

+ 這邊我將導入之前沒有使用到的套件-dash.dependencies底下的Input與Outout套件
+ 使用@app.callback(Output(component_id, component_property), [Input(component_id,component_property)])來實現回調(Callbacks)功能，將他寫在我們自訂一的Input -> Output函式上面
+ 更多應用可以參考官網(http://dash.plotly.com/basic-callbacks)
+ 程式碼1:簡單的輸入框回調功能

```Python
## 導入Dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 導入Callbacks所需的套件
from dash.dependencies import Input, Output

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([
    
    html.H4("實作 Callbacks 方法"),
    
    ## 繪製Input輸入框
    html.Div([
        dcc.Input(
            id = 'my_input',
            value = '請輸入任何符號與文字',
            type = 'text'
        ),
    ]),
    ## 空行
    html.Br(),
    html.Br(),
    html.Label("結果: "),

    ## 設定Output的地方
    html.Div(id = 'my_output'),
        
])

## 實現Callbacks方法
@app.callback(
    Output(component_id = 'my_output', component_property = 'children'),
    [Input(component_id = 'my_input', component_property = 'value')]
)
def apply_output_div(input_value):
    return "結果為: {}".format(input_value)


## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```



![image4](images\image4.png)



+ 程式碼2: 結合Plotly，實現滑桿調整視覺圖

```Python
## 導入Dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 導入Callbacks所需的套件
from dash.dependencies import Input, Output

## 導入 Plotly Express 套件
import plotly.express as px

## 載入數據集，並且取1900年之後的數據
df = px.data.gapminder().query('year >= 1900')

## 啟動Dash
app = dash.Dash()

## 設置網頁標題名稱
app.title = 'Dash Callbacks 教學'

## 網頁介面
app.layout = html.Div([
    
    ## 設置輸出的圖
    dcc.Graph(id = 'graph_according_to_slider'),
    
    ## 繪製滑桿
    dcc.Slider(
        id = 'year_slider',
        min = df.year.min(),
        max = df.year.max(),
        value = df.year.min(),
        marks = {str(year): str(year) for year in df.year.unique()},
        step = None
        
    )
    
])


## 撰寫Callbacks 函式
@app.callback(
    Output(component_id = 'graph_according_to_slider', component_property = 'figure'),
    [Input(component_id = 'year_slider', component_property = 'value')]

)
def apply_output_figure(year_selected):
    ## 取得選擇的年份數據集
    selected_df = df[df.year == year_selected]

    ## 繪圖
    fig = px.scatter(selected_df, x = 'pop', y = 'lifeExp',
                    size = "gdpPercap", color = "continent", hover_name = "country",
                    log_x = True, size_max = 66 )
    
    ## 更新介面
    fig.update_layout(transition_duration = 600)
    
    
    return fig


## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

**使用Callbacks功能就能讓我們的視覺畫圖更具互動效果喔!!**

**補充: 使用app.title()可以設定我們顯示在網頁上的主題名稱喔**

![image5](images\image5.png)







**這邊我們對於Dash的一些控制元件的實作有了實作的能力後，大家就可以開始自己動手玩玩看喔，如果對於Callbacks功能想了解更多的話，可以參考我的下一篇，也可以自己參考官網的教學喔!!**





**感謝您的閱讀，如果覺得我還可以，哈哈，幫我拍拍手鼓勵一下喔，感恩**





## Reference

https://www.cnblogs.com/xjnotxj/p/10725268.html

https://medium.com/@auwit0205/dash-by-plotly-%E7%B0%A1%E5%96%AE%E4%BB%8B%E7%B4%B9-ba81f5ef4e29