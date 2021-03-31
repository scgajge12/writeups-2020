# 2.DevTools_2

```txt
開発者ツールを使うと表示を書き換えることができます。
5000兆円欲しい！
(5000000000000000円持っていることにするとフラグを手に入れることができます。)
writer : suuhito
```

## Solution

問題文にあるように、開発者ツールで`あなたの総資産は5000000000000000円です！`に書き換えることでflagがアラートで表示されます。

そして該当の処理が行われているJSファイルにアクセスすると直接見ることができます。
```shell
$ curl http://localhost:30001/js/index.js
// 対象とするノードを取得
const target = document.querySelector('.card');

// オブザーバインスタンスを作成
const observer = new MutationObserver(_ => {
    const text = document.querySelector('.card-text').textContent;
    if (text.includes("5000000000000000")) {
        const flag = "FLAG{you_can_edit_html_using_devtools}";
        alert(flag);
        console.log(flag);
    }
});

// オブザーバの設定
const config = { childList: true, characterData: true, subtree: true };

// 対象ノードとオブザーバの設定を渡す
observer.observe(target, config);
```

## Flag

```txt
FLAG{you_can_edit_html_using_devtools}
```
