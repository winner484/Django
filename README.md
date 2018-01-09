# Django
环境搭建：1 安装Python ；配置path
         2 pip install django
         3 然后是配置环境变量，将这几个目录添加到系统环境变量中：
           C:\Python33\Lib\site-packages\django;C:\Python33\Scripts。
检查 ：
    1.输入python 
    2.输入import django
    3.输入django.get_version()
 
# Django + wechat app
1\ csrf 验证例外
  view.py  
  from django.views.decorators.csrf import csrf_exempt
  ...
  @csrf_exempt
  def insert(request):
    if request.method == "POST":
        username = request.POST.get('username')
        password = request.POST.get("password")
        print(username)
        print(password)
        models.message.objects.create(username=username, password=password)
        # models.message.save()
    # 给普通H5返回一个html模板
    # return render_to_response('insert.html',context_instance=RequestContext(request))
    # return render_to_response('insert.html',)
        # 给微信返回数据
        return HttpResponse("ok")
        
    wechat.js
    var username = 'aaaa991'
    var password = 'bbbb991'
    wx.request({
      url: 'http://127.0.0.1:8000/insert/',
      header: { 'Content-Type': 'application/x-www-form-urlencoded' },
      data: {
        username: username,
        password: password
      },
      method: 'POST', // OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT
      // header: {}, // 设置请求的 header
      success: function (res) {
        console.log(res)
        # res.data:ok       
        // success
      },
      fail: function () {
        // fail
      },
      complete: function () {
        // complete
      }
    })
