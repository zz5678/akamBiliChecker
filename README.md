# akamBiliChecker

测试Bilibili海外CDN -- upos-hz-mirrorakam.akamaized.net 对应不同 CDN IP 的**下载速度**，可以批量，或检测目前正在使用的解析 IP。

受到[akamTester](https://github.com/miyouzi/akamTester)启发，制作此工具，但不同在于，本工具检测**直接连接的下载速度**，而不是仅仅测试连接延迟。

用户如需要**测试通过代理下载速度**，请自行修改脚本内 `curl` 代理设置即可。

## 安装

本工具使用 shell 脚本和 [ykdl](https://github.com/zhangn1985/ykdl)，请首先安装如下命令和软件包：

shell 工具：`gnu-awk gnu-sed jq curl`

**目前脚本在 `bash` 下运行**

Mac OS 上建议安装 `gnu` 系列命令，如使用Mac OS原装的BSD sed和awk，请自行修改脚本内命令和参数。

安装 ykdl（需要python环境）：`pip / pip3 install https://github.com/zhangn1985/ykdl/archive/master.zip`

## 配置

### 代理配置：

**如果你已经身在国外，将脚本`akamBiliChecker.sh`第三行改为：** 

```proxy=""```

**如果你在国内使用，需要使用海外代理，支持http和socks代理，写法分别如下：**

```proxy="http://127.0.0.1:1087"```

或

```proxy="socks5://127.0.0.1:1080"```

切记shell脚本中不要随便增加空格。

### IP 列表配置：

本项目下附带四个文件分别来自

1. [https://www.whatsmydns.net/#A/upos-hz-mirrorakam.akamaized.net](https://www.whatsmydns.net/#A/upos-hz-mirrorakam.akamaized.net)

2. [https://www.ping.cn/dns/upos-hz-mirrorakam.akamaized.net](https://www.ping.cn/dns/upos-hz-mirrorakam.akamaized.net)

3. 以及[https://www.17ce.com/cdn](https://www.17ce.com/cdn) 进行查询后的 IP 列表的**手动获得**，后者获得 IP 数量要多于前者。譬如这次解析结果：[https://www.17ce.com/site/resolveip/20200823_9c392ef0e50211eaa3efcf9d6c96c620:1.html](https://www.17ce.com/site/resolveip/20200823_9c392ef0e50211eaa3efcf9d6c96c620:1.html)

4. 以及来自[akamTester](https://github.com/miyouzi/akamTester)的 akamTester_vX.0_ip_list

由于可能会过期失效，请用户自行重新解析整理成可用 IP 列表。
由于 whatsmydns 解析IP不是太全，因此放弃了自动获取 IP 列表的作法，请用户自行去获取 IP 列表，并制作成列表文件进行测试，或者使用本作携带的几个 IP 列表。

## 使用

1. 无参数直接执行命令，测试本机目前配置 IP：
   `./akamBiliChecker.sh`

2. 带多个 IP 作为参数，测试并排序，例如：

   ```
   ./akamBiliChecker.sh 124.124.40.18 203.69.141.27 188.43.75.8
   ```

3. 带 IP 列表文件，使用 `-f` 参数，测试后排序：

   ```
   ./akamBiliChecker.sh -f whatsmydns.net_iplist_20200810.txt
   ```

   

测试命令行最后会出现排序结果，类似：

```686114 200 184.50.87.74
688206 200 23.210.202.25
753695 200 45.64.21.11
818610 200 203.69.141.26
966731 200 23.210.202.17
```

第一列数字是下载速度，选取数字越大越好。
第二列数字200是测试正常，非200的结果是不可用结果。
第三列是cdn的对应解析 IP。

`result_17ce.txt` 是某次测试的结果，仅供观摩，**由于和你网络配置不同，此结果不要随意取用**。