# アクションフック

アクションフックとは特定のイベントが発生した時に実行される処理のことです。

## ここでのゴール

* アクションフックを使ってWordPressに独自のCSSを追加する処理を学習する。

## リファレンス

### アクションフックの使い方

アクションフックは以下のように`add_action()`を使用してコールバック関数を指定します。

```
add_action( 'wp_enqueue_scripts', 'add_my_custom_style' );

function add_my_custom_style() {
    // do something
}
```

上の例では、`<head>`内でCSSやJavaScript用のタグを出力する際に発火するイベント`wp_enqueue_scripts`に対してコールバック関数を指定しています。

https://developer.wordpress.org/reference/functions/add_action/

### WordPressにCSSを追加する

WordPressにCSSを追加するには直接`<link>`タグを記述しないで`wp_enqueue_style`を使用することが推奨されています。

```
wp_enqueue_style(
    'my-custom-style' // ID
    plugins_url( 'css/my-custom-style.css', __FILE__ ), // CSSまでのURL
    array(), // 依存関係（このCSSよりも先にロードしてほしいCSSがあればそのIDを指定する）
    '1.0.0', // バージョン（リバースプロキシやキャシュ系プラグインを使う場合に重要）
    'all' // <link>たぐのmedia属性の値
);
```

https://developer.wordpress.org/reference/functions/wp_enqueue_style/
