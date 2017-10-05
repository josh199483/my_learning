# ajax
## 1、ajax簡介
Ajax：Asynchronous JavaScript and XML<br>
事實上，Ajax 並非一個新的技術，而是將一些已存在的技術整合在一起<br>
* XHTML and CSS<br>
* The Document Object Model (DOM)<br>
* JavaScript<br>
* XML and XSLT<br>
* The XML HttpRequest object<br>
其非同步的觀念，免除傳統於轉換新頁面時，呼叫載入整個網頁資料，可於使用者在頁面操作某項動作時，已非同步的方式呼叫後端服務器進行資料傳遞或比對之動作，增加更快速且及時的互動效果。<br>
## 2、ajax基本操作
.ajax()為低階用法，這邊的低階不是比較爛，是代表可以使用更底層的方法<br>
.get()和.post()方法為高階方法，代表已經用.ajax()包裝成一種常用的方法<br>
```
$.ajax({
            type:"POST", //選擇POST方法
            url:"{% url 'ajax' %}", //選擇要取得資訊的URL
            data:{"a":a, "b":b}, //也可以額外傳送data到server端，經過server處理後再回傳
            data-type:"json", //設定回傳格式
            success: function(alldata){ //根據收到的response，取得json裡的資料，並且把資料做一些呈現或處理
                    $('#result').html(alldata.result)
                     }
        })
```
## 3、與django搭配
整個流程是這樣:前端發送HTTP文件、django接收HTTP文件並返回HTTP RESPONSE文件、前端接收RESPONSE並顯示結果
在template中，此處使用.ajax()，發送get(也可以是post request) request是名稱為ajax的url，type回傳json的格式，當success之後，要對收到的回應做對應的處理
```
<script>
    $(document).ready(function(){
      $("#sum").click(function(){
        var a = $("#a").val();
        var b = $("#b").val();
        $.ajax({
            type:"GET",
            url:"{% url 'ajax' %}",
            data:{"a":a, "b":b},
            data-type:"json",
            success: function(alldata){ //根據收到的response，取得json裡的資料
                    $('#result').html(alldata.result)
                     }
        })
        .done(function(alldata,status) {
            
            //做任何你想要在ajax請求成功後想做的事
        })
      });
      setInterval(getData,2000); //也可以讓ajax請求以setInterval的方式，定時請求
    });
</script>
```
在django中url.py，傳送的ajax內容是由views的ajax方法來決定
```
urlpatterns = [
     ..
     url(r'^ajax/', views.ajax, name = 'ajax'),
]
```
在views.py，根據收到哪種類型的request，去回傳對應的資料
```
def ajax(request):
    if request.method == 'GET':
        print(request.GET)
        if request.is_ajax(): #確認該request為ajax請求
            a = int(request.GET.get('a'))
            b = int(request.GET.get('b'))
            value = Post.objects.all().values_list('value').order_by('-date')[:1][::-1] #也可直接把資料庫物件以ajax方式回傳
            return_json = {'result':a+b,'value':value} #回傳a+b的結果，和資料庫的資料物件
            return HttpResponse(json.dumps(return_json), content_type='application/json')
            #return JsonResponse(return_json, safe=False) 較新的版本可使用，直接回傳json response 
```
## 問題1:ajax的success option、.done()、.success()、.ajaxSuccess()之間的差別??!
1、首先.success()方法is deprecated，就是叫你別用的意思啦!<br>
2、.done()和.ajaxSuccess()的最大差別在.done()is local event，.ajaxSuccess()is global event，也就是，假設有兩個ajax request:
var jqXHRobject1 = $.post({url: “http:// a.com”});<br>
var jqXHRobject2 = $.post({url: “http:// a.com”});<br>
.ajaxSuccess()方法只能被$(document)呼叫，並且.done()會比.ajaxSuccess()優先呼叫<br>
$(document).ajaxSuccess(callback)，以上任何一個ajax request成功，callback都會被觸發<br>
$(document).done(callback)，這句就是錯誤的<br>
因為.done()只能被jqXHR物件呼叫，如下:<br>
jqXHRobject1.done(callback)，此時只有jqXHRobject1的request成功才會呼叫callbackFunction<br>
3、ajax的success option和.done()其實很相像，幾個小差別是:<br>
* .done()可以在ajax request外面呼叫，比較彈性
* .done()可以包含多個callbackFunction，success option只能有一個
* 用.done()比較有可讀性
* 不過success option一定比.done()早執行，因為是包含在ajax request裡面

