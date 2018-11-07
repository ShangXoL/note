# 使用screen守护进程软件启动eosio节点nodeos
+ 1.安装screen<br>
``apt-get install screen``
+ 2.创建任务<br>
 ``screen -S eosio``
+ 3.在screen任务中运行nodeos启动命令<br>
`` screen nodeos``
+ 4.查看screen所有任务<br>
``screen -ls``
+ 5.恢复screen任务<br>
``screen -r <session>``
