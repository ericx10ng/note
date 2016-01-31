

最近搭建服务状态统计系统，采用statsd协议，向telegraf发送统计数据，然后存入influxdb, grafana负责从influxdb读取数据并展示，告警采用kapacitor,
整个一套下来真是神器啊，太好用了；telegraf,influxdb,kapacitor都是influxdata公司出品。
美中不足是influxdb暂时没有group by后再top的操作

- statsd client
  负责发数据
  
- telegraf
  负责收数据
  
- influxdb
  负责存储数据
  
- grafana
  负责展示数据
  
- kapacitor
  负责监控告警
  


