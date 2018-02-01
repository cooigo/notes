# SpringMVC controller控制页面跳转

- 方法1：controller的处理方法返回类型设置为String，方法return "redirect:" + YOURLINK

```java

    @RequestMapping(method = RequestMethod.GET)  
    public String initForm(@RequestParam("link") String link) {  
    return "redirect:" + link;  

```

- 方法2: controller的处理方法返回类型设置为ModelAndView

```java

public ModelAndView redirect(@RequestParam("link") String link) {  
    ModelAndView view = new ModelAndView();  
    view.setViewName("redirect:http://172.24.208.168/Default.aspx");  
    return view;  
}  

public ModelAndView redirect(@RequestParam("link") String link) {  
    return new ModelAndView(new RedirectView(link));   
}  

```

# 避免jpa 查询自动更新
- 创建新对象，使用BeanUtils.copyProperties