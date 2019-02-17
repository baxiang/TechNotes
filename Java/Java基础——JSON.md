####org.json
配置pom.xml
```
<dependencies>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20160810</version>
        </dependency>
    </dependencies>
```
####put
```
      JSONObject xiaoming = new JSONObject();
        xiaoming.put("name","xiaoming");
        xiaoming.put("gender","男");
        xiaoming.put("age",20);
        xiaoming.put("hobby",new String[]{"羽毛球","爬山"});
        Object nullObject = null;
        xiaoming.put("comment", nullObject);
        System.out.println(xiaoming.toString());
```
json结果
```
{"gender":"男","name":"xiaoming","age":20,"hobby":["羽毛球","爬山"]}
```
####HashMap
```
       HashMap<String,Object> xiaoming = new HashMap<String, Object>();
        xiaoming.put("name","xiaoming");
        xiaoming.put("gender","男");
        xiaoming.put("age",20);
        xiaoming.put("hobby",new String[]{"羽毛球","爬山"});
        Object nullObject = null;
        xiaoming.put("comment", nullObject);
        System.out.println(new JSONObject(xiaoming).toString());
```
####JavaBean
```
public class Student {
    private String name;
    private  String gender;
    private  int age;
    private  String[] hobby;
    private  String comment;

    public String getName() {
        return name;
    }

    public String getGender() {
        return gender;
    }

    public int getAge() {
        return age;
    }

    public String[] getHobby() {
        return hobby;
    }

    public String getComment() {
        return comment;
    }
    
    public void setName(String name) {
        this.name = name;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setHobby(String[] hobby) {
        this.hobby = hobby;
    }

    public void setComment(String comment) {
        this.comment = comment;
    }
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", gender='" + gender + '\'' +
                ", age=" + age +
                ", hobby=" + Arrays.toString(hobby) +
                ", comment='" + comment + '\'' +
                ", ingorVal='" + ingorVal + '\'' +
                '}';
    }
}
```
```
       Student xiaoming = new  Student();
        xiaoming.setName("xiaoming");
        xiaoming.setGender("男");
        xiaoming.setAge(20);
        xiaoming.setHobby(new String[]{"羽毛球","爬山"});
        xiaoming.setComment(null);
        System.out.println(new JSONObject(xiaoming).toString());
```
####解析JSON 
```
        String jsonStr = "{\"gender\":\"男\",\"name\":\"xiaoming\",\"age\":20,\"hobby\":[\"羽毛球\",\"爬山\"]}";
        JSONObject jsonContent = new JSONObject(jsonStr);
        System.out.println("name="+jsonContent.getString("name"));
        System.out.println("age="+jsonContent.getInt("age"));
        System.out.println("hobby="+jsonContent.getJSONArray("hobby").getString(0));

```
####gson
```
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.8.5</version>
</dependency>
```
```
        Student xiaoming = new  Student();
        xiaoming.setName("xiaoming");
        xiaoming.setGender("男");
        xiaoming.setAge(20);
        xiaoming.setHobby(new String[]{"羽毛球","爬山"});
        xiaoming.setComment(null);
        System.out.println(new Gson().toJson(xiaoming));
```
注解 @SerializedName
```
 @SerializedName("Name")
    private String name;
    @SerializedName("Gender")
    private  String gender;
    @SerializedName("Age")
    private  int age;
    @SerializedName("Hobby")
    private  String[] hobby;
    @SerializedName("Comment")
    private  String comment;
```
```
        GsonBuilder builder = new GsonBuilder();
        builder.setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                char [] cs = name.toCharArray();
                cs[0]-=32;
                return String.valueOf(cs);
            }
        });

        System.out.println(builder.create().toJson(xiaoming));
```
transient 忽略某个字段
```
 private transient String ingorVal;
```
解析json
```

       String jsonStr = "{\"gender\":\"男\",\"name\":\"xiaoming\",\"age\":20,\"hobby\":[\"羽毛球\",\"爬山\"]}";
        Gson gson = new Gson();
        Student s = gson.fromJson(jsonStr,Student.class);
        System.out.println(s);
```
