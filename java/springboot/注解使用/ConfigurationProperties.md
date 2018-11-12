## @ConfigurationProperties注解的使用

* 1.将配置文件的值注入到对象的属性中去
```
@Component("diceConfig")
@ConfigurationProperties("dice")
@Data
public class DiceConfig {

    private String  walletName;
    private String walletPwd;
    private String accountActive;
    private String chainUrl;
    private String walletUrl;


    private String  platAccount;
    private String eosCode;
    private String platCode;
    private String baseRollUnder;
    private String eosSymbol;
    private String platSymbol;
}
```