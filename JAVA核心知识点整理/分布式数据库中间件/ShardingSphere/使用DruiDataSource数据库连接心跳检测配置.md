## 心跳检测配置
### 主库
sharding.jdbc.datasource.ds-master.testWhileIdle=true
sharding.jdbc.datasource.ds-master.timeBetweenEvictionRunsMillis=60000
sharding.jdbc.datasource.ds-master.testOnBorrow=true
sharding.jdbc.datasource.ds-master.initialSize=1
sharding.jdbc.datasource.ds-master.minEvictableIdleTimeMillis=300000
sharding.jdbc.datasource.ds-master.validationQuery=SELECT 1 FROM DUAL
### 从库
sharding.jdbc.datasource.ds-slave.testWhileIdle=true
sharding.jdbc.datasource.ds-slave.timeBetweenEvictionRunsMillis=60000
sharding.jdbc.datasource.ds-slave.testOnBorrow=true
sharding.jdbc.datasource.ds-slave.initialSize=1
sharding.jdbc.datasource.ds-slave.minEvictableIdleTimeMillis=300000
sharding.jdbc.datasource.ds-slave.validationQuery=SELECT 1 FROM DUAL


## 完整主从数据库配置示例

```properties
sharding.jdbc.datasource.names = ds-master,ds-slave
sharding.jdbc.datasource.ds-master.type = com.alibaba.druid.pool.DruidDataSource
sharding.jdbc.datasource.ds-master.driver-class-name = com.mysql.cj.jdbc.Driver
sharding.jdbc.datasource.ds-master.url = jdbc:mysql://mysql.hopson.com:3306/${mysql.database}?useUnicode=true&characterEncoding=utf8&tinyInt1isBit=false&serverTimezone=Asia/Shanghai&autoReconnect=true
sharding.jdbc.datasource.ds-master.username = hopsonone
sharding.jdbc.datasource.ds-master.password = hopsonone123
sharding.jdbc.datasource.ds-master.testWhileIdle = true
sharding.jdbc.datasource.ds-master.timeBetweenEvictionRunsMillis = 60000
sharding.jdbc.datasource.ds-master.testOnBorrow = true
sharding.jdbc.datasource.ds-master.initialSize = 1
sharding.jdbc.datasource.ds-master.minEvictableIdleTimeMillis = 300000
sharding.jdbc.datasource.ds-master.validationQuery = SELECT 1 FROM DUAL
sharding.jdbc.datasource.ds-slave.type = com.alibaba.druid.pool.DruidDataSource
sharding.jdbc.datasource.ds-slave.driver-class-name = com.mysql.cj.jdbc.Driver
sharding.jdbc.datasource.ds-slave.url = jdbc:mysql://smysql.hopson.com:3306/${mysql.database}?useUnicode=true&characterEncoding=utf8&tinyInt1isBit=false&serverTimezone=Asia/Shanghai&autoReconnect=true
sharding.jdbc.datasource.ds-slave.username = hopsonone
sharding.jdbc.datasource.ds-slave.password = hopsonone123
sharding.jdbc.datasource.ds-slave.testWhileIdle = true
sharding.jdbc.datasource.ds-slave.timeBetweenEvictionRunsMillis = 60000
sharding.jdbc.datasource.ds-slave.testOnBorrow = true
sharding.jdbc.datasource.ds-slave.initialSize = 1
sharding.jdbc.datasource.ds-slave.minEvictableIdleTimeMillis = 300000
sharding.jdbc.datasource.ds-slave.validationQuery = SELECT 1 FROM DUAL
sharding.jdbc.config.masterslave.name = ms
sharding.jdbc.config.masterslave.master-data-source-name = ds-master
sharding.jdbc.config.masterslave.slave-data-source-names = ds-slave


```
