# Data Visualization資料視覺化- Python -Plotly進階視覺化 - Dash教學(二)



嗨嗨~~ 這篇是延續上一篇，繼續進行我們的Dash實作教學喔~~ 如果不知道Plotly或Dash是什麼都可以參考我之前的文章喔!!



## 實作



#### 實作 5: 使用Dropdown製作下拉式選單

+ 這邊教大家如何使用dash_core_components.Dropdown()函式來製作下拉式選單
+ 補充: 當然還有很多dash_core_components能使用的函式，大家可以參考官網(http://dash.plotly.com/dash-core-components)，裡面有範圍條、勾選欄、文字欄、日期選單等等的功能界鄵與教學
+ Dropdown參數介紹:
  + id: 用來識別這個元素用的
  + className: 設定元素的類別名稱
  + clearable: 傳入Boolean(True or False)，預設為True，用來設定選項旁邊是否出現'X'來取消選項
  + disabled: 傳入Boolean(True or False)，預設為False，設定是否禁止選單功能
  + multi: 傳入Boolean(True or False)，預設為False，設定是否可以同時選擇多個選項
  + optionHeight: 設定選項的範圍高度
  + placeholder: 使用者還未選擇前的預設出現文字
  + style: 設定選單樣式
  + value: 預先輸入的值，輸入選項的value值，就會預先選擇該選項
  + options: 設定選項
    + label: 設定選項標籤名稱
    + value: 設定標籤的值，會與上面的value對應，簡單來說就是上面的value輸入的值會對應到這裡值得標籤名稱
    + disabled:  傳入Boolean(True or False)，預設為False，設定是否禁止此選項
    + title: 屬標移入選項時，自動跳出的一些資訊

+ 補充: 更多Dropdown能使用的參數與功能介紹，可以參考官網(http://dash.plotly.com/dash-core-components/dropdown)

+ 程式碼1: 預設的下拉式選單與自訂預設選項

```Python
## 導入Dash所需的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 設定介面
app.layout = html.Div([
    
    
    ## 製作下拉式選單
    dcc.Dropdown(
            id = 'First Dash Dropdown',
            options = [
                    {'label':'T-shirts', 'value': 'TS'},
                    {'label':'Jeans', 'value': 'J'},
                    {'label':'Skirt', 'value': 'S'},
                    {'label':'Jersey', 'value': 'J'},
                    {'label':'Shoes', 'value': 'SH'},
                    {'label':'Coat', 'value': 'C'}
            ],
            ##預設輸入的值
            value = 'SH'
    )
])


## 開啟Local Server
if __name__ == '__main__':
    app.run_server()

```

![image1](images\image1.png)

+ 程式碼2: 實現可以選擇多個選項的下拉式選單、自訂沒有選項時的預設文字、禁用某個選項和鼠標移入顯示資訊的功能

```Python
## 導入dash所需套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 介面
app.layout = html.Div([
    
    ##下拉式選單
    dcc.Dropdown(
            id = 'First Dash Dropdown',
            options = [
                    {'label':'T-shirts', 'value': 'TS'},
                    {'label':'Jeans', 'value': 'J'},
                    {'label':'Skirt', 'value': 'S', 'disabled':True, 'title':'Skirt is disabled'},
                    {'label':'Jersey', 'value': 'J'},
                    {'label':'Shoes', 'value': 'SH'},
                    {'label':'Coat', 'value': 'C'}
            ],
            ##預設輸入的值
            placeholder = "Choose Products Here",
            multi = True
    )
    
])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()

```

![image2](images\image2.png)



#### 實作6. 使用Slider()製作滑桿

+ 這邊教大家如何使用dash_core_components.Slider()函式來製作滑桿

+ Slider參數介紹:

  + id: 用來識別這個元素用的
  + className: 設定元素的類別名稱
  + disabled: 傳入Boolean(True or False)，預設為False，設定是否禁止滑桿的功能
  + dots: 傳入Boolean(True or False)，如果一步值大於1，可以設定為True，它就會用點渲染滑桿
  + included: 傳入Boolean(True or False)，如果為True，表示會包含連續值，反之就是獨立值
  + marks: 設定滑桿上的點
    + label: 點的標籤名稱
    + style: 點的樣式

  + min: 滑桿的最小值
  + max: 滑桿的最大值
  + step:滑桿滑動一步的值
  + tooltip: 設定工具提式
    + always_visible: 傳入Boolean(True or False)，設定工具提示是否一直呈現
    + placement: 工具提示的相對位置，有"left"、"right"、"top"、"bottom"、"topLeft"、"topRight"、"bottomLeft"、"bottomRight"字串可以帶入

  + updatemode: 滑桿數值的更新模式
    + mouseup: 完成滑動滑塊後，才更新值
    + drag: 滑動滑塊的同時，數值不斷更新

  + value: 設定滑桿的初始值
  + vertical: 傳入Boolean(True or False)，滑桿是否垂直呈現
  + verticalHeight: 設定滑桿高度，預設為400，單位是像素

+ 補充: 更多Dropdown能夠使用的參數與功能介紹，可以參考官網(http://dash.plotly.com/dash-core-components/slider)

+ 程式碼1: 製作一個簡單的滑桿

  ```Python
  ## 導入dash所需套件
  import dash
  import dash_core_components as dcc
  import dash_html_components as html
  
  ## 啟動Dash
  app = dash.Dash()
  
  ## 介面
  app.layout = html.Div([
      
      html.Label('Slider of Distance'),
       
      ## 滑桿
      dcc.Slider(
          min = 0,
          max = 100,
          value = 65,
          step = 5,
          marks = {i: i for i in range(0,100,20)},
          tooltip = {'always_visible':True, 'placement':"bottomRight"},
          vertical = False
      )
  ])
  
  ## 啟動Local Server
  if __name__ == '__main__':
      app.run_server()
  ```



![image3](images\image3.png)



+ 程式碼2: 繪製兩條滑桿，看出設定included的差別，與有無使用tooltip的差別

```Python
## 導入dash需要的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([
    
    ## 建立滑桿
    ## 設定included = True
    dcc.Slider(
        min = 0,
        max = 200,
        value = 66,
        marks = {
            0: {'label':'原點', 'style': {'color': '##FFD306'}},
            50: {'label': '1/4距離'},
            100: {'label': '一半距離'},
            150: {'label': '3/4距離'},
            200: {'label': '到達目的地'}
        },
        step = 20,
        included = True
    ),
    
    
    ## 空行
    html.Br(),
    html.Br(),
    html.Br(),
    html.Br(),
    
    ## 設定included = False
    dcc.Slider(
        min = 0,
        max = 200,
        value = 66,
        marks = {
            0: {'label':'原點', 'style': {'color': '##FFD306'}},
            50: {'label': '1/4距離'},
            100: {'label': '一半距離'},
            150: {'label': '3/4距離'},
            200: {'label': '到達目的地'}
        },
        step = 20,
        tooltip = {'always_visible':True},
        included = False
    ),
    
])


## 啟動Local Server
if __name__ == '__main__':
    app.run_server()

```

![image4](images\image4.png)

**補充小技巧: 在jupyter notebook中，想知道函式裡有哪些參數，除了help()，還有一個方法，就是在函式()框框裡面按下tab+shift就能看到一個資訊框喔**





#### 實作7. 使用RangeSlider()製作範圍滑桿

+ 範圍滑桿的參數幾乎與滑桿一樣，他們的不同在於範圍滑桿可以調整雙向來選取滑塊，這樣感覺很難理解，但是我帶大家看看下面的例子，大家就會了解囉
+ 程式碼: 範圍滑桿 與 滑桿的差別

```Python
## 導入dash需要的套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([
    
    html.Label("範圍滑桿 RangeSlider"),
    
    ## 範圍滑桿
    dcc.RangeSlider(
        min = 0,
        max = 100,
        step = 5,
        value = [60,78],
        marks = {i: i for i in range(0,101,20)}
    ),
    
    ## 空行
    html.Br(),
    html.Br(),
    html.Br(),
    html.Br(),
    
    ## 範圍滑桿
    dcc.Slider(
        min = 0,
        max = 100,
        step = 5,
        value = 66,
        marks = {i: i for i in range(0,101,20)}
    ),
])


## 啟動 Local Server
if __name__ == '__main__':
    app.run_server()

```



![image5](images\image5.png)

#### 實作8. 使用Input()製作一個輸入格

+ 這邊教大家如何使用dash_core_components.Input函式來製作輸入格

+ Input參數介紹:
  + id: 用來識別這個元素用的
  + autoComplete: 瀏覽器是否會自動完成控制元件的值
  + autoFocus: 傳入Boolean值(True/False)，是否在網頁家在好後，自動聚焦
  + className: 設定元素的類別名稱
  + disabled: 傳入Boolean(True or False)，預設為False，設定是否禁止輸入格的功能
  + inputMode: 提示使用者可以輸入的數據類型，有"numeric"、"tel"、"email"、"url"等可以傳入
  + placeholder: 使用者還未輸入字符前的預設出現文字
  + max: 使用者可以輸入的最大值
  + min: 使用者可以輸入的最小值
  + maxLength: 使用者可以輸入的最大字符數量
  + minLength: 使用者可以輸入的最小字符數量
  + multiple: 允許用附輸入多個值，但只適用於"email"或"file"類型(type)，其他類型不適用
  + name: 控制元件的名稱，會與表單提交的同時，一起傳出
  + pattern: 傳入字串(string)，使用正則表達式來檢查使用者輸入的訊息是否符合期待的格式，適用類型為text,search,tel,url,email或password等
  + style: 設定輸入框的樣式
  + size: 控制元件的大小，除非類型為text或password，不然單位就是像素(pixel)，預設為20，適用類型為text,search,tel,url,email或password等
  + spellCheck: 傳入Boolean(True or False)，是否檢查輸入的文字與拼字
  + type: 控制元件的類型，有"text"、"number"、"password"、"email"、"range"、"search"、"tel"、"url"、"hidden"類型傳入
  + value: 預先輸入的值

+ 更多Input參數與應用，可以參考官網(http://dash.plotly.com/dash-core-components/input)
+ 程式碼

```Python
## 導入 Dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 介面
app.layout = html.Div([
    
    html.Label("基本資訊: "),
    
    ## 空一行
    html.Br(),
    
    ## 輸入格
    dcc.Input(
        placeholder = "Input Your Information",
        type = 'text',
        value = '',
        autoFocus = True,
        style = {'color':'black'},
        size = 60,
        spellCheck = False

    ),
    
    ## 空一行
    html.Br(),
    
    html.Label("電話: "),
    
    dcc.Input(
        placeholder = "Input Your Phone Number(10 number only)",
        type = 'tel',
        value = '',
        multiple = True,
        style = {'color':'blue'},
        maxLength = 10,
        size = 60
    )
    
    
])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

![image6](images\image6.png)



#### 實作9. 使用Textarea製作一個文字框

+ 這邊教大家如何使用dash_core_components.Textarea函式來製作文字框
+ 參數介紹
  + id: 用來識別這個元素用的
  + autoFocus: 傳入Boolean值(True/False)，是否在網頁家在好後，自動聚焦
  + className: 設定元素的類別名稱
  + disabled: 傳入Boolean(True or False)，預設為False，設定是否禁止文字框功能
  + cols: 設定文字框的列數
  + contentEditable: 指定元素內容是否可以編輯
  + dir: 設定文字的方向，可以傳入'ltr': 從左到右、'rtl': 從右到左
  + draggable: 傳入Boolean(True or False)，是否可以拖動元素
  + lang: 設定使用的語言
  + maxLength: 使用者可以輸入的最大字符數量
  + minLength: 使用者可以輸入的最小字符數量
  + name: 控制元件的名稱，會與表單提交一起傳出
  + rows: 設定文字框的行數
  + spellCheck: 傳入Boolean(True or False)，是否檢查輸入的文字與拼字
  + style: 文字框的樣式
  + title: 鼠標移入元素時，所顯示的文本訊息
  + value: 預先輸入文字框裡的值
  + wrap: 設定文字是否換行

+ 更多Input參數與應用，可以參考官網(http://dash.plotly.com/dash-core-components/textarea)喔

+ 程式碼

```Python
## 導入 Dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([ 
    ## 繪製文字框
    dcc.Textarea(
        placeholder = '請寫下您的寶貴意見',
        value = '',
        style = {'width':'60%'}
    ),
    ## 空一行
    html.Br(),
    dcc.Textarea(
        placeholder = '備註',
        value = '',
        style = {'width':'40%'}        
    )
])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()
```

![image7](images\image7.png)

#### 實作10. 用Checklist實作一個複選的勾選清單

+ 這邊教大家如何使用dash_core_components.Checklist函式來製作勾選清單

+ 參數介紹

  + id: 用來識別這個元素用的
  + className: 設定元素的類別名稱
  + inputStyle: 設定勾選元素的樣式
  + options: 設定選項
    + label: 設定選項的標籤名稱
    + value: 設定想像的值，它會用在下面提到的value參數上，作為欲勾選的項目值
    + 傳入Boolean(True or False)，預設為False，設定是否禁止勾選此選項

  + value: 預先輸入的值，會自動勾選對應的選項
  + style: 容器的樣式

+ 更多Checklist參數與應用，可以參考官網(http://dash.plotly.com/dash-core-components/checklist)喔

+ 程式碼

```Python
## 導入 Dash套件
import dash
import dash_core_components as dcc
import dash_html_components as html

## 啟動Dash
app = dash.Dash()

## 網頁介面
app.layout = html.Div([
    
    html.Label("垂直的勾選清單"),
    ## 勾選清單
    dcc.Checklist(
        options = [
            {'label':'寫作業','value':'h'},
            {'label':'創作','value':'c','disabled':True},
            {'label':'澆花','value':'f'},
            {'label':'遛狗','value':'d'},
            {'label':'倒垃圾','value':'g'},
            {'label':'洗碗','value':'w'},
        ],
        value = ['h','c'],
        style = {'color':'darkblue'},
        labelStyle = {'display':'block'}
    ),
    
    
    ## 空行
    html.Br(),
    html.Br(),
    html.Label("水平的勾選清單"),
    
    dcc.Checklist(
        options = [
            {'label':'寫作業','value':'h'},
            {'label':'創作','value':'c','disabled':True},
            {'label':'澆花','value':'f'},
            {'label':'遛狗','value':'d'},
            {'label':'倒垃圾','value':'g'},
            {'label':'洗碗','value':'w'},
        ],
        value = ['h','c'],
        style = {'color':'darkblue'},
        labelStyle = {'display':'inline-block','color':'gold'}
    
    )
    
])

## 啟動Local Server
if __name__ == '__main__':
    app.run_server()

```

![image8](images\image8.png)



**這次我們學會了使用Dash在網頁上製作各種功能元件，下一篇我會繼續跟大家一起學習如何使用dash_core_components的其他功能，想要自己學習的話可以參考官網喔，那邊有非常完整的資訊!!**



**感謝您的閱讀，如果覺得我還可以，哈哈，幫我拍拍手鼓勵一下喔，感恩**



## Reference

https://www.cnblogs.com/xjnotxj/p/10725268.html

https://medium.com/@auwit0205/dash-by-plotly-%E7%B0%A1%E5%96%AE%E4%BB%8B%E7%B4%B9-ba81f5ef4e29