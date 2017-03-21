#一、前台传后台

##A.前台传后台

### Ajax+Json对象
```
var testurl = '/workteam/staff/forp.do';
$("#testResult") .click(function(){
	$.ajax({
		type : "POST",
		dataType : "json",
		url : testurl,	
		//数据为自己构造的。	
		data:{
			"oldrole":oldrole,
			"newrole":newrole,
			"staffcode":zmstaffcode,
			"staffname":zmstaffcode,
			"pcode":pcode
		},
		success : function(result){
			alert(result.entity[1].id);
			/*toastr.error(,'警告信息');*/
		}
	});
});
```

### 无Ajax+序列化
```
var formData = $(".form-horizontal").serialize();//表格数据来自于序列化
$.post(testurl,formData,function(result){
	if(result != 0){
	//dosomething
	}else{
	//dosomething
	}
});
```


##B.后台解析


###1.使用HttpServletRequest的getParameter：
```
HttpServletRequest request = getRequest();//getRequest()内部细节暂时不讨论
String name= request.getParameter("name");
```
###2.直接使用Spring MVC的注解：
```

public void get(@RequestParam String name）
```
###3.后台解析json对象

```
JSONObject json=JSONObject.fromObject(request.getParameter("data"));
String name = new String(json.getString("name"));
```






#二、后台传前台


## 1.@ResponseBody
```
@SuppressWarnings("unchecked")
@RequestMapping(value = "/test.do", method = RequestMethod.POST)
public @ResponseBody Map test(@RequestParam String name）{
	Map map = new HashMap();
	return map;
}
```

##2.构造输出流输出
```
@SuppressWarnings("unchecked")
@RequestMapping(value = "/test.do", method = RequestMethod.POST)
public void test(@RequestParam String name）{
	Map map = new HashMap();
	JSONObject result =   JSONUtil.toJSONObject(map);
	getOut().print(result);//构造输出流输出,细节省略
}
```
