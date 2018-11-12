## ubuntu挂载数据盘步骤

* 一、查看没有挂载的数据盘

    ```
    fdisk -l
    ```

* 二、数据硬盘分区/dev/vdb

    ```
    fdisk /dev/vdb
    ```
  1. 依次输入 n 、p、 1、 回车、回车、wq
  2. 这里的VDB是我们上面看到数据硬盘的名称，如果你不是这个需要根据你真实的盘名称替换，如果是和我一样，那就直接复制。

* 三、ext3格式化分区

    ```
    mkfs.ext4  /dev/vdb
    ```


* 四、挂载新分区

+ 1. 创建要挂载的目录
        ```
        mkdir xxxcd 
        ```

  2. 挂载分区

        ```
        mount   /dev/vdb     /root/.local/share/eosio/nodeos/data
        ```

  3. 写入fstab 设置开机自动挂载，不设置重启之后挂载的不显示
        ```
        echo '/dev/vdb  /root/.local/share/eosio/nodeos/data  ext4  defaults 0 0' >> /etc/fstab
        ```

