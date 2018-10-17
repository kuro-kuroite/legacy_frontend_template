# legacy_frontend_template

Stylus(CSS3), HTML5, ES6, Atomic Design, BEM or FLOCSS を使用した開発環境構築用のテンプレート．このテンプレートではあえて，レガシーなHTML5, CSS3, ESNext を採用し，BEM等の命名規則とディレクトリ構造を保証する．2015年以降の流れである React.js Vue.js 等のコンポーネント志向とBabel, Webpack での複雑で柔軟性の高いbuild環境に至った経緯までを想定している ．単純な構造と規則を採用することで，コンポーネント志向の環境に置換できるようにしていく

## Over view

src/images にある **/*.{png, jpg, ...} を dist/images/*.{png, jpg, ...} にバンドル

src/pug にある *.pug を pug 経由で dist/html/*.html にバンドル

src/stylus にある *.styl を stylus 経由で dist/css/style.css にバンドル

## 出来ること

npm scirpt にある機能. 基本的に，`yarn dev:watch` と `yarn deploy` で事足りる

自動的に src/{images, pug, stylus} ファイルを監視しつつ，コーディング規約に合わせたあと，dist/ 以下に deploy

```bash
yarn dev:watch
```

src/{images, pug, stylus} ファイルを，dist/ 以下に deploy

```bash
yarn deploy
```

stylus, pug, html ファイルをコーディング規約に合わせて, 自動的に修正

```bash
yarn lint:watch
```

<補足> stylus, pug, html ファイルをコーディング規約に合わせて修正

```bash
yarn lint
```

## for developer

npm-scripts として，以下のスクリプトを定義すると，自動的に, `yarn dev:watch`, `yarn lint:watch` に組み込まれる

- lint:[language]:fix
  - \[linter]:\*:fix
- lint:[language]:fix:dry
  - \[linter]:\*:fix:dry

### ファイル階層

#### 概要

ファイル構造として，役割ごとに最小の単位であるコンポーネントに「分割」し，複数のコンポーネントを使って一つのより大きな役割へと「統合」するやり方を採用する．役割ごとに「分割」し，管理しやすくしながら，それらを組み合わせていく「統合」で再利用性も高くなるファイル構造を目指す．

### Atomic Design

基本的に，src/{images, pug, stylus} は「Atomic Design」で「分割」と「統合」をする．Atomic Design とは，化学の「原子，分子，有機体, ...」のようなイメージで個々の要素を組み上げていく考え方だ．すなわち，コンポーネント単位である「atoms, molecules, organisms, templates, pages」を階層的に作り上げる設計手法となる．

### Pug, Stylus の採用

今回は，HTML変換言語である Pug と CSS変換言語である Stylus を採用した．なぜなら，より大きな役割へと「統合」するには，別のファイルを取り込む機能が必要となり，素の HTML5, CSS3 では力不足であるためだ．Pug では，`include path/to/component/_partial.{html, pug}` で，Stylus では，`@import 'path/to/component/style.{css, ''}';` の構文を使用できる．

### 実装方法

#### Stylus(Css)

src/stylus/ の中を使用する．style.styl が `yarn deploy` で dist/css/style.css に変換される．

    _base/
      _setting.styl
        プロパティ以外の設定項目を定義
        I18n, L10n, A11y, ...(今後変更可能性あり)

      _properties.styl
        - Value層: 色相名とカラーコードの紐づけなど，ある値に対する別名を定義する
        ex)
          color-black = #333;
          color-blue = #094ab2;
          color-green = #51c300;
          color-red-pale = #d03c56;
          color-yellow = #ffce08;
          color-white = #f4f5f7;
        - Branding層: サービス全体で値の意味を統一するために，Value層の値に意味づけした名前を定義する
        ex)
          color-base = color-white;     // 70%
          color-main = color-blue;      // 25%
          color-accent = color-yellow;  // 5%
          color-danger = color-red-pale;
          color-success = color-green;
          color-waining = none;
        - Media層: Web, iOS, 紙など，それぞれの媒体で固有に使用される一般的な要素に値を定義する．
        ex)
          color-link = color-red-pale;
          color-link-active = color-link;
          color-link-hover = color-link;
          color-text = color-black;
          color-text-outlined = color-white;
          color-input-placeholder = color-text;
          color-input-outlined = color-black;
          color-header = color-main;
          color-footer = color-main;
          font-size-xxs = none;
          font-size-xs = 0.6rem;
          font-size-m = 1rem;
          font-xxxxl = 2.0rem;
          font-weight-default = 400;
          font-weight-bold = 700;
            :
          border-xxx
          radius
          hover-feedback-opacity
          hover-animation
          fade-animation
            :
          z-header = 10;
            :
          ios-xxx
          android-xxx
          media query
            :


      _mixins.styl
        Stylus 言語にある，関数みたいな形でスタイルの拡張が出来る機能 mixin が集まったファイル
        ex)

          border-radius(n = radius)
            -webkit-border-radius n
            -moz-border-radius n
            border-radius n

      _reset.styl
        User Agent Style Sheetで定義されているブラウザ独自のスタイルをリセットしたり，ノーマライズしたりするスタイルを定義する
        - Eric Meyer's reset.css
        - HTML5 Doctor CSS Reset
        - Yahoo! (YUI 3) Reset CSS
        - Normalize.css
        - Reboot.css
            :

      _base.styl
        *, *::before, *::after, html, body といった汎用的なタグについてのスタイル定義
        ex)
          html {
            font-size: 62.5%;
          }

          *,
          *::before,
          *::after {
            box-sizing: border-box;
          }

          body {
            background: color-base;
            color: color-text;
            font-size: 1.2rem;
            font-family: Raleway, 'Hiragino Kaku Gothic ProN', Meiryo, sans-serif;
            letter-spacing: 1px;
          }

      [tag]/style.styl
        [tag]: a, img, input, ... 等の一般的な HTML タグ
        基本的な HTML タグのスタイルを定義する．自作のNormalize.cssのような想定
        ex) a/style.styl
          a {
            &:link,
            &:visited,
            &:hover,
            &:active {
              color: color-link;
              text-decoration: none;
            }
          }

    atoms/
      [Component]/style.styl
        [Component]: Button, Img, TextBox, Balloon, Loading,NumberBadge, Dialog, Pagination ... と .Button, .Img に追加したいスタイルを定義. atomsでは，それ以上UIとしての機能性を破壊しない最低限の粒度とする．atomsによってUIやアニメーションの統一感を保証する

        tip) ../[tag]/style.styl にある tag スタイルを extends / mixin してもよい

        ex) Button/style.styl
          .Button {
            @extends button;

            &:hover {

            }
          }

    molecules/
      [ConcreteComponent]/style.styl
        [ConcreteComponent]: DeleteButton, IntroduceCard, SearchForm, FollowBadge, AlertDialog, ProgramNavigation, UserInformation, PostReaction ... と .DeleteButton, .MailForm に追加したいスタイルを定義. moleculesでは，ユーザの動機に対する機能を提供する粒度とする．すなわち，何のために，ボタンをクリックしたり，テキストを入力したりするのかを，名前に含める役割を持ち，atomsより具体化される．(SearchButton(button for search), FollowBadge(badge for showing follower number)). また，molecules も独立して存在することはできない． あくまで，organisms の一部として組み込まれ，その機能を助けるヘルパーとしての存在意義が強い．molecules によって，ユーザの行動を阻害しない操作性を保証する．

        tip) ../atoms/[Component]/style.styl にある .Component スタイルを extends / mixin してもよい

        ex) DeleteButton/style.styl
          .DeleteButton {
            @extends .Button;
            background-color: color-danger;

            &:hover {

            }
          }

    organisms/
      [CompositeConponent]/style.styl
        [CompositeConponent]: Header, Footer, Sidebar, Post, PostList, Notification, NotificationList, ... と .Header, .PostList に追加したいスタイルを定義． organismsでは， コンポーネントとして独立するコンテンツを提供できる粒度とする． molecules のコンポーネントを組み合わせることで，独立した1つのコンテンツとして自由にページのレイアウトができる．すなわち，organism は，template と呼ばれる仮想のページ上で配置を考えるときの，スタンドアローンなコンテンツと同じである．organism によって，ユーザの行動を促すコンテンツを保証する．

        tip) ../atoms/[Component]/style.styl にある .Component スタイルを extends / mixin してもよい． ../molecules/[ConcreteComponent]/style.styl にある .ConcreteComponent のスタイルの構造を模倣しながら，スタイルを更新する

        ex) DeleteAccount/style.styl
          .DeleteAccount {
            display: flex;
            flex-direction: row;

            & .TextBox {
              order: -1;
              margin-right: 10px;
            }
            & .DeleteButton {

              &:hover {

              }
            }
          }

    templates/
      [pageTemplate]/style.styl
        [pageTemplate]: IndexTemplate, AboutTemplate と .IndexTemplate, .AboutTemplate に追加したいスタイルを定義． templatesでは， HTMLのページ単位の粒度とする． organism のコンポーネントを配置することで，ページのレイアウトをする．templatesのタグに入るコンテンツの中身は「Lorem Ipsum ...」や「default image」とする．templateの目的は，中身のコンテンツにかかわらず正しい配置となっているのかを表示が正しいことを保証するために存在する．すなわち，template によって，画面全体のレイアウトを保証する．

        tip)  ../organism/[CompositeComponent]/style.styl にある .CompositeComponent のスタイルの構造を模倣しながら，スタイルを更新する.

        ex) IndexTemplate/style.styl
          .IndexTemplate {
            display: grid;

            & .Header {
              grid-area: 'header';
              margin-bottom: 10px;
            }

            & .Sidebar {
              grid-area: 'footer';
              margin-right: 10px;
            }

            & .DeleteAccount {
              display: hidden;
            }
          }

    pages/
      [pagePage]/style.styl

        基本的には，templates のスタイルを  @import しながら作成する．すなわち，スタイルを書くことは基本的にはない. ただし，background-imageでimageのパスをurl()で更新することはあるだろう．

#### Pug(HTML)

src/pug/ の中を使用する．*.pug が `yarn deploy` で dist/html/*.html に変換される．

    [page].pug
      [page]: index, about といった各ページの pug を定義．
      基本的には，organism/[ConpositeComponent]/_partial.html を include 構文で組み合わせる．

    atoms/
      [Component]/_partial.html
        note) 基本的には，1つのタグで完結することが多いので，includeの手間とあまり変わらない．

        ../../stylus/atoms/[Component]/style.styl で再利用したいHTMLを定義する．../[page].pug で include atoms/

    molecules/
      [Component]/_partial.html

        ../../stylus/molecules/[ConcreteComponent]/style.styl で再利用したいHTMLを定義する．include atoms/[Component]/_partial.html を使用することがある．

    organisms/
      [Component]/_partial.html

        ../../stylus/organisms/[CompositeComponent]/style.styl で再利用したいHTMLを定義する．include molecules/[Component]/_partial.html を使用することがある．

    templates/
      [pageTemplate]/_partial.html
        note) 使用しないことを推奨する．templates層は，pages層でコンテンツを動的に変更するのを想定しているため，HTML, CSS の静的言語だけでは，実用できない．よって，何かしらのJSライブラリ(React.js等)を使用しない限りは，基本的にHTMLを書くことはないだろう

        templatesのタグに入るコンテンツの中身は「Lorem Ipsum ...」や「default image」とする．

    pages/
      [pagePage]/_partial.html
        note) 使用しないことを推奨する．templates のHTML をESNext や サーバサイド言語で中身のコンテンツを動的に書き換えながら生成する必要がある．よって，何かしらのJSライブラリ(React.js等)を使用しない限りは，基本的にpages のみでHTML, pugを書くのを推奨する．すなわち，templates のpugを includeすることはないだろう．

#### Image

src/images/ の中で使用する． `yarn deploy` で dist/images/ に移動する．

src/images/**/ に画像をコピーする．Pug(HTML)やStylus(CSS) で画像を使用したいときは，`../images/[image].{png,jpg}` で参照できる．なぜなら，dist/{html, css}/*.* に移動することで，../images/ に画像ファイルが存在するためだ．

### Component

React.js 等を使用したい場合は，使っても構わない
