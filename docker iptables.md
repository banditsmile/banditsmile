重启docker 容器的时候提示"docker iptables no chain/target/match by that name",在网上搜索了一下有不少出现这种情况的,最后分析原因发现是启动docker服务时没有启动iptables服务导致的(有些docker需要再iptables开放有些端口)

解决方法

1.启动iptables服务
2.重启docker服务
3.重启docker容器
