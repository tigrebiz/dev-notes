# 初めに

以下に「**Node.js + npm + Yarn + npx + create-react-app**」まわりの基本的な概念やコマンドを整理したまとめを作成します。次回の復習や、新たに学習を進めるうえで活用していただければと思います。

---

# 全体像

## 1. 用語・役割の整理

| 用語                 | 役割 / 概念                                                                                                                 |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Node.js**          | JavaScript をサーバーサイドで動かせるようにする「ランタイム」。<br>インストールすると「npm」も付属してくる。                |
| **npm**              | Node.js に標準で付属する「パッケージマネージャ」。<br>依存ライブラリのインストールやスクリプト実行を担当。                  |
| **npx**              | npm v5.2.0 以降で同梱される「CLI 実行補助ツール」。<br>パッケージを一時的にダウンロード＆実行できる。                       |
| **Yarn**             | Facebook（Meta）などが開発した**npm 代替のパッケージマネージャ**。<br>npm と同じようにインストールやスクリプト実行が可能。  |
| **create-react-app** | React プロジェクトの**雛形（ひな型）を作る CLI ツール**。<br>Webpack・Babel などの設定を省略してすぐに React を始められる。 |

### まとめ図（概念的なイメージ）

```ascii
+----------------------+                 +--------------------------+
|   Node.js (Runtime)  |                 |    npmjs.org (Registry)  |
|  - V8エンジン etc.    |                 |  (パッケージが集まる倉庫) |
+----------+-----------+                 +------------+-------------+
           |                                      ^
           | インストールすると                     | npm / Yarn はここから
           | npm & npx が付属                     | パッケージを取得
           v                                      |
  +------------------+    Yarn は別途導入    +------------------+
  |   npm & npx     | <-------------------- |    Yarn (v1)     |
  | (パッケージ      |                       | (パッケージ       |
  |  マネージャ /    |                       |  マネージャ)      |
  |  CLI 実行補助)   |                       +------------------+
  +---------+--------+
            | 例: npx create-react-app
            |     npm install react
            |
            v
  +----------------------------+
  |  create-react-app (CLI)   |
  |  - Reactアプリの雛形生成   |
  +----------------------------+
            |
            v
  +-----------------------------------+
  |   React + TypeScript プロジェクト |
  |   (src/, package.json, etc...)    |
  +-----------------------------------+
```

---

## 2. Node.js と npm / npx

1. **Node.js** を PC にインストールすると、コマンドラインに **`node`** と **`npm`** と **`npx`** が使えるようになります。
2. **npm** は依存ライブラリのインストールなどを行うコマンド。
3. **npx** は「**一時的に（ローカルやグローバルにインストールしていない）CLI ツールを、その場で実行**」するためのコマンドです。
   - `npx create-react-app myapp` → create-react-app をグローバルインストールしなくても、すぐに「myapp」という React プロジェクトが作れる。

---

## 3. Yarn

- **Yarn** は npm と同様に、パッケージのインストールやスクリプトの実行ができる別のパッケージマネージャ。
- インストール方法:
  ```bash
  # npm経由でグローバルインストールする例
  npm install --global yarn
  ```
- インストール後は、`yarn add <package>`, `yarn remove <package>`, `yarn start` など、npm とほぼ同じ操作感で利用できます。
- **`yarn create`** は Yarn が提供するサブコマンドで、以下のように一時的に CLI ツールを取得＆実行できます。
  ```bash
  yarn create react-app myapp --template typescript
  ```

---

## 4. create-react-app

- **create-react-app (CRA)** は 「**React プロジェクトを手早く開始するためのツール**」。
- npm レジストリ（npmjs.org）にパッケージとして公開されています。
- 下記いずれかの方法で使える。結果的に同じ雛形ができあがります。

| 実行コマンド                                                 | 説明                                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `npx create-react-app myapp`                                 | 一時的に `create-react-app` をダウンロードして実行する（npm の仕組み）。 |
| `npm install -g create-react-app` → `create-react-app myapp` | グローバルインストールしてからコマンドを実行。                           |
| `yarn create react-app myapp`                                | Yarn の `create` サブコマンド経由で実行。                                |
| `pnpm dlx create-react-app myapp`                            | **pnpm** を使って同様に実行する方法。                                    |

