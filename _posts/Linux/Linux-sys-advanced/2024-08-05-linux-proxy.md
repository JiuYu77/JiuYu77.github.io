---
title: Linux代理（proxy）设置
description: Linux代理（proxy），设置环境变量。
author: yu
date: 2024-08-05 13:39:00 +0800
categories: [Linux, Linux系统-进阶]
tags: [Linux, proxy]
---


## 有关网络代理的环境变量

<table>
  <thead>
    <tr>
      <th>环境变量</th>
      <th>说明</th>
      <th>可选的取值</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>http_proxy</td>
      <td>http协议的网络连接使用该代理。</td>
      <td rowspan="4">ip:port<br/>http://ip:port<br/>socks://ip:port<br/>socks4://ip:port<br/>socks5://ip:port</td>
    </tr>
   <tr>
      <td>https_proxy</td>
      <td>https协议的网络连接使用该代理。</td>
    </tr>
       <tr>
      <td>ftp_proxy</td>
      <td>ftp协议使用该代理。</td>
    </tr>
   <tr>
      <td>all_proxy</td>
      <td>所有网络协议的网络连接都使用该代理；<br/>all_proxy变量的优先级低于以上变量(http_proxy等)。</td>
    </tr>
    <tr>
      <td>no_proxy</td>
      <td>无需代理的主机和域名，<br/>支持通配符，<br/>多个主机 / 域名之间使用逗号分隔。</td>
      <td>localhost,10.*,*.ad.com,</td>
    </tr>
  </tbody>
</table>

## proxy.sh

`port` 和 `port2`是端口

```bash
#!/bin/bash

arg=$1


set_proxy() {
  default_all_port=1089
  default_http_port=8889
  default_all_protocol=socks5


  read -p "Please enter all_proxy port ($default_all_port by default):" all_port
  all_port=${all_port:-$default_all_port}

  read -p "Please enter all_proxy protocol, socks5 or http ($default_all_protocol by default):" all_protocol
  all_protocol=${all_protocol:-$default_all_protocol}

  read -p "Please enter http_proxy port ($default_http_port by default):" http_port
  http_port=${http_port:-$default_http_port}

  # 小写
  # export all_proxy=socks5://127.0.0.1:$all_port
  export all_proxy=$all_protocol://127.0.0.1:$all_port

  export http_proxy=http://127.0.0.1:$http_port
  export https_proxy=http://127.0.0.1:$http_port

  # 大写
  # export ALL_PROXY=socks5://127.0.0.1:$all_port
  export ALL_PROXY=$all_protocol://127.0.0.1:$all_port

  export HTTP_PROXY=http://127.0.0.1:$http_port
  export HTTPS_PROXY=http://127.0.0.1:$http_port
}

unset_proxy() {
  unset all_proxy
  unset http_proxy
  unset https_proxy

  unset ALL_PROXY
  unset HTTP_PROXY
  unset HTTPS_PROXY
}

help() {
  echo "    help:"
  echo "       set      set proxy"
  echo "       unset    unset proxy"
  echo "       proxy    check proxy"
  echo "       help     help information"
  echo
  echo -e "use \e[32m. proxy.sh\e[0m or \e[32msource proxy.sh\e[0m"
}

if [[ $arg = 'set' ]];then
  set_proxy
elif [[ $arg = 'unset' ]];then
  unset_proxy
elif [[ $arg = 'proxy' ]];then
  env |grep -i proxy
else
  help
fi
```

使用：
```shell
. proxy.sh help
. proxy.sh set
. proxy.sh unset
. proxy.sh proxy
```

> 参考文献
> [Linux设置网络代理](https://blog.csdn.net/Dancen/article/details/128045261)