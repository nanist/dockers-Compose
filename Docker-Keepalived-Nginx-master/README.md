# Docker-Keepalived-Nginx
centos环境利用docker快速构建Nginx+Keepalived双主集群实现负载均衡
https://blog.csdn.net/u010797364/article/details/129022103

**目录结构：**

- node_1 
  - docker-compose.yml
  - Dockerfile
  - keepalived.conf
  - nginx.conf
- node_2 同上

分别用于部署在两台不同的服务器上

**使用简介：**

- nginx.conf

  配置nginx以实现负载均衡，node_1和node_2一般情况下相同

- keepalived.conf

  默认使用双BACKUP，nopreempt非抢占模式，根据需要修改

  - interface # 当前进行vrrp通讯的网络接口卡(当前centos的网卡) 用ifconfig查看你具体的网卡
  - auth_pass # 指定认证方式。PASS简单密码认证(推荐),AH:IPSEC认证(不推荐)
  - unicast_peer # 另外一台的服务器ip
  - virtual_ipaddress # 定义虚拟ip(VIP)，可多设，每行一个

- docker-compose

  修改宿主机和nginx容器的端口映射

拷贝修改后的node_1和node_2到宿主机，进入目录执行

```shell
启动：
docker-compose -f docker-compose.yml up -d

停止：
docker-compose down

docker-compose logs


```







