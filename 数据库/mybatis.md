# mybatis

## 功能架构

![1562988855574](1562988855574.png)

## 原理详解

![1562988002462](1562988002462.png)

 MyBatis应用程序根据XML配置文件创建SqlSessionFactory，

SqlSessionFactory在根据配置，配置文件和Java代码的注解，获取一个SqlSession。

SqlSession包含了执行sql所需要的所有方法，

可以通过SqlSession实例直接运行映射的sql语句，

完成对数据的增删改查和事务提交等，用完之后关闭SqlSession。



## 注解开发

首先我们需要获取 **SqlSession**:

```java
SqlSession session = sqlSessionFactory.openSession(true);
```

参数设置为`true`表示开启自动提交模式

Session 在注解形式的使用方式如:

```java
ClusterMessage clusterMessage = new ClusterMessage();
clusterMessage.setClusterId(1);
clusterMessage.setClusterName("useName");
clusterMessage.setClusterTime(new Date().getTime());
clusterMessage.setClusterAddress("localhost");
clusterMessage.setClusterAccessUser("user");
clusterMessage.setClusterAccessPasswd("test");

session.getMapper(ClusterMessageMapper.class).updateClusterMessage(clusterMessage);
```

### 案例

表

```sql
CREATE TABLE cluster_manager(
    cluster_id int PRIMARY KEY AUTO_INCREMENT ,
    cluster_name VARCHAR(20) NOT NULL,
    cluster_time BIGINT NOT NULL ,
    cluster_address VARCHAR(50) NOT NULL,
    cluster_access_user VARCHAR(50) NOT NULL ,
    cluster_access_passwd VARCHAR(50) NOT NULL
)AUTO_INCREMENT=0;
```

Bean

```java
public class ClusterMessage {
    int clusterId = 0;                     // 集群ID
    String clusterName = "";               // 集群名称
    Long clusterTime = 0l;                 // 集群上线时间
    String clusterAddress = "";            // 集群内网访问地址
    String clusterAccessUser = "";         // 集群访问用户
    String clusterAccessPasswd = "";       // 集群访问密码

    public int getClusterId() {
        return clusterId;
    }
    public void setClusterId(int clusterId) {
        this.clusterId = clusterId;
    }
    public String getClusterName() {
        return clusterName;
    }
    public void setClusterName(String clusterName) {
        this.clusterName = clusterName;
    }
    public Long getClusterTime() {
        return clusterTime;
    }
    public void setClusterTime(Long clusterTime) {
        this.clusterTime = clusterTime;
    }
    public String getClusterAddress() {
        return clusterAddress;
    }
    public void setClusterAddress(String clusterAddress) {
        this.clusterAddress = clusterAddress;
    }
    public String getClusterAccessUser() {
        return clusterAccessUser;
    }
    public void setClusterAccessUser(String clusterAccessUser) {
        this.clusterAccessUser = clusterAccessUser;
    }
    public String getClusterAccessPasswd() {
        return clusterAccessPasswd;
    }
    public void setClusterAccessPasswd(String clusterAccessPasswd) {
        this.clusterAccessPasswd = clusterAccessPasswd;
    }

    @Override
    public String toString() {
        return "ClusterMessage{" +
                "clusterId='" + clusterId + '\'' +
                ", clusterName='" + clusterName + '\'' +
                ", clusterTime=" + clusterTime +
                ", clusterAddress='" + clusterAddress + '\'' +
                ", clusterAccessUser='" + clusterAccessUser + '\'' +
                ", clusterAccessPasswd='" + clusterAccessPasswd + '\'' +
                '}';
    }
}
```

Mapper

```java
public interface ClusterMessageMapper {

    // Insert
    @Insert("insert into cluster_manager(cluster_name, cluster_time, 	 cluster_address, cluster_access_user, cluster_access_passwd) " 
    +"values(#{clusterName}, #{clusterTime}, #{clusterAddress}, 	   	   #{clusterAccessUser}, #{clusterAccessPasswd})")
    @Options(useGeneratedKeys = true, keyColumn = "cluster_id", keyProperty = "clusterId")
    public void insertClusterMessage(ClusterMessage clusterMessage);

    // select
    @Select("select * from cluster_manager")
    @Results(
            id = "clusterMessage",
            value = {
           @Result(column = "cluster_name", property = "clusterName", 			 javaType = String.class, jdbcType = JdbcType.VARCHAR),
           @Result(column = "cluster_time", property = "clusterTime", 			 javaType = Long.class, jdbcType = JdbcType.BIGINT),
           @Result(column ="cluster_address",property="clusterAddress", 
           javaType = String.class, jdbcType = JdbcType.VARCHAR),
           @Result(column = "cluster_access_user", 							   property="clusterAccessUser", 
           javaType = String.class, jdbcType = JdbcType.VARCHAR),
           @Result(column = "cluster_access_passwd", property = 			   "clusterAccessPasswd", 
           javaType = String.class, jdbcType = JdbcType.VARCHAR)
            }
    )
    public List<ClusterMessage> getClusterMessage();

    @Select("select * from cluster_manager")
    @MapKey("clusterId")
    public Map<Integer, ClusterMessage> getClusterMessageMapper();


    @Select("select * from cluster_manager where cluster_id=#			{clusterId}")
    @ResultMap("clusterMessage")
    public ClusterMessage getClusterMessageById(@Param("clusterId") int 	clusterId);


    // update
    @Update("update cluster_manager set cluster_name=#{clusterName} 	where cluster_id=#{clusterId}")
    public void updateClusterMessage(ClusterMessage clusterMessage);

    // delete
    @Delete("delete from cluster_manager where cluster_id=#	{clusterId}")
    public void deleteClusterMessage(@Param("clusterId")int clusterId);
}
```



### 关键注解词

@Insert ： 插入sql , 和xml insert sql语法完全一样

@Select ： 查询sql, 和xml select sql语法完全一样

@Update ： 更新sql, 和xml update sql语法完全一样

@Delete ： 删除sql, 和xml delete sql语法完全一样

@Param ： 入参

@Results ： 结果集合

@Result ： 结果