# Data Visualization資料視覺化- Python -Plotly進階視覺化- Callback進階方法教學 - Dash教學(四)



Yoyo~~ 這篇也是延續上一篇喔，這篇我會著重在Callback的功能喔，如果不知道Plotly或Dash是什麼都可以參考我之前的文章喔!! 這篇會教大家如何使用更多Callback的方法，來實現更具互動效果的Plotly視覺化圖喔~~



## 實作





#### 實作15. Callbacks回調功能(多個Input)

+ 一樣使用@app.callback()(Output(component_id, component_property), [Input(component_id,component_property)])來實現回調(Callbacks)功能，但這次增加了許多個Input(component_id,component_property)
+ **補充: 這邊使用的數據是來自https://github.com/plotly/datasets這裡喔!!這裡面有相當多的數據集，並且都是開源的可以供大家練習用**
+ **補充: 這邊有使用外部的樣式表external_stylesheets，有興趣的大家，可以參考官網(https://dash.plotly.com/external-resources)的介紹喔**
+ 程式碼:

```Python
## 導入Dash 所需套件
import dash
import dash_core_components as dcc
import dash_html_components as html

##　導入 Callbacks 所需的套件
from dash.dependencies import Input, Output

## 導入 Python 數據處理套件
import pandas as pd

## 導入 外部Css 樣板
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動 Dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 導入數據集 
df = pd.read_csv('https://raw.githubusercontent.com/plotly/datasets/master/country_indicators.csv')

## 列出indicater Name底下有的特徵值
indicators = df['Indicator Name'].unique()

## 設定網頁介面
app.layout = html.Div([
    
  html.Div([
    
    ## 設定左邊的區域
    html.Div([
        ## 繪製下拉選單
        dcc.Dropdown(
            id = 'xaxis_column',
            options = [{'label': i, 'value': i} for i in indicators],
            value = 'Domestic credit provided by financial sector (% of GDP)'
        ),
        
        ## 繪製單選圓形清單
        dcc.RadioItems(
            id = 'xaxis_type',
            options = [{'label': i, 'value': i} for i in ['Linear', 'Log']],
            value = 'Linear',
            labelStyle = {'display' : 'inline-block'}
        )
    ],
    style = dict(width = '40%', display = 'inline-block')),
    
    
    ## 設定右邊的區域
    html.Div([
        ## 繪製下拉選單
        dcc.Dropdown(
            id = 'yaxis_column',
            options = [{'label': i, 'value': i} for i in indicators],
            value = 'Population density (people per sq. km of land area)'
        ),
        
        ## 繪製單選圓形清單
        dcc.RadioItems(
            id = 'yaxis_type',
            options = [{'label': i, 'value': i} for i in ['Linear', 'Log']],
            value = 'Linear',
            labelStyle = {'display' : 'inline-block'}
        )
    ],
    style = dict(width = '40%', display = 'inline-block')),
]),

## 設置輸出的圖片
dcc.Graph(id = 'indicator_graphic'),

## 繪製滑桿
dcc.Slider(
    id = 'year_slider',
    min = df.Year.min(),
    max = df.Year.max(),
    value = df.Year.min(),
    marks = {str(year): str(year) for year in df.Year.unique()},
    step = None
)

])



## 實作Callbacks方法，傳入多個Input，Output出一張圖
@app.callback(
    Output(component_id = 'indicator_graphic', component_property = 'figure'),
    [
        Input(component_id = 'xaxis_column', component_property = 'value'),
        Input(component_id = 'yaxis_column', component_property = 'value'),
        Input(component_id = 'xaxis_type', component_property = 'value'),
        Input(component_id = 'yaxis_type', component_property = 'value'),
        Input(component_id = 'year_slider', component_property = 'value')
    ]
)
def apply_graph(xaxis_column_name, yaxis_column_name,
                 xaxis_type, yaxis_type,
                 year_value):
    
    ## 滑桿選擇的年份數據
    selected_year = df[df['Year'] == year_value]
    
    ## 繪製散點圖
    fig = px.scatter(
                     ## x軸選的indicator名稱的值
                     x=selected_year[selected_year['Indicator Name'] == xaxis_column_name]['Value'],
                     ## y軸選的indicator名稱的值   
                     y=selected_year[selected_year['Indicator Name'] == yaxis_column_name]['Value'],
                     ## 屬標移入的資訊
                     hover_name=selected_year[selected_year['Indicator Name'] == yaxis_column_name]['Country Name'])

    
    ## 更新介面
    fig.update_layout(margin={'l': 40, 'b': 40, 't': 40, 'r': 40}, hovermode='closest')
    
    
    ## 更新x軸
    fig.update_xaxes(title=xaxis_column_name, type='linear' if xaxis_type == 'Linear' else 'log') 
    
    
    ## 更新y軸
    fig.update_yaxes(title=yaxis_column_name, type='linear' if yaxis_type == 'Linear' else 'log') 

    return fig



## 啟動Local Server
if __name__ == '__main__':
    app.run_server(debug = False)
```



![image1](images\image1.png)





#### 實作16 Callbacks回調功能(多個Output)

+ 一樣使用@app.callback(Output(component_id, component_property), [Input(component_id, component_property)])來實現回掉的功能，但這次增加了許多個Ouput(component_id, component_property)

+ 補充: dash_html_components**次方**的寫法，格式: [填入底數, html.Sup('填入次'方數')]，像是如果我們想寫x的四次，就可以寫成['x', html.Sup(4)]，或是6的x次方就寫成[6, html.Sup('x)]這樣表達

+ **小筆記: Input 與 Output裡面的component_id與 component_propert是可以省略不寫的喔**

+ 程式碼:

```Python
## 導入Dash 所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output

## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 設定網頁主題名稱
app.titile = 'Dash Multi-Output'

## 設定網頁介面
app.layout = html.Div([
    
    ## 製作一個輸入格
    dcc.Input(
        id = 'num_v',
        type ='number',
        value = 6
    ),
    
    
    ## 製作表格(輸出成果)
    html.Table([
       html.Tr([html.Td(['x', html.Sup(2)]), html.Td(id = 'square')]),
       html.Tr([html.Td(['x', html.Sup(3)]), html.Td(id = 'cube')]),
       html.Tr([html.Td(['x', html.Sup(6)]), html.Td(id = 'six')]),
       html.Tr([html.Td([2, html.Sup('x')]), html.Td(id = 'twos')]),
       html.Tr([html.Td([6, html.Sup('x')]), html.Td(id = 'sixs')]),
       html.Tr([html.Td(['x', html.Sup('x')]), html.Td(id = 'x^^x')])
        
    ]),
    
])




## 實作callback功能，多個Output實現
@app.callback(
    [
        Output('square', 'children'),
        Output('cube', 'children'),
        Output('six', 'children'),
        Output('twos', 'children'),
        Output('sixs', 'children'),
        Output('x^^x', 'children')
    ],
    [Input('num_v', 'value')]
)
def multi_ouput_callback(x):
    return x**2, x**3, x**6, 2**x, 6**x, x**x



## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```



![image2](images\image2.png)

#### 實作17. 串鍊型的Callback使用方法，立即更新結果

+ 這次的不同是我們要用上許多的@app.callback()函式來達成

+ 程式碼1: 這邊我想製作一個有關聯的兩個選項，也就是第二個選項會根據第一個選項調整呈現的選擇項目，然後結合這兩個選項，最後輸出一個文字串

```Python
## 導入Dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output

## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 創建選項字典dict
area_options = {
    'Taiwan': ['HsinChu', 'Taipei', 'Taichung', 'Hualien'],
    'America': ['Los Angeles', 'New York', 'Houston', 'Chicago'],
    'China': ['Beijin', 'ShangHai', 'Macau', 'Hong Kong']
    
}

## 設定網頁介面
app.layout = html.Div([
    
    html.Label("請選擇一個地區"),
    
    ## 製作單選選項(第一個選項)
    dcc.RadioItems(
        id = 'area_selected',
        options = [{'label': i, 'value': i} for i in area_options.keys()],
        value = 'Taiwan'
    ),
    
    ## 分隔線
    html.Hr(),
    
    html.Label("請選擇地區裡的城市"),
    
    
    ## 製作單選選項(第二個選項)
    dcc.RadioItems(id = 'city_selected'),
    
    html.Hr(),
    
    html.Div(id = 'display_selected_area')
    
])

## 使用callback，控制選完區域(第一選項)後，城市選項(第二選項)的生成
@app.callback(
    Output('city_selected', 'options'),
    [Input('area_selected', 'value')]
)
def cities_options(selected_area):
    return [{'label': c, 'value': c} for c in area_options[selected_area]]


## 給定香的城市選項清單，一個預設的value，也就是有一個預設的選項，這邊我的預設選在第三個城市選項
@app.callback(
    Output('city_selected', 'value'),
    [Input('city_selected', 'options')]
    
)
def set_cities_RadioItems_value(availabel_options):
    return availabel_options[2]['value']
    
    
##　結合兩個選項的value值顯示最終的字串結果
@app.callback(
    Output('display_selected_area', 'children'),
    [
        Input('area_selected', 'value'),
        Input('city_selected', 'value')
    ]
)
def set_display_children(selected_area,selected_city):
        return '{} is a beautiful city in {}'.format(selected_city, selected_area)
    
## 啟動Local Sercer
if __name__ == '__main__':
    app.run_server()

```



![image3](images\image3.png)





+ 程式碼2: 上面的時做適用選項結合來輸出結果，這邊我想讓使用者自行輸入四個值，並結合輸出結果



```Python
## 導入dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output

## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 網頁介面
app.layout = html.Div([
    html.Label("造句: 跑很快的..., 跳很高的..., 飛很高的..., 兇猛的..."),
    
    ## 製作輸入格
    dcc.Input(id = 'input_1', type = 'text', value = '小狗'),
    dcc.Input(id = 'input_2', type = 'text', value = '小貓'),
    dcc.Input(id = 'input_3', type = 'text', value = '小鳥'),
    dcc.Input(id = 'input_4', type = 'text', value = '小虎'),
    
    ## 放置結果
    html.Div(id = 'result_string')
])

## 啟用callback，將輸入於輸入格的文字結合成字串
@app.callback(
    Output('result_string', 'children'),
    [
        Input('input_1', 'value'),
        Input('input_2', 'value'),
        Input('input_3', 'value'),
        Input('input_4', 'value')
    ]
)
def applt_output(input1, input2, input3, input4):
    return '跑很快的"{}"，跳很高的"{}"，飛很高的"{}"，兇猛的"{}"'.format(input1,input2,input3,input4)
    

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()

```

![image4](images\image4.png)

#### 實作18. State用法，不立即更新結果

+ 上面實作17中的第二個粒子，當使用者輸入值時，會立即觸發結果的更新，也就是即時呈現的效果，而State的用法，則是可以不馬上進行更新，而是間皆被設定為Input()的元件觸發，大家實際跑過下面的例子後就會理解囉

+ 這邊我們的@app.callback()裡面除了Output與Input，會多使用到State的方法

+ 程式碼: 前面四個的輸入值，必須等到後面的按鈕被觸發後，才會更新結果

```Python
## 導入Dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output, State

## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 網頁介面
app.layout = html.Div([
    html.Label("造句: 跑很快的..., 跳很高的..., 飛很高的..., 兇猛的..."),
    
    ## 製作輸入格
    dcc.Input(id = 'input_1', type = 'text', value = '小狗'),
    dcc.Input(id = 'input_2', type = 'text', value = '小貓'),
    dcc.Input(id = 'input_3', type = 'text', value = '小鳥'),
    dcc.Input(id = 'input_4', type = 'text', value = '小虎'),    
    
    ## 製作一個按鈕
    html.Button(id = 'button_submit', n_clicks = 0, children = 'Submit'),
    
    ## 輸出結果
    html.Div(id = 'result_string')
    
    
])

## 調用callback，使用State用法來實現不立即更新的效果
@app.callback(
    Output('result_string', 'children'),
    [Input('button_submit', 'n_clicks')],
    [
        State('input_1', 'value'),
        State('input_2', 'value'),
        State('input_3', 'value'),
        State('input_4', 'value')
    ]
)
def apply_update_output(n_clicks, input1, input2, input3, input4):
    return u'''
        '跑很快的"{}"，跳很高的"{}"，飛很高的"{}"，兇猛的"{}"
        按鈕備案了{}次
    '''.format(input1, input2, input3, input4, n_clicks)
    
## 啟動Local Sever
if __name__ == '__main__':
    app.run_server()
```





![image5](C:\Users\user\Desktop\Data-Visualization\Dash\4\images\image5.png)



#### 實作19. 使用PreventUpdate來禁止某些狀況下的更新

+ Senario: Callbacks會馬上對輸出結果進行更新，但有些狀況下，我們並不想要更新輸出，這時就可以使用raise PreventUpdate的方法來達成目標

+ 這邊我們需要導入PreventUpdate套件來實作

+ 程式碼: 這邊我想時做一個按鈕，每當點擊它時，它就會顯示一段文字，以及點擊次數，但我希望能夠在一開始，也就是還未被點擊時與第二次點擊按鈕時，輸出的結果文字並不會更新，也就是顯示在輸出字串中的點擊次數，在第二次點擊的時候並不會更新

```Python
##  導入Dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
from dash.exceptions import PreventUpdate

## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 網頁介面
app.layout = html.Div([
        html.Label('為點擊與第2次點擊時，輸出內容並不會更新'),
    
        ## 製作按鈕
        html.Button('按下按鈕', id = 'button1', n_clicks = None),
        
        ## 輸出結果
        html.Div(id = 'show_content')
])


##使用callback，並搭配Prevent Update來防止某些狀況下的更新
@app.callback(
    Output('show_content', 'children'),
    [Input('button1', 'n_clicks')]
)
def update_content(n_clicks):
    if n_clicks == None or n_clicks == 2:
        raise PreventUpdate
    else:
        return '''
                    目前第{}點擊按鈕，
                    未點擊與第2次點擊時，輸出內容並不會更新
                '''.format(n_clicks)

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

![image6](C:\Users\user\Desktop\Data-Visualization\Dash\4\images\image6.png)

## 實作20. 只防止部分更新的方法- dash.no_update

+ 使用情境: 假設我們有兩個輸出值A與B，一般情況下只會觸動一個輸出值A，但在某些狀況下，我希望能觸動輸出值B，但又要保留上一次的輸出值A，這時就會需要用到dash.no_update這個方法

+ 程式碼:

```Python
## 導入dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
from dash.exceptions import PreventUpdate


## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)

## 網頁介面
app.layout = html.Div([
    html.Label('輸入一個數值來找尋它的質因子'),
    
    
    ## 製作一個輸入格
    dcc.Input(id = 'num', type = 'number', debounce = True, min = 1, step = 1),
    
    
    ## 輸出結果
    html.P(id = 'error', style = {'font':10, 'color':'darkblue'}),
    html.P(id = 'prime_factor')
])


## 使用callback，來實現只質因子的功能，以及當輸入質就是質因子時，只防止部分顯示的功能
@app.callback(
    [
     Output(component_id = 'prime_factor', component_property = 'children'),
     Output(component_id = 'error', component_property = 'children')
    ],
    [Input(component_id = 'num', component_property = 'value')]
)
def output_prime_factor(num):
    ## 當使用者還未輸入任何值時
    if num == None:
        ## 防止更新任何的輸出結果
        raise PreventUpdate

    ## 計算輸入值的質因素
    prime_factors = find_prime_factors(num)
    

    ## 如果質因素只有一個，輸出它自己就是質因素，並且不更新上一次輸入值的輸出結果，也就是保留了上一次的結果
    if len(prime_factors) == 1:
        return dash.no_update, '{} 本身就是質因子!!當樹入值就是質因子時，上一次不是本身是質因子的輸出結果不會更新'.format(num)
    
    ## 輸出所有的質因子
    return '{} 的質因素: {}'.format(num, '*'.join(str(f) for f in prime_factors)),''


## 找尋質因素
def find_prime_factors(num):
    n = 2
    output_result = []
    
    while n <= num:
        if ((num % n) == 0):
            output_result.append(n)
            num = int(num / n)
        else:
            n += 1
            
    return output_result

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()

```



![image7](images\image7.png)















#### 補充: component_property 如何賦予值

+ 我們在寫callback的時候，Input與Output一定會有兩個參數，component_id 與 component_property，component_id很容易理解就是針對我們要操作的元件id，而component_property的給值我們時常看到像是"children"、"figure"、"value"、"options"等等，為什麼是這樣給呢?

+ 其實是這樣的不同的控制元件，像是Slider()、Grap()、RadioItems()，裡面都有自己的屬性參數，像是RadioItems就會有options, value等，而Graph()就會有figure等，那component_property指的就是我們要針對哪一個元件底下的參數進行操作

+ 簡單來說component_id是欲操作的元件id，而component_property指的是欲操作此元件底下的哪個參數值

+ 示意圖

  

![image8](images\image8.png)



#### 實作21. 使用dash.callback_context來了解哪些輸入元件被觸發的狀況

+ 使用情境: 假設有一個班級要選班長，然後有四位的候選人，班上同學依序上來投票，要能及時呈現各候選人的票數，以及當下上來投票的同學投給誰

+ 程式碼:

```Python
## 導入dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output

import json

## 導入外部樣式表
external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

## 啟動dash
app = dash.Dash(__name__, external_stylesheets = external_stylesheets)


## 網頁介面
app.layout = html.Div([
    html.Label("班長候選人", style = {'color':'darkblue'}),
    
    ## 製作按鈕
    html.Button('小紅', id = 'btn_1'),
    html.Button('小黃', id = 'btn_2'),
    html.Button('小白', id = 'btn_3'),
    html.Button('小紫', id = 'btn_4'),
    
    ## 輸出結果
    html.Div(id = 'result_container')
])

## 使用callback，來記錄與呈現按鈕資訊
@app.callback(
    Output('result_container', 'children'),
    [
        Input('btn_1', 'n_clicks'),
        Input('btn_2', 'n_clicks'),
        Input('btn_3', 'n_clicks'),
        Input('btn_4', 'n_clicks')
    ]
)
def display_voting_result(btn1, btn2, btn3, btn4):
    
    ## 啟用dash.callback_context
    cbc = dash.callback_context
    
    ## 用json呈現dash.callback_context如何存取觸發數據
    cbc_json = json.dumps({
        'states': cbc.states,
        'triggered': cbc.triggered,
        'inputs': cbc.inputs
    }, indent = 2)
    
    ## 是否觸發按鈕
    ## 觸發按鈕
    if cbc.triggered:
        ## 取得觸發按鈕的prop_id，並把後面的n_clicks與前面的btn_拿掉
        bid = cbc.triggered[0]["prop_id"].split('.')[0].split('_')[1]
        
        button_id = '候選人 '+ bid
        
    ## 沒有觸發按鈕
    else: 
        button_id = '還沒開始投票'
        
    
    return html.Div([
        html.Tr([
            html.Th('小紅'),
            html.Th('小黃'),
            html.Th('小白'),
            html.Th('小紫'),
            html.Th('這位同學投票給')
        ]),
        

        html.Tr([
            html.Td(btn1 or 0),
            html.Td(btn2 or 0),
            html.Td(btn3 or 0),
            html.Td(btn4 or 0),
            html.Td(button_id)
        ]),
        ## 顯示json格式
        html.Pre(cbc_json)
    ])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```



![image9](images\image9.png)







**對Dash有了很多理解後，結合之前Plotly視覺化套件，我們就能在網頁實現互動視覺圖囉，如果想了解Plotly的話，也可以參考我的文章喔，感謝您的閱讀，如果覺得我還可以，哈哈，幫我拍拍手鼓勵一下喔，感恩**





## Reference

https://www.cnblogs.com/xjnotxj/p/10725268.html

https://medium.com/@auwit0205/dash-by-plotly-%E7%B0%A1%E5%96%AE%E4%BB%8B%E7%B4%B9-ba81f5ef4e29