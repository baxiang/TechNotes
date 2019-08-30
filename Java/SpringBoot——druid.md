```
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://39.104.94.219:3306/houses?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=baxiang
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource

#连接池的设置
#初始化时建立物理连接的个数
spring.datasource.druid.initial-size=5
#最小连接池数量
spring.datasource.druid.min-idle=5
#最大连接池数量 maxIdle已经不再使用
spring.datasource.druid.max-active=20
#获取连接时最大等待时间，单位毫秒
spring.datasource.druid.max-wait=60000
#申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
spring.datasource.druid.test-while-idle=true
#既作为检测的间隔时间又作为testWhileIdel执行的依据
spring.datasource.druid.time-between-eviction-runs-millis=60000
#销毁线程时检测当前连接的最后活动时间和当前时间差大于该值时，关闭当前连接
spring.datasource.druid.min-evictable-idle-time-millis=30000
#用来检测连接是否有效的sql 必须是一个查询语句
#mysql中为 select 'x'
#oracle中为 select 1 from dual
spring.datasource.druid.validation-query=select 'x'
#申请连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
spring.datasource.druid.test-on-borrow=false
#归还连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
spring.datasource.druid.test-on-return=false

#合并多个DruidDataSource的监控数据
spring.datasource.druid.use-global-data-source-stat=true

spring.datasource.druid.maxPoolPreparedStatementPerConnectionSize=20
spring.datasource.druid.connectionProperties=druid.stat.mergeSql\=true;druid.stat.slowSqlMillis\=5000
spring.datasource.druid.web-stat-filter.enabled=true
spring.datasource.druid.web-stat-filter.url-pattern=/*
spring.datasource.druid.web-stat-filter.exclusions=*.js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico,/druid/*
spring.datasource.druid.stat-view-servlet.url-pattern=/druid/*
spring.datasource.druid.stat-view-servlet.allow=127.0.0.1
spring.datasource.druid.stat-view-servlet.reset-enable=false
spring.datasource.druid.stat-view-servlet.login-username=admin
spring.datasource.druid.stat-view-servlet.login-password=123456
```
