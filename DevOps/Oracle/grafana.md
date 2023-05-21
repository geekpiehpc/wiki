## Oracle 被主办方自带高了一个 telegraf
我们用另一个端口和 binary 部署了一个 grafana 机器放实时机器信息，后来发现这东西延时只能当历史记录看，运维又老是掉SSD，Ceph over HDD 没有 replica 真不可靠，还是不弄了。

后台大致长这样：
![](https://studentclustercompetition.us/2021/Grafana.jpg)