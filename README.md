# fabric 2.3 for Raspberry Pi
fabric 2.3版本的docker容器主要基于alpine版本，这个版本支持arm，arm64,理论上支持Raspberry Pi 3B 或Raspberry Pi 3B+。
但是官方并没有针对此平台进行优化，同时有很多库依赖问题导致编译出错，此版本对fabirc-2.3进行修改，适配，可直接编译，没有花里胡哨的东西。
# 编译二进制
将此项目clone到本地
```bash
git clone -b release-2.3 https://github.com/bluarry/fabric_raspypi.git
cd fabric_raspypi
make release
```
如果环境配置没问题，以上命令即可编译成功.
# 编译docker
```bash
# 和amd64编译命令一样
make docker 
```

如果以上命令都没有出错，那么恭喜你，fabric2.3可以在你的机器上跑了。


