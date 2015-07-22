# ショートコード

ショートコードとは、WordPressの投稿や固定ページ内に動的にコンテンツを挿入するための仕組みです。

例:

* `[contact-form-7 id="1234"]`
* `[map]スカイツリー[/map]`

## ここでのゴール

* `[permalink id="1234"]`というショートコードを記事に挿入したら、その記事のアイキャッチ画像とリンクが挿入されるプラグインを作成する。
* 記事のIDから様々な情報を取得するためのテクニックを学習する
* WordPressのエスケープについて学習する

## リファレンス

### ショートコードを登録するには？

```
add_shortcode( $tag, $func )
```

* `$tag` - ショートコードのタグ
* `$func` - コールバック関数

たとえば、`add_shortcode( 'permalink', 'permalink_shortcode' )`のように記述すると以下のようなショートコードが利用できるようになります。

```
[permalink]
```

https://developer.wordpress.org/reference/functions/add_shortcode/

### コールバック関数の書き方

前述した`add_shortcode()`の第二引数と同じ名前の関数を用意してください。

```
function permalink_shortcode( $atts, $content = '' ) {
    // do something

    return $something;
}
```

`$atts`には、ショートコードの属性が連想配列として格納されています。

たとえばショートコードが`[permalink post_id="1234"]`のように記述された場合、`id`属性に指定された値はコールバック関数で以下のように取得することができます。

```
function permalink_shortcode( $atts, $content = '' ) {
    $post_id = $atts['post_id']; // 1234

    if ( ! intval( $post_id ) ) { // $idは数値であるべき
        return '';
    }

    return $something;
}
```

### その他のTips

#### 記事のタイトルを取得する

指定されたIDの記事のタイトルを取得するには以下のように`get_the_title()`を使用してください。

```
$post_title = get_the_title( $post_id );
```

出力を行う際には`esc_attr()`などの適切なエスケープが必要です。

https://developer.wordpress.org/reference/functions/get_the_title/

#### 記事のパーマリンクを取得する

指定されたIDの記事のパーマリンクを取得するには以下のように`get_permalink()`を使用します。

```
$permalink = get_the_permalink( $post_id )
```

出力を行う際には`esc_url()`でエスケープをしましょう。

https://developer.wordpress.org/reference/functions/get_the_permalink/

### アイキャッチ画像を取得する

指定された記事のアイキャッチ画像は以下のように取得することができます。

```
if ( has_post_thumbnail( $post_id ) ) {
    get_the_post_thumbnail( $post_id )
}
```

https://developer.wordpress.org/reference/functions/get_the_post_thumbnail/
