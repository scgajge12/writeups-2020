# 1.DevTools_1

```txt
ブラウザの開発者ツールを使ってソースコードをのぞいてみましょう！
writer : suuhito
```

## Solution

ソースコードにそのままflagがコメントアウトで埋め込まれています。

```shell
$ curl http://localhost:30001/
<!DOCTYPE html>
<html>
  <head>
    ...
  </head>
  <body>
    <nav class="navbar navbar-expand-sm navbar-light bg-light">
      <a class="navbar-brand" href="/">資産管理画面</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
    </nav>
    <div class="container">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">あなたの総資産</h5>
          <p class="card-text">あなたの総資産は0円です！</p>
        </div>
      </div>
    </div>
    <script src="/js/index.js"></script>
    <!-- FLAG{you_can_read_html_using_devtools} -->
  </body>
</html>
```

## Flag

```txt
FLAG{you_can_read_html_using_devtools}
```
