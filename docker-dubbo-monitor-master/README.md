## Dockerfile 编译说明/注意事项
- 从github上checkout，不要下载 github 打好包的 2.6 代码包
- openjdk 和 oracle jdk 编译有差异
    - 代码 dubbo-config-spring 使用了 sun 的包， openjdk 编译失败，编译前要删除。   
    - 前者 assemble zip时含 116个 jar 包，后者 78个。对比后删除多余的，不然会报“类重复”错误
- start.sh 使用了 nohup 在后台运行，docker-compose 可以加 tty: true 保持容器运行。但如果 docker 版本太旧，不支持。所以增加 tail 命令到 start.sh
- start.sh 使用了 ps, netstat 等命令，要使用 apt-get 安装

## 运行说明
- 使用 github 提供的 docker-compose.yml 启动
- 注意要传入系统环境变量 ZOOKEEPER_ADDRESS