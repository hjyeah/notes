### parameterType    
sql接收参数类型：（1）基本类型（key名字）（2）对象（属性名称作为key）（3）List（list作为key）（4）数组（array作为key）（5）Map   ------本质的传递过程都是一个map对象过去，所以parameterType 在不知道写什么时，写成"map"肯定不会出错。     
note：传入单个基本类型时，#{名字}可以任意指定（如果接口中的参数用@param标识，则需要具体参数名字匹配）；如果是多个参数值，可以按照#{0}，#{1}。。。顺序匹配；另外，list和array参数可以通过mybatis标签的迭代来实现；
*****
### 标签    
https://blog.csdn.net/weixin_40950778/article/details/78655288     
**insert**      
delete      
update       
select      
resultMap(resultType)      
foreach       
if        
choose      
where       
set      
trin         
collection            
association             
**sql**  
### 开发方式（接口+XML两种）
* 通过session.insert(xml-namespace.id....)、selectone(.........)、selectlist(...)等等
* 对应接口方式(sql语句可以在注解或者xml)，用session.getMapper(接口.class)，调用接口对应方法
### $与#的区别
#{}表示占位符，非数字型会加上''符号;$表示拼接字符串,比如 like '%${value}%'正常，用#{a}表示的话则要a值为 %XX% 这种,也常用于order by ${...} 等语句中`（要么不允许用户输入这些字段，要么自行转义并检验）`  
### resultmap和resulttype
sql数据返回的执行过程  
1. 通过JDBC查询得到ResultSet对象
2. 遍历ResultSet对象存储到hashmap中，key为结果集的字段名
3. 根据ResultMap标签的type属性通过反射实例化领域模型
4. 对应hashmap的key-value对应到实体中
1v1 association  mult v 1 collection， discriminator -鉴别器
resultType的本质也就是resultMap，类似property和column同命名方式
>      mybatis3.4.0开始加入了@Mapper注解,作用添加了@Mapper注解之后这个接口在编译时会生成相应的实现类
### mybatis缓存
https://www.jianshu.com/p/c553169c5921  
https://www.cnblogs.com/realshijing/p/8082598.html
* 一级缓存：默认开启，在SqlSession 层面进行缓存，即，`同一个SqlSession` ，多次调用同一个Mapper和同一个方法的同一个参数，只会进行一次数据库查询，然后把数据缓存到缓冲中，以后直接先从缓存中取出数据，不会直接去查数据库
* 二级缓存：默认不开启，`各个SqlSession对象`共享。默认二级缓存是不开启的，需要手动进行配置<cache ..... />(SQL 映射文件中)  
  `要使二级缓存有效的话必须显示的调用commit()方法，即使设置的是opensession(true)自动提交事务也不行`
* 自定义缓存:实现org.mybatis.cache.Cache 接口，写法`<cache type="com.domain.something.MyCustomCache"/>`
### typehandle类型处理器
https://www.cnblogs.com/dongying/p/4040435.html  
[typehandles推荐文章](http://blog.51cto.com/4443915/1828533)
* 实现TypeHandler接口
* 继承自BaseTypeHandler类(建议，不需要再去关心null的处理问题)
![typehandle匹配逻辑](https://github.com/hjyeah/notes/blob/master/typehandle.png)
```
@MappedJdbcTypes({JdbcType.VARCHAR})
@MappedTypes({Date.class})
public class MyDateTypeHandler extends BaseTypeHandler<Date> {
    //设置参数的方法，若需要，具体在insert和update时候设置类型转换器，其中由于编码关系，在blob等字段插入更新中不需要设置此转换，防止多次编码操作导致乱码
    public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType jdbcType) throws SQLException {
        preparedStatement.setString(i, String.valueOf(date.getTime()));
    }

    public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
        return new Date(resultSet.getLong(s));
    }

    public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
        return new Date(resultSet.getLong(i));
    }

    public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
        return callableStatement.getDate(i);
    }
}
```
@MappedJdbcTypes指定JdbcType类型，@MappedTypes指定JavaType的数据类型，实际也可根据泛型类型推算出  
