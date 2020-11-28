## Cookie的特性

同一网站（同一域名）共享一套cookie

cookie文件大小非常小

拥有过期时间

## cookie在js上的应用

```javascript
document.cookie='user=helene';

document.cookie='password=12345'; //为添加，不是赋值，两条cookie都会被保存;当同名时将会覆盖。
```

\*无过期时间，浏览器关闭就会过期

```javascript
var oDate=new Date();
oDate.setDate(oDate.getDate()+14);
document.cookie='user=helened;expires='+oDate;
```

## 封装

设置cookie：

```javascript
function setCookie(name,value,iDay){
    var oDate=new Date();
    oDate.setDate(oDate.getDate()+iDay);
    document.cookie=name+'='+value+';expires='+oDate;
}
```

读取cookie：

```javascript
function getCookie(name){
    var arr=document.cookie.split("; ");
    for (var i in arr){
        var arr2=arr[i].split("=");
        if(arr2[0]==name){
            return arr2[1];
        }
    }
      return "";
}
```

删除cookie：

```javascript
function removeCookie(name){
    setCookie(name,1,-1);
}
```

