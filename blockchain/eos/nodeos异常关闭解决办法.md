## nodeos异常关闭之后启动报错

* 解决办法：在启动命令加如下参数：
```
--hard-replay-blockchain
```