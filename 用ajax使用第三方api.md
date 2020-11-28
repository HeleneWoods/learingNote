由于跨域限制，无法直接引用第三方api，但可以通过后台php进行访问、获取数据，php文件的代码为：

```php
<?php
	$hotTopic = file_get_contents("https://www.v2ex.com/api/topics/hot.json");
	echo $hotTopic;
?>
```

然后直接在前端ajax中的open里引用php的url就好了。