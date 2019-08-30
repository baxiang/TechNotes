####一级缓存
####二级缓存
二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的。
开启二级缓存
```
<settings>
<!-- 开启二级缓存的支持 -->
<setting name="cacheEnabled" value="true"/>
</settings>
```
因为cacheEnabled 的取值默认就为true，所以这一步可以省略不配置。为 true代表开启二级缓存;为false 代表不开启二级缓存。
