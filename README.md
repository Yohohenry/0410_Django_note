# Django_note 0410
teacher: 何振豪&nbsp;jojotenya@gmail.com

### 網頁運作原理
client (發出請求 —>) web server(發出請求 —> )database<br>	
&emsp;&emsp;&emsp;&emsp;(<— 返回結果)&emsp;&emsp;&emsp;&emsp;&emsp;(<— 回傳資料)

### Django 基本介紹
許多大公司都使用Django，且歷史悠久市場佔有率最高，提供total solution 給初學者了解網站的運作

### Django 原理與架構(MTV)
網頁伺服器接收客戶端的請求（寫菜單），用URLS做分析其中的views 若有需要取得DB的資料，接著models會再使用ORM的語法去DB取用資料（料理食材），接著回傳後套用Templates呈現在客戶端面前（擺盤）
主要的三個角色MTV（models, templates, views）
<br>

獨立出這三個角色的目的是為了
* 分離商業邏輯與UI
* 前後端獨立開來
* 擁有彈性
* 易維護
* 降低複雜度
<br>
![image](https://github.com/Yohohenry/Django_note/blob/master/MTV.png)
<br>
### 使用虛擬環境
source py3env/bin/activate 啟用，若要關閉則是直接deactivate

### 下載Django
`pip install Django==3.0.0`

### 開始建立Django專案（blog）
一個專案可以含有多個app（功能），可以隨時擴充拔脫也可以拿去給別的專案使用
* 建立Django project  —> django-admin startproject project .
* 建立資料庫 python mange.py migrate 指令會產生db.sqlite3的資料庫
* 建立app python manage.py startapp blog

### url dispatcher — django 如何解析使用者於瀏覽器輸入的url
在urls.py檔案中urlpatterns （想像為菜單）寫到 
urlpatterns = [
    path(‘admin/', admin.site.urls)
   path(‘blog/‘, include(‘blog.urls’))
]

當url符合此格式時就會去使用admin.site.urls的app進到後台管理系統
1. python manage.py runserver 架起server
2. 預設port為8000 
3. 在瀏覽器輸入https://127.0.0.1:8000/admin即可進入後台
當路徑符合／blog時會導入blog.urls
而blog/下的urls 則是做 views.post_list, name='post_list’)的工作

### url dispatcher — django 如何根據url指派正確的view程式運行
先在views.py 中新增post_list的函示, 讓http 回傳render 的內容

### 製作templates
blog/templates/blog/post_list.html 
將模板導入即可使寫死的網頁成功顯示在/blog路徑

### django ORM 與資料庫對話
![image](https://github.com/Yohohenry/Django_note/query.png)
* {% for post in posts %}一個for 迴圈取posts裡所有的post & {{ }}  Tags: {% %}, 處理邏輯 Variables: {{}} , 取值用
### static 資料夾 -裝飾網頁
使用bootstrap免費模板
1. 將模版下載後找到需要的樣式之index.html檔放入templates 中
2. 而樣式.css 與bootstrap.min.css 則放入static資料夾中
3. 修改index.html中css link路徑
4. 在urls.py中新增一項urlpatterns url(r'^$', views.frontpage)  ps. frontpage為自定義函式
5. 既然要在views中取用frontpage函式，則在views.py中新建一個frontpage 連結到 index.html檔案

### 加入表單
1. 新增form.py放入表單的樣式
2. 接著在html中加入form的tags
3. 必須在views中加入variables供html取用
4. 而表單提交的功能也須在urls中加入path
5. views新增add_record function
6. 此時會有csrf的認證需要解決，必須在html 中的加入{% csrf_token %} 在form tag中

### url 跨頁傳遞資料
1. 在文章的標題有超連結，需要在urlpatterns 加入新的菜單(post_record)
2. views.py 新增新的function post_record
3. 並再新增post_record.html 檔案

### 模板繼承
1. 建立base.html（當作固定模版）
2. 將要替換的部分寫成{% block content %}{% endblock %}
3. 接著修改post_record.html & post_list.html
4. 利用{% extends ‘blog/base.html’ %}
<br>{% block content %}內容{% endblock %} 包覆內容（\<head>部分不需寫）

