## 顯示當前使用者的grants
```sh
show grants;
# SHOW GRANTS FOR CURRENT_USER; # 這也是可以
```


## Allow all remote connections
```sql
GRANT ALL ON *.* to user@localhost IDENTIFIED BY 'password';
GRANT ALL ON *.* to user@'%' IDENTIFIED BY 'password';
```
兩者都必須建立，對mysql來說是不同帳號

參考: https://stackoverflow.com/questions/10236000/allow-all-remote-connections-mysql



## 創建帳號
IDENTIFIED可以在創建帳號時寫入，也可以在GRANT時一起附上，其實上面的指令也是創建帳號  
先創建再GRANT
```sql
CREATE USER 'user'@'localhost' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost' WITH GRANT OPTION;
```
* 注意這裡有沒有加`PRIVILEGES`都通，但從`show grants`指令所看到的結果來看，有加應該是比較正規的做法


## 移除帳號
`revoke all`並不會把使用者移除，`show grants`依然會有GRANT USAGE ON ...
```sql
revoke all, GRANT OPTION From 'user'@'localhost';
```
依然還是可以登入

要完全移除必須
```sql
DROP USER 'user'@'localhost'; 
```

## 情境:限制帳號只能讀取某張表格
``` sql
CREATE USER 'USER'@'IP' IDENTIFIED BY 'PASSWORD'
```
``` sql
GRANT SELECT ON database.`table` TO `USER`@`IP`
```

* 連線時報錯
  ```
  Client does not support authentication protocol requested by server; consider upgrading MySQL client
  ```
  原因是mysql8更改了密碼加密模式，由mysql_native_password改為caching_sha2_password  
  此案例為java程式要連遠端db，可能是java函式庫太舊
  ``` sql
  ALTER USER 'USER'@'IP' IDENTIFIED WITH mysql_native_password BY 'PASSWORD';
  FLUSH PRIVILEGES;
  ```
  或者創建時直接下`CREATE USER 'USER'@'IP' IDENTIFIED WITH mysql_native_password BY 'PASSWORD'`
