## 转：Spring Boot形参Map并没有添加到类似于ModelAndView中，但是页面却可以取到相应的值？

 本节大纲：

（1）留言代码翻译
（2）问题分析
（3）Spring MVC数据模型
（4）写法延伸



```
   接下来看下具体的内容：
```

留言代码翻译：

我们在controller常写的代码是这样子的：

Java代码  
@RequestMapping("/index")    
public ModelAndView index(){    
   ModelAndView mv = new ModelAndView("/index");    
   mv.addObject("name", "[Angel -- 守护天使]");    
   return mv;    
}    

但是在我的好多代码里，却是这样子写的：

Java代码  
//http://127.0.0.1:8080/spring-boot/index2  ;  
@RequestMapping("/index2")    
public String index2(Map<String,Object> map){    
   map.put("name", "[Angel -- 守护天使]");    
   return "/index";    
}    

第一个代码使用了ModelAndView，第二个直接使用了map，那为什么第二个例子的参数name也能够在前端获取到呢，或者说为什么使用map也可以呢？这就是本篇文章要解决的问题。

 

问题分析：

我们看下ModelAndView的代码：

Java代码  
public class ModelAndView {    
     

```
/** View instance or view name String */    
private Object view;    
 
/** Model Map */    
private ModelMap model;    
 
/** Optional HTTP status for the response */    
private HttpStatus status;    
 
/** Indicates whether or not this instance has been cleared with a call to {@link #clear()} */    
private boolean cleared = false;    
//省略其它代码…    
```

}    

```
   在这里有一个属性ModelMap，这个很重要，我们在看里面的addObject，源码如下：
```

Java代码  
public ModelAndView addObject(String attributeName, Object attributeValue) {    
   getModelMap().addAttribute(attributeName, attributeValue);    
   return this;    
}    

```
   所以这里添加是使用了modelMap属性进行添加的，看下ModelMap源码：
```

Java代码  
public class ModelMap extends LinkedHashMap<String, Object> {    
     

```
/**  
 * Construct a new, empty {@code ModelMap}.  
 */    
public ModelMap() {    
}    
//省略其它代码…    
```

}    

```
   可以看出ModelMap继承了LinkedHashMap，而LinkedHashMap又继承了Map，所以这里map为什么也是可以设置前端属性值的，慢慢的就清楚了。
```

 

Spring MVC数据模型：

SpringMVC在调用方法前会创建一个隐含的数据模型，作为模型数据的存储容器， 成为”隐含模型”。如果处理方法入参为Map或者Model类型，SpringMVC会将隐含模型的引用传递给这些入参。

```
   spring Web MVC 提供Model、Map或ModelMap让我们能去暴露渲染视图需要的模型数据。

   看如下的一段很有趣的代码：
```

Java代码  
@RequestMapping(value = "/index3")    
public String index3(Model model, Map<String,Object> model2, ModelMap model3) {    
    model.addAttribute("a", "a");    
    model2.put("b", "b");    
    model3.put("c", "c");    
    System.out.println(model == model2);    
    System.out.println(model2 == model3);    
    return "index3";    
 }    



```
   控制台的打印是：
```

true
true



```
   虽然此处注入的是三个不同的类型（Model model, Map model2, ModelMap model3），但三者是同一个对象。

   AnnotationMethodHandlerAdapter和RequestMappingHandlerAdapter将使用BindingAwareModelMap作为模型对象的实现，即此处我们的形参（Model model, Map model2, ModelMap model3）都是同一个BindingAwareModelMap实例。

   我们跟踪源代码可以发现：
```

BindingAwareModelMap extends ExtendedModelMap

而ExtendedModelMap：

ExtendedModelMap extends ModelMap implements Model

```
   所以到头来这些都是一个引用，就可以解释的通了。
```

 

ModelAndView特别说明：

```
   我们会发现上面并没有过多的提到这个，ModelAndView：是包含ModelMap 和视图对象的容器。正如名字暗示的一样既包含模型也包含视图，而ModelMap只是包含模型的信息。

   一旦你知道这些的话，这些涉及到的类就可以比较好的理解了。
```

 

写法延伸：

```
   从上面的讲解中，我们在controller可以有很多种写法，接着往下看：
```

（1）写法1：String写法

Java代码
@RequestMapping(value = "/index4")    
    public String index4() {    
        return "index4";    
     }    

（2）写法2：String加Map写法

Java代码  
@RequestMapping(value = "/index5")    
public String index5(Map<String,Object> map) {    
   map.put("name", "Andy");    
    return "index5";    
 }    

（3）写法3：String加ModelMap写法

Java代码  
@RequestMapping(value = "/index6")    
public String index6(ModelMap modelMap) {    
   modelMap.addAttribute("name","Andy-2");     
    return "index5";    
 }    

（4）写法4：String加接口Model写法

@RequestMapping(value = "/index7")

```
public String index7(Model model) {

   model.addAttribute("name","Andy-3"); 

    return"index5";

 }
```

 

（5）写法5：String加大杂烩写法

Java代码  
@RequestMapping(value = "/index6")    
public String index6(ModelMap modelMap) {    
   modelMap.addAttribute("name","Andy-2");     
    return "index5";    
 }    



（6）写法6：ModelAndView写法

Java代码  
@RequestMapping("/index")    
public ModelAndView index(){    
   ModelAndView mv = new ModelAndView("/index");    
   mv.addObject("name", "[Angel -- 守护天使]");    
   return mv;    
}    

```
   好了，可能还有别的写法，就到这里吧。那么这多的写法，我们用哪一种呢？所以第一一个项目要稍微保持统一，选择一到二两种就可以了，博主比较喜欢的就是Map和ModelAndView这两种方式了。
```



本文转至林祥纤的【Spring Boot形参Map并没有添加到类似于ModelAndView中，但是页面却可以取到相应的值？】（http://412887952-qq-com.iteye.com/blog/2396299） 一文。

文章标签： SpringBoot
个人分类： Java