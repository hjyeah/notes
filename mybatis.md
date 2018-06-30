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

