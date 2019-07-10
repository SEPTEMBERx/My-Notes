# Exception

2019-06-07 23:22:23.643 ERROR 6352 --- [nio-8084-exec-1] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.type.TypeException: Could not set parameters for mapping: ParameterMapping{property='cookie', mode=IN, javaType=class java.lang.String, jdbcType=null, numericScale=null, resultMapId='null', jdbcTypeName='null', expression='null'}. Cause: org.apache.ibatis.type.TypeException: Error setting non null for parameter #1 with JdbcType null . Try setting a different JdbcType for this parameter or a different configuration property. Cause: org.h2.jdbc.JdbcSQLDataException: Invalid value "1" for parameter "parameterIndex" [90008-199]] with root cause



 remove the single quotes around '#{date,jdbcType=DATE}'.    **WORKED** 

> Try to change #{date,jdbcType=DATE} to ${date,jdbcType=DATE}  NOTWORK

#### 

