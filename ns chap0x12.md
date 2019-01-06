## 实战Bro网络入侵取证

### 实验环境

- kali(10.0.2.4)

### 实验过程

#### 1.安装bro

` apt-get install bro bro-aux`

#### 2.查看Bro信息

`lsb_release -a #查看系统版本信息`

`uname -a #查看系统名`

`bro -v #查看bro版本信息`

![2](image\2.png)

### 3.配置Bro

- 在/etc/bro/site/local.bro的文末增加两行代码 

![3](image\3.png)

- 在/etc/bro/site/目录下创建名为mytuning.bro的文件，写入redef ignore_checksums = T;忽略校验和认证。

![4](image\4.png)

### 4.分析Bro

- 下载包

`wget https://sec.cuc.edu.cn/huangwei/textbook/ns/chap0x12/attack-trace.pcap`

- 使用bro分析下载的包

`bro -r attack-trace.pcap /etc/bro/site/local.bro`

执行上面的代码后，你会看到/etc/bro/site/下面出现了很多新的文件。

![1](image\1.jpg)

- 利用virustotal分析得到的文件

进入extract_files文件夹，把extract_files文件夹里的文件上传至VirusTotal网站进行分析

![5](image\5.png)

- 寻找入侵的线索

打开usr/share/bro/base/files/extract/main.bro

我们发现文件名的最后一部分是files.log的唯一id

![6](image\6.png)

- 查看files.log

我们知道了文件名最后一部分就是files.log的唯一的id

![2](image\2.jpg)

- 查看conn.log

我们可以从中找到它的IP是98.114.205.102

![3](image\3.jpg)

### 5.用wireshark分析

- 查看主机信息

  ![4](image\4.jpg)

- 查看攻击者ip

攻击者ip就是98.114.205.102

![7](image\7.png)

- 查看攻击时间

攻击时间就是16.22秒

![5](image\5.jpg)