実行後は `myapp/` ディレクトリに以下のような構成が自動生成され、すぐに React アプリを起動可能になります。

```
myapp/
├─ src/
│  ├─ App.tsx      # Reactコンポーネント例
│  └─ index.tsx    # エントリポイント
├─ public/
│  └─ index.html
├─ package.json
├─ tsconfig.json   # TypeScript設定
└─ ... (その他ファイル) ...
```

---

## 5. npm や Yarn の主要コマンド一覧

### npm のよく使うコマンド

| コマンド                | エイリアス      | 説明                                                                                  |
| ----------------------- | --------------- | ------------------------------------------------------------------------------------- |
| **npm init**            | -               | `package.json` を作る（プロジェクトの初期化）。                                       |
| **npm install <pkg>**   | **npm i <pkg>** | ローカルにパッケージをインストール。                                                  |
| **npm run <script>**    | -               | `package.json` の `scripts` で定義したスクリプトを実行。                              |
| **npm start**           | -               | `package.json` の `"start"` スクリプトを実行。                                        |
| **npm test**            | -               | `package.json` の `"test"` スクリプトを実行。                                         |
| **npm uninstall <pkg>** | **npm remove**  | パッケージをアンインストール。                                                        |
| **npm update <pkg>**    | -               | 既存パッケージを更新。                                                                |
| **npx <コマンド>**      | -               | (npm 付属) インストールせずにパッケージを取得＆実行。例: `npx create-react-app myapp` |

### Yarn (Classic v1) のよく使うコマンド

| コマンド                 | 説明                                                                                                  |
| ------------------------ | ----------------------------------------------------------------------------------------------------- |
| **yarn init**            | 新規プロジェクトの初期化。 `package.json` を生成。                                                    |
| **yarn install**         | `package.json` / `yarn.lock` に従って依存パッケージをインストール。                                   |
| **yarn add <pkg>**       | パッケージをローカルに追加し、`package.json`・`yarn.lock` に記入。                                    |
| **yarn remove <pkg>**    | パッケージを削除し、`package.json`・`yarn.lock` を更新。                                              |
| **yarn run <script>**    | `package.json` の `scripts` で定義されたスクリプトを実行。                                            |
| **yarn start**           | `scripts.start` を実行。                                                                              |
| **yarn test**            | `scripts.test` を実行。                                                                               |
| **yarn create <ツール>** | 一時的にツールをインストール＆実行し、プロジェクト雛形などをセットアップ。例: `yarn create react-app` |

---

## 6. まとめ・ポイント

1. **Node.js をインストール**すると、標準で `node`, `npm`, `npx` がグローバルコマンドとして使えるようになる。
2. **npm** は Node.js 標準の**パッケージマネージャ**、**npx** は「一時的に CLI ツールを実行する補助コマンド」。
3. **Yarn** は npm に代わる**別のパッケージマネージャ**（別途インストールが必要）。
   - Yarn の `create` サブコマンドも、npx と同じように「CLI ツールをその場で取得＆実行」できる。
4. **create-react-app** は React アプリの雛形を自動生成する**CLI パッケージ**。
   - npm / Yarn / pnpm など、どれを経由しても実行結果はほぼ同じ。
5. それぞれのコマンドを**混在**させると依存関係のトラブルが起きる場合もあるので、**プロジェクト内では npm か Yarn か、どちらかに統一**するのが無難。

---

# 参考リンク

- [Node.js 公式サイト](https://nodejs.org/)
- [npm Docs (Commands)](https://docs.npmjs.com/cli/v9/commands)
- [npx Docs](https://docs.npmjs.com/cli/v9/commands/npx)
- [Yarn (Classic) Docs](https://classic.yarnpkg.com/en/docs/cli/)
- [Create React App GitHub](https://github.com/facebook/create-react-app)

以上が、これまでのやりとりをベースにしたまとめとなります。図や表を使いながら整理しましたので、次回の復習や疑問点の解消にお役立てください。
