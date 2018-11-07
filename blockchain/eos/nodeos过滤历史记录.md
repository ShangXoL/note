# nodeos添加过滤条件，获取指定账号的交易历史记录

1. 打开nodeos的配置文件（默认位置：/root/.local/share/eosio/nodeos/config）
2. 修改如下内容(注意看使用说明)：<br>

```
# Track actions which match receiver:action:actor. Actor may be blank to include all. Action and Actor both blank allows all from Recieiver. Receiver may not be blank. (eosio::history_plugin)
filter-on = youraccountname::
```