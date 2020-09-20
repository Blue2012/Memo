【環境】
　AWS EC2
  CentOS7
  MySQL 8.0

MySQLのインストールが終わって、ログインしようとすると、アクセスDenyが出る。
これを解消するには以下の手順が必要。

```
vi /etc/my.cnf
```

![image](https://user-images.githubusercontent.com/18514297/93692596-18206680-fb30-11ea-8a8f-3160f4aad8da.png)

skip-grant-table

を追加して、MySQLのサービスを再起動。

```
systemctl restart mysqld
```

これでログインできるようになるが、この状態だと特権が必要な操作をするとエラーになってしまう
（このモードでは実行できません的なやつ）

なので、いったん、以下のコマンドを実行して、rootのパスワード変更、root以外のユーザー作成を済ませてしまう。

```
FLUSH PRIVILEGES;
```

ただし、以下のエラーが出力されるケースがあるので、エラーが出力された場合は以下のように対処する。


```
【出力されるエラー】
ERROR 1146 (42S02): Table 'mysql.user' doesn't exist
```

まずはSQLから抜けて、以下のコマンドをSSHから入力。

```
mysqld  --initialize --user=mysql
```

参考URL
https://www.field-note.net/linux/480/
