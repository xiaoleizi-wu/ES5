## Window

	setTimeout(), setInterval()
	setTimeout()和setInterval() 用来注册指定的时间之后单次或重复调用的函数
	setTimeout() 方法用来实现一个函数在指定毫秒执行
```
updateClock = function () {
    console.log('begin to update')
}
```
	注意代码顺序
```
setTimeout(updateClock, 6000) // 6s后只执行一次updateClock()函数
setInterval(updateClock, 6000) // 每6秒调用updateClock()

function invoke(f, start, interval, end) {
    if(!start) start = 0
    if(arguments.length <= 2) {
        setTimeout(f, start)
    } else  {
        setTimeout(repeat, start)
        function repeat() {
            var h = setInterval(f, interval)
            if(end) {
                setTimeout(function () {
                    clearInterval(h)
                }, end)
            }
        }
    }
}

// invoke(updateClock, 2, 1, 10)
```

### 浏览器定位和导航
	Window对象的location属性引用是Location对象，表示该窗口中当前显示的文档URL
	Document对象的location属性也引用到了Location对象
	console.log(window.location === document.location) // true

### 提取URL的搜索字符串中的参数
```
function urlArgs() {
    var args = {}
    var query = location.search.substring(1) // 查找到查询串，并且去掉？
    var pairs = query.split('&')
    for(var i = 0; i < pairs.length; i++) {
        var post = pairs[i].indexOf('=') // 查找'name = value'
        var name = pairs[i].substring(0, post)
        var value = pairs[i].substring(post+1)
        value = decodeURIComponent(value) // 对value进行解码
        args[name] = value
    }
    return args
}
```

### 载入新的文档
	Location对象的assign()方法可以载入并显示指定的URL文档，replace()也类似，但是它载入新文档
	之前会把浏览历史中的文档删除
	location = 'http://www.baidu.com'
	可以把URL赋值给location, 它们会相对于当前的URL进行解析
	location = 'page2.html'
	#top跳转到文档的顶部，若是没有元素的ID是'TOP'，它会让浏览器跳到文档开始
	location = '#top'

### 浏览历史
	Window对象的history属性引用是窗口的History对象。History是把窗口的浏览历史用文档和文档状态表示
	History对象的length属性表示浏览历史的列表数量
	History对象的back()和forward() 与浏览器的'后退'和'前进'一样
	history.go(-2) // 后退两个历史记录

### 浏览器和屏幕信息
####  Navigator
	Window对象的navigator属性引用的是包含浏览器厂商和版本信息的Navigator对象
	 包含appName、appVersion、
	userAgent HTTP头部发送的字符串
	platform 浏览器上的操作系统
***
	webkit: Safari或者Chrome
	opera: Operal 浏览器
	mozilla: Firefox火狐
	msie: IE

***
	onLine: navigaot.online 浏览器是否联网
	geoloaction: 获取用户地理位置
	cookieEnable() 若是浏览器永久保存cookies时，返回true
	
***
	 Screen: Window对象的screen属性是引用Screen对象，提供显示大小和可用颜色的信息


### 对话框
	Window对象有三种向用户提供简单的对话框。alert()向用户显示一条消息，并等待用户关闭对话框
	confirm() 显示一条消息，要去用户点击'确定'活'取消'按钮
	prompt() 显示一条消息，等待用户输入字符串，并返回一个字符串
	
***
	 HTML 文档经常使用<iframe>来嵌套多个文档，iframe创建的上下文用自己Window表示，每个iframe都有
	自己独立的Window对象
	打开窗口和关闭窗口
	Window对象的open()方法可以打开新的窗口，
```
function openNewWindow() {
    var iheight = 500
    var iWidth = 800
    var iTop = (window.screen.availHeight - iheight) / 2
    var iLeft = (window.screen.availWidth - iWidth) / 2

    var windowStyle = 'height='+iheight + ',width='+iWidth + ',left='+ iLeft + ',top='+iTop
    subwindow = window.open('url', 'newwindow', windowStyle)
    subwindow.moveTo(iLeft, iTop)
```
### get 传值
    var params = 'event=xxx'
    subwindow = window.open('url'+params, 'newwindow', windowStyle)
### post 传值
	创建表单提交到后台
```
var form = document.createElement('from')
form.setAttribute('method', 'post')
form.setAttribute('action', '提交url')
form.setAttribute('target', 'alarmConfigWindow')
	
var hiddenFiled = document.createElement('input')
hiddenFiled.setAttribute('type', 'hidden')
hiddenFiled.setAttribute('name', 'event.gid')
	
form.appendChild(hiddenFiled)
document.body.appendChild(form)
// 关闭窗口
// window.close()

}
openNewWindow()
```