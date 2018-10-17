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
