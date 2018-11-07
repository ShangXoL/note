### scatter转eos代币

+ 1.scatter能转代币吗？ 可以
+ 2.需要具备哪些知识？ eosjs

```
let transaction = () => {
    if (scatter.identity && identity.accounts != "") {
        const eosOptions = {expireInSeconds: 60};
        const eos = scatter.eos(network, Eos, eosOptions);
        const account = scatter.identity.accounts.find(x => x.blockchain === 'eos');
        const name = account.name;
        const authority = account.authority;
        console.log(authority)
        const transactionOptions = {authorization: [name + "@" + authority]};
        
        //转eos
        /*eos.transfer(name, 'shangxoliang', '0.0100 EOS', 'memo', transactionOptions).then(trx => {
            console.log(`Transaction ID: ${trx.transaction_id}`);
        }).catch(error => {
            console.error(error);
        });*/
        // eos.contract('shangxoleos1', {}).then(contract => contract.transfer())
        //--start
        //平台币code
        const platformcode="shangxoleos1"
        //平台接收地址
        const toAddress="shangxoliang";
        //转代币数量
        const quantity="3.0000 BAT";
        eos.transaction(
            {
                actions: [
                    {
                        account: platformcode,
                        name: 'transfer',
                        authorization: [{
                            actor: name,
                            permission: 'active'
                        }],
                        data: {
                            from: name,
                            to: toAddress,
                            quantity: quantity,
                            memo: 'token transfer test by shangxl'
                        }
                    }
                ]
            },{broadcast: true, sign: true}
        ).then(trx => {
            console.log(`Transaction ID: ${trx.transaction_id}`);
        }).catch(error => {
            console.error(error);
        });
        //---end
    } else {
        alert("请先登录 ")
    }

};
```
* 参考资料
<a href="https://www.npmjs.com/package/eosjs">eosjs</a>