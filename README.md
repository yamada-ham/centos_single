# 構築内容
```
Centos7
PHP v8.1
phpMyAdmin
MySQL v8
Composer
Node.js v16
gulp

1システム 1台
```


## ポートの追加方法
### 例:92番ポートを通す方法
<br>

./volumes/v_host.confに以下を追加

```
<VirtualHost *:92>
DocumentRoot /var/www/html/site92 #ポートを通すパスを指定
ServerName localhost92
</VirtualHost>
```
<br>

centos7/copy/httpd.confに以下を追加
の編集
```
Listen 92
```
<br>

centos7/docker-compose.ymlに以下を追加
```
ports:
- 8092:92
```

<br>

コマンド 再度ビルドして起動
```
$ docker-compose up --build -d
```

<br>

## MySQLのパスワード変更

初期パスワード確認
```
$ cat /var/log/mysqld.log | grep password
```

<br>

パスワードポリシー変更
```
# コマンドラインの場合
$ echo -e "validate_password.check_user_name=OFF\nvalidate_password.length=4\nvalidate_password.mixed_case_count=0\nvalidate_password.number_count=0\nvalidate_password.special_char_count=0\nvalidate_password.policy=LOW\n" >> /etc/my.cnf
# /etc/my.cnf をvimaなどで編集する場合
$ vim /etc/my.cnf
# Password policy change
validate_password.check_user_name=OFF 
validate_password.length=4
validate_password.mixed_case_count=0
validate_password.number_count=0
validate_password.special_char_count=0
validate_password.policy=LOW

# MySQL再起動
$ systemctl restart mysqld
```

<br>

パスワード変更
```
$ ALTER USER 'root'@'localhost' identified BY 'root';
```
