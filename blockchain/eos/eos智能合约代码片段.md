## eos智能合约代码片段

* 1.断言判断账号是否存在

```
  eosio_assert( is_account( to ), "to account does not exist");
```
* 2.合约内代币转币(code:shangxoliang,action,issue)

```
 action(
          permission_level{ N(charity), N(active) },
          N(shangxoliang), N(issue),
          std::make_tuple(bettor, quant, std::string("memo"))
      ).send();
```
* 3.合约内EOS转币

```
action(
        permission_level{ _self, N(active) },
        N(eosio.token), N(transfer),  //调用 eosio.token 的 Transfer 合约
        std::make_tuple(_self, bettor, payout, std::string(""))
  ).send();
```
* 4.添加表记录
```
void todo_contract::create(account_name author, const uint32_t id, const std::string &description)
{
  todos.emplace(author, [&](auto &new_todo) {
    new_todo.id = id;
    new_todo.description = description;
    new_todo.completed = 0;
  });

  eosio::print("todo#", id, " created");
}
```

* 5.修改表记录
```
void todo_contract::complete(account_name author, const uint32_t id)
{
  auto todo_lookup = todos.find(id);
  eosio_assert(todo_lookup != todos.end(), "Todo does not exist");

  todos.modify(todo_lookup, author, [&](auto &modifiable_todo) {
    modifiable_todo.completed = 1;
  });

  eosio::print("todo#", id, " marked as complete");
}
```

* 6.删除表记录
```
void todo_contract::destroy(account_name author, const uint32_t id)
{
  auto todo_lookup = todos.find(id);
  todos.erase(todo_lookup);

  eosio::print("todo#", id, " destroyed");
}
```

* 7.只允许拥有合约账号

```
require_auth(_self);
```

<br><br>
* 附录：可参考代码：

```
void token::transfer( account_name from,
                      account_name to,
                      asset        quantity,
                      string       memo )
{
    eosio_assert( from != to, "cannot transfer to self" );
    eosio_assert( is_account( to ), "to account does not exist");
    auto sym = quantity.symbol.name();
    stats statstable( _self, sym );
    const auto& st = statstable.get( sym );

    require_recipient( from );
    require_recipient( to );

    eosio_assert( quantity.is_valid(), "invalid quantity" );
    eosio_assert( quantity.amount > 0, "must transfer positive quantity" );
    eosio_assert( quantity.symbol == st.supply.symbol, "symbol precision mismatch" );
    eosio_assert( memo.size() <= 256, "memo has more than 256 bytes" );

    auto payer = has_auth( to ) ? to : from;

    sub_balance( from, quantity );
    add_balance( to, quantity, payer );
}
```

