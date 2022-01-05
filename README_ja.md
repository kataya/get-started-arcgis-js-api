<!--# Tutorial - Getting started with web 3D using ArcGIS API for JavaScript-->
# チュートリアル - ArcGIS API for JavaScript を利用した 3D Web マッピングアプリケーション入門

<!--In this tutorial you'll learn how to create a globe visualization of places you've been to. Something like this pin globe (the style is all up to you, you can get creative and style it totally different):-->
このチュートリアルでは、訪問した事がある場所を地球儀にピンで表示する方法を学んでもらいます（地図のスタイルはあなた次第で、より創造的なスタイルにすることも可能です）。

※ このドキュメントは、[Tutorial - Getting started with web 3D using ArcGIS API for JavaScript](https://github.com/RalucaNicola/get-started-arcgis-js-api) を翻訳したもので、TypeScript の linter でtslint を利用しているなど、一部、現在は非推奨の古い情報が含まれます。

[![screenshot](./images/screenshot.png "Click to see app in action")](https://ralucanicola.github.io/get-started-arcgis-js-api/)

---

<!--If you want to skip the tutorial and create your own globe just by replacing the data, do this:-->
もしあなたがこのチュートリアルをスキップしてあなた自身の地球儀を作成するには、データを置き換えし、次を行ってください。
<!--
- clone this repo: `git clone https://github.com/RalucaNicola/get-started-arcgis-js-api.git`
- go to the folder: `cd get-started-arcgis-js-api`
- remove any references to the original repository: `rm -rf .git`
- install dependencies: `npm install`
- start watching for changes by running `npm run start` in the console
- replace your data by changing the [locations.json](./data/locations.json) file with your own GeoJSON. You can create a GeoJSON of your locations with the [geojson.io web-app](http://geojson.io/).
- make any other changes you want to the app (tweak title, font, colors etc.)
- when you are done, stop the server with Ctrl/Cmd-C
- ready to deploy the app? now you can copy it on your web-server or you can deploy it on GitHub:
  - run `git init` to initialize the repository
  - add all the files to the repo: `git add .`
  - make your first commit: `git commit -m "initial commit"`
  - create a repository on GitHub and connect it to the local repo: `git remote add origin https://github.com/your-username/your-app-name.git`; `git push -u origin master`
  - go to the Settings tab in your repository, scroll down to GitHub Pages, choose the master branch as a source and hit Save.
-->

- このリポジトリをクローン: `git clone https://github.com/RalucaNicola/get-started-arcgis-js-api.git`
- フォルダーを移動: `cd get-started-arcgis-js-api`
- オリジナルのリポジトリへの参照を削除: `rm -rf .git`
- 依存関係をインストール: `npm install`
- コンソールで `npm run start` を実行し、変更の監視を開始します。
- [locations.json](./data/locations.json) ファイルを自分のGeoJSON ファイルに置き換えすることも可能です。[geojson.io web-app](http://geojson.io/) を使って、自分でGeoJSON を作成することも可能です。
- アプリに変更を加えることも可能です(タイトル、フォント、色 など)。
- 終了したら、Ctrl + C でサーバーを停止します。
- アプリをデプロイする準備ができたら、web サーバーにコピーするか、GitHub にデプロイします。
  - リポジトリを初期化するため `git init` を実行します。
  - `git add .` ですべてのファイルをリポジトリに追加します
  - `git commit -m "initial commit"` で最初のコミットを行います
  - GitHub にリポジトリを作成し、ローカルリポジトリにそれを接続します。 `git remote add origin https://github.com/your-username/your-app-name.git`; `git push -u origin master`
  - リポジトリの Settings タブで GitHub Pages にスクロールし、master ブランチをソースとして選択して、Save をクリックします。


---

<!--For those who want to follow the detailed tutorial on how to create this from scratch, you'll need to have the data in [geojson](http://geojson.org/) format and like all things web, make sure to have `node` and `git` installed. It's useful if you have a [GitHub account](https://github.com/) so you can host the code there and deploy it in the end on [GitHub Pages](https://pages.github.com/). P.S. This is a nice web-app, if you want to create geojson data: http://geojson.io/.-->
この詳細なチュートリアルに従って、スクラッチでこのアプリケーションを作成方法を知りたい人は、[geojson](http://geojson.org/) フォーマットのデータが必要となります。また、`node` と `git` がインストールされているか確認してください。[GitHub アカウント](https://github.com/) があれば、コードをホストしたり、最終的には[GitHub Pages](https://pages.github.com/) にデプロイすることができるので便利です。 
P.S. geojson データを作成する場合、このweb アプリケーション: http://geojson.io/ を使うとよいでしょう。

<!--## Step 1: Set up the project and development environment

### Set up your project-->

## ステップ 1: プロジェクト と開発環境 の設定

### プロジェクト の設定

<!--Create the folder for your project and inside it in the console run `npm init`. Fill up project name, keywords, author, etc.
This will generate a `package.json` file.-->
プロジェクト用のフォルダーを作成し、そのフォルダー下のコンソールで `npm init` コマンドを実行し、対話的にプロジェクト名 (project name), キーワード (keywords), 著者 (author), 等の情報を入力します。
これにより `package.json` ファイルが作成されます。

<!--### Set up a git repo-->
### git repo を設定
<!--
In the terminal run `git init` in the root folder. Create a `.gitignore` file and add

```
node_modules/*
.DS_Store
```

to exclude them from `git`.
-->

`git` から除外するために、`git init` を実行したルートフォルダーのターミナル内で  `.gitignore` ファイルを作成し、そのファイルに次のように除外する対象を記載します。

```
node_modules/*
.DS_Store
```


<!--### Set up app structure-->
### アプリケーションの構成を設定
<!--
Create an `index.html` file and an `app` folder that will store the JS/TS code and a style folder that will store the CSS.

- index.html
- app -> main.js
- style -> main.css
-->
`index.html` ファイル、JS/TS コードを格納する`app` フォルダーと main.js ファイル、CSS を格納する`style` フォルダーと main.css ファイル を作成します。

- index.html
- app -> main.js
- style -> main.css
  
<!--### Set up TypeScript-->
### TypeScript の環境を設定

次のコマンドを実行します。
```
npm install --save-dev typescript
npm install --save @types/arcgis-js-api
```
<!--
Add `tsconfig.json` file with the following configuration:
-->
そうすると、`tsconfig.json` ファイルに次の設定情報が追加されます:

```js
{
  "compilerOptions": {
    "module": "amd",
    "noImplicitAny": true,
    "sourceMap": true,
    "jsx": "react",
    "jsxFactory": "tsx",
    "target": "es5",
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "preserveConstEnums": true,
    "suppressImplicitAnyIndexErrors": true
  },
  "include": [
    "./app/*"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

<!--
Add a script in `package.json` to watch for changes and compile everytime a `.ts` file changes:
-->
すべての `.ts` ファイルの変更を監視してコンパイルするために、 `package.json` ファイルに次のスクリプトを追加します:

```js
  ...,
  "scripts": {
    "tsc": "tsc -w" // compiles and watches for changes
  },
```
<!--
You can find a detailed description of this step [here](https://developers.arcgis.com/javascript/latest/guide/typescript-setup/index.html).

To test this, change `main.js` file to `main.ts` and run `npm run tsc`. This will compile the files and watch for changes.
-->
上記で述べた詳細な手順は、 [ここ](https://developers.arcgis.com/javascript/latest/guide/typescript-setup/index.html) に記載されています（AMD CDN モジュールを使う場合）。

これをテストするために、`main.js` ファイルを `main.ts` に変更し、`npm run tsc` のコマンドを実行します。これは、ファイルをコンパイルし、変更を監視するものです。

<!--### Set up a linter for TypeScript-->
### TypeScript の linter を設定
<!--
Install the tool needed for linting:

`npm install --save-dev tslint`

You can read more about the tool [here](https://palantir.github.io/tslint/).
Set up the linting rules in a `tslint.json` file. The rules that I use in this project are the ones that we also use when developing the API:
https://github.com/Esri/jsapi-resources/blob/master/4.x/typescript/demo/tslint.json.

Copy the `tslint.json` file in the root of the project and create a script to run the command from the command lint:
-->
リンティング に必要なツールのインストール:
<!--tslint はすでに非推奨になったいるのでeslint に書き換えした方がよい
Qiita:
https://qiita.com/suzuki_sh/items/fe9b60c4f9e1dbc5d903
Add npm to existing ArcGIS JS API web app project:
https://urbandatapalette.com/post/2021-06-arcgis-js-api-env-setup/
これは？:
https://www.unpkg.com/browse/arcgis-js-api@4.15.2/.eslintrc.json
tslint.jsonをeslintrcに変換してくれるツールの例：
https://qiita.com/shisama/items/b3d63142797b07cb554e
-->

`npm install --save-dev tslint`

ツールの詳細は、[こちら](https://palantir.github.io/tslint/) を参照してください。
`tslint.json` ファイルのリンティング ルールを設定します。このプロジェクトでのルールは、API を開発する際にも使っているルールです。
https://github.com/Esri/jsapi-resources/blob/master/4.x/typescript/demo/tslint.json.

プロジェクトのルートに `tslint.json` ファイルをコピーし、lint コマンドを実行するスクリプトを記載します。:

```js
  "scripts": {
    ...,
    "lint": "tslint app/**/*.ts"
  },
```
<!--
You can run this command if you want to check for syntax errors in your code.
-->
このコマンドは、コードに構文エラーがないか確認する場合に実行します。

<!--### Set up a local web server-->
### ローカル Web サーバー のセットアップ
<!--
Install `browser-sync` to set up a web server. This will also listen for changes and reload the page:

`npm install --save-dev browser-sync`

Install `npm-run-all` to have the web server watch for changes in parallel to the typescript compiler.

`npm install --save-dev npm-run-all`

Change the `scripts` property to the following:
-->
Web サーバー をセットアップするために、`browser-sync` をインストールします。これはまた変更をリッスンし、ページを再読み込みします。

`npm install --save-dev browser-sync`

`npm-run-all` をインストールすると、typescript のコンパイラと並行して、Web サーバー が変更を監視するようになります。

`npm install --save-dev npm-run-all`

`scripts` プロパティ を次のように変更します。:

```js
"scripts": {
  "watch:build": "tsc -w",
  "watch:server": "browser-sync start --server -w",
  "lint": "tslint app/**/*.ts",
  "start": "npm-run-all --parallel watch:build watch:server"
}
```

<!--### Add repository to GitHub (optional step)-->
### GitHub にリポジトリを追加 (オプションのステップ)

<!--
This is optional, but it's useful to have a backup of your code and also to deploy the app at the end using GitHub Pages.

Go to [GitHub](https://github.com/) and create an account if you don't have one yet.
Create a new repository (don't create a Readme file) and then push the local repository to the remote one:
-->
このステップはオプションですが、コードのバックアップや、GitHub Pages へ最終的にデプロイする場合に便利です。

[GitHub](https://github.com/) にアクセスします。まだアカウントを持っていない場合、作成してください。
新しいリポジトリ を作成し(Readme ファイルは作成しないでください)、ローカル リポジトリをリモート リポジトリにプッシュします。

```
git remote add origin https://github.com/your-username/your-app-name.git
git push -u origin master
```

<!--## Step 2: Create a globe-->
## ステップ 2: 地球儀 の作成
<!--
In this step we'll add a 3D map to our project. So let's create a `Map` and render it in a `SceneView`:
-->
このステップでは、3D マップをプロジェクトに追加します。 `Map` を作成し、`SceneView` で描画してみましょう。:

```ts
import Map from "esri/Map";
import SceneView from "esri/views/SceneView";

const map = new Map({
  basemap: "topo"
});

const view = new SceneView({
  map,
  container: "viewDiv"
});
```

<!--To see the div element that contains the map, we should change the `height` to `100%`:-->
map を含む div 要素を表示するには、main.css ファイルの`height` を `100%` に変更する必要があります。

```css
html,
body,
#viewDiv {
  margin: 0;
  padding: 0;
  height: 100%;
}
```

<!--## Step 3: Add location points as GeoJSON-->
## ステップ 3: GeoJSON として位置情報を追加
<!--
We can add data in GeoJSON format as a [GeoJSONLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-GeoJSONLayer.html).

First, create a `data` folder and add the file with the locations there. I added [locations.json](./data/locations.json).

Each point in the file has `properties` and `geometry`. For example, this is the point corresponding to "Taipei":
-->
[GeoJSONLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-GeoJSONLayer.html) として、GeoJSON フォーマットのデータを追加することが可能です。.

最初に、`data` フォルダーを作成し、そこに位置情報を持ったファイルを追加します。このサンプルの場合は、[locations.json](./data/locations.json) を追加してあります。.

ファイル内の各ポイントは、`properties` と `geometry` を持っています。 例えば、これは "Taipei" に対応するポイントのサンプルです。:

```js
{
  "properties": {
    "country": "Taiwan",
    "location": "Taipei"
  },
  "geometry": {
    "coordinates": [
      121.3,
      25.03
    ],
    "type": "Point"
  }
}
```
<!--
Adding the layer then is pretty straight-forward:
-->
レイヤーを追加するのは、とても簡単です。:

```js
const locationsLayer = new GeoJSONLayer({
  url: dataUrl,
  renderer,
  labelingInfo
});

map.add(locationsLayer);
```
<!--
The layer has a UniqueValueRenderer to display each point with a different color. The colors are randomly assigned based on the ObjectID of the features.-->
レイヤーはそれぞれの点を異なる色で表示するために、個別値分類レンダラー（UniqueValueRenderer）を保持しています。色はフィーチャーの ObjectID をもとに、ランダムに割り当てされます。

```js
const colorPalette = [
  "#FFFFFF",
  "#F78D99",
  "#ffc700",
  "#27004D",
  "#005892",
  "#E5211D",
  "#198E2B",
  "#f4a142",
  "#fc8879",
  "#fcec78",
  "#8de8b1",
  "#8dcfe8",
  "#8d8ee8",
  "#c88de8"
];

const uniqueValueInfos = colorPalette.map((color: string, index: number) => {
  return {
    value: index,
    symbol: new PointSymbol3D({
      symbolLayers: [
        new ObjectSymbol3DLayer({
          material: {
            color: color
          },
          height: 150000,
          width: 150000,
          resource: { primitive: "sphere" }
        })
      ],
      verticalOffset: {
        screenLength: 40,
        maxWorldLength: 10000000,
        minWorldLength: 10000
      },
      callout: {
        type: "line", // autocasts as new LineCallout3D()
        size: 1.5,
        color: "#555"
      }
    })
  };
});

const renderer = new UniqueValueRenderer({
  valueExpression: `$feature.ObjectID % ${colorPalette.length}`,
  uniqueValueInfos: uniqueValueInfos
});
```

<!--## Step 4 - some more styling-->
## ステップ 4 - いくつかのスタイルを追加で設定
<!--
In this step we're going to change the basemap and make the background color transparent. In the end we will also add a title and labels for our locations.
-->
このステップではベース マップを変更し、背景色を透明にすることを行います。最後には、位置情報にタイトルとラベルを追加します。

<!--
### Add a vector tile layer basemap
-->
### ベクター タイル レイヤー を ベースマップ として追加
<!--
Many vector tile layer have unique styles and are really creative. Have a look at this [group](https://www.arcgis.com/home/group.html?id=30de8da907d240a0bccd5ad3ff25ef4a) for inspiration. I like [this layer](http://www.arcgis.com/home/item.html?id=1c365daf37a744fbad748b67aa69dac8), it reminds me of a school atlas and I think those pins would fit well on this basemap.
When I add a vector tile layer to a 3D map, most of the times I remove the labels because draped labels don't really make much sense. So what I do, is [download the style](https://www.arcgis.com/sharing/rest/content/items/1c365daf37a744fbad748b67aa69dac8/resources/styles/root.json?f=pjson) of the vector tile layer and then I modify and load it with the API. You can see the modified version [here](./data/basemap-style.json).

This is the code that I use to create a basemap from a vector tile layer with a style hosted locally:
-->
多くのベクター タイル レイヤーは、ユニークなスタイルを持っており、とても創造的です。この [グループ](https://www.arcgis.com/home/group.html?id=30de8da907d240a0bccd5ad3ff25ef4a) を見て、インスピレーションを得てください。私は、[このレイヤー](http://www.arcgis.com/home/item.html?id=1c365daf37a744fbad748b67aa69dac8) が好きです。学校の地図帳を思い起こさせますし、これらのピンはこのベース マップによく合っていると思います。
3D マップ にベクター タイル レイヤーを追加するとき、ほとんどの場合、ラベルを削除します。 そこで私がしたことは、このベクター タイル レイヤーの[スタイルをダウンロード](https://www.arcgis.com/sharing/rest/content/items/1c365daf37a744fbad748b67aa69dac8/resources/styles/root.json?f=pjson) し、それを修正してAPIでロードすることをしています。修正したバージョンは、[ここ](./data/basemap-style.json) で見ることができます。

これは、ローカルにホストしたスタイルで、ベクター タイル レイヤー からベースマップを作成するのに、私が使用しているコードです。:

```ts
// change the map declaration to have a white ground color and no basemap
const map = new Map({
  ground: {
    surfaceColor: "#fff"
  }
});

// define vector tile layer and load the style
function getBaseLayer() {
  const baseLayer = new VectorTileLayer({
    url: "https://basemaps.arcgis.com/arcgis/rest/services/World_Basemap_v2/VectorTileServer"
  });

  return esriRequest("./data/basemap-style.json").then(result => {
    baseLayer.loadStyle(result.data);
    return baseLayer;
  });
}

// once the style is loaded, set the VTL as the base layer
getBaseLayer().then(baseLayer => {
  map.basemap = new Basemap({
    baseLayers: [baseLayer]
  });
});
```

<!--### Change background color-->
### 背景色の変更
<!--
Ok, let's get rid of the realistic dark space behind our globe so that we can see the web page background. This all happens in the [environment](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#environment) of the view.

First we need to enable transparency on the view by setting [alphaCompositingEnabled](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#alphaCompositingEnabled) to `true`.

Next we want to set the background of the view to be transparent. As the view background is behind the stars and the atmosphere we also need to remove them:
-->
では、Web ページの背景が見えるように、地球儀の背後にあるリアルな暗黒空間を取り除きましょう。それは view の [environment](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#environment) で設定します。

最初に、[alphaCompositingEnabled](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-SceneView.html#alphaCompositingEnabled) を `true` に設定して、 view の透明性を有効化します。

次に、view の背景を透明に設定します。また、view の背景の背後には星や大気があるので、それらを取り除く必要があります。

```js
environment: {
  background: {
    type: "color",
    color: [0, 0, 0, 0]
  },
  starsEnabled: false,
  atmosphereEnabled: false
}
```
<!--
To remove the navigation widgets I usually just remove everything in the `view.ui` in the top left corner like this:
-->
ナビゲーション ウィジェット（navigation widgets）を削除するには、通常、次のように `view.ui` の左上隅にあるものをすべて削除するだけです。

```js
view.ui.empty("top-left");
```

<!--### Some final changes: Add labels and a title-->
### いくつかの最終的な変更: ラベルとタイトルの追加
<!--
I picked a font for the map from [Google Fonts](https://fonts.google.com/) and now I'll add the labels and a title.

For the labels, all you need to do is set the labelingInfo on the FeatureLayer:
-->
マップ用のフォントを [Google Fonts](https://fonts.google.com/) から選び、ラベルとタイトルを追加します。

ラベルについては、FeatureLayer に labelingInfo を設定するだけです。:

```ts
const labelingInfo = [
  new LabelClass({
    labelExpressionInfo: { expression: "$feature.location" },
    symbol: new LabelSymbol3D({
      symbolLayers: [
        new TextSymbol3DLayer({
          material: { color: "#333" },
          size: 10,
          font: {
            family: "Permanent Marker"
          },
          halo: {
            color: "white",
            size: 1
          }
        })
      ]
    })
  })
];
```

<!--## Step 5 - Deploy your website to GitHub-->
## ステップ 5 - GitHub に website をデプロイする
<!--
[Here](https://pages.github.com/) is a step by step tutorial on how to do this. Basically you need to go to the Settings tab in your repository, scroll down to GitHub Pages, choose the master branch as a source and hit Save.
You can check my project at [https://ralucanicola.github.io/get-started-arcgis-js-api/](https://ralucanicola.github.io/get-started-arcgis-js-api/.).
-->
[ここに](https://pages.github.com/) どのように行うかの、ステップ バイ ステップのチュートリアルがあります。基本的には、あなたのリポジトリの Settings タブで GitHub Pages までスクロールし、master ブランチをソースとして選択し Save を押す必要があります。
Raluca Nicola さんのプロジェクトは [https://ralucanicola.github.io/get-started-arcgis-js-api/](https://ralucanicola.github.io/get-started-arcgis-js-api/.) で確認できます。
