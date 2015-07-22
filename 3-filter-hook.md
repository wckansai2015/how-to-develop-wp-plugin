# フィルターフック

フィルターフックとはWordPressの内部的な設定値やコンテンツなどをプラグインからカスタマイズするための仕組みです。

## ここでのゴール

* フィルターフックを使って投稿のコンテンツの後に、同じカテゴリーの記事の一覧を5件出力する。
* 特定の条件にマッチした記事のリストを取得するテクニックを学習する

## リファレンス

### フィルターフックの使い方

フィルターフックは以下のように`add_filter()`関数を使用します。

WordPressのテンプレートタグ`the_*()`には必ずと言っていいほどフィルターフックが用意されていますのでこれらを使用すると簡単に出力をカスタマイズすることができます。

```
add_filter( 'the_content', `add_similar_posts` )

function add_similar_posts( $content ) {
    // do something

    return $content;
}
```

上の例ではテンプレートタグ`the_content()`内に設置されたフィルターフックを使用して、その出力をカスタマイズするためのサンプルです。

この例では`add_filter()`を使用して、フィルターフックにコールバック関数を定義し、そのコールバック関数でコンテンツをカスタマイズしています。

コールバック関数の第1引数にはフィルターフックでカスタマイズするための元となる値が自動的に渡されます。

https://developer.wordpress.org/reference/functions/add_filter/

#### `get_posts()`で特定のカテゴリーの記事を取得する

特定のカテゴリーに所属する記事を取得するには`get_posts()`を使用します。

```
$args = array(
    'posts_per_page'   => 5,
	'offset'           => 0,
	'category'         => '1234', // カテゴリーのID
	'orderby'          => 'date',
	'order'            => 'DESC',
	'post_type'        => 'post',
	'post_status'      => 'publish',
	'suppress_filters' => true,
);
$posts = get_posts( $args );
```

この例では、`category`に所属する記事5件を最新順に取得しています。

$postsには、取得した記事のオブジェクトが配列で格納されていますので、それをループで処理することで記事の一覧等を生成することができます。

https://developer.wordpress.org/reference/functions/get_posts/

#### 記事が所属するカテゴリーを取得する

特定の記事が所属するカテゴリーを取得するには`get_the_category()`を使用してください。

```
$categories = get_the_category( $post_id );
```

https://developer.wordpress.org/reference/functions/get_the_category/
