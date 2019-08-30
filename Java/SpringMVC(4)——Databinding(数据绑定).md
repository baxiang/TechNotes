[http://localhost:8080/basetype?uid=100](http://localhost:8080/basetype?uid=100)
```
@ResponseBody
    @RequestMapping("/basetype")
    public String baseType(@RequestParam(value = "uid",required = false) Integer id){
        return "uid:"+id;
    }
```
