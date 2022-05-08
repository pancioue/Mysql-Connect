### Allow all remote connections
```sql
GRANT ALL ON *.* to user@localhost IDENTIFIED BY 'password'; 
GRANT ALL ON *.* to user@'%' IDENTIFIED BY 'password'; 
```
兩者都必須建立

參考: https://stackoverflow.com/questions/10236000/allow-all-remote-connections-mysql

### 移除帳號
```sql
DROP USER 'user'@'localhost'; 
```
這段指令並不會把`'user'@'%'`移除
