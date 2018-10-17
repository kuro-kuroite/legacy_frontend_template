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
