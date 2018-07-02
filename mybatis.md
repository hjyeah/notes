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
