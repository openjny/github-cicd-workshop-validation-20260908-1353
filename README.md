# Community Events - GitHub CI/CD スターター

Issue、ブランチ、Pull Request、CI、GitHub Pages CD を一巡するための小さな Vite アプリです。実在の組織、イベント、アカウントは使用していません。

## 前提条件

- Git
- Node.js 22（最低要件は Node.js 20.19）
- npm
- GitHub Actions と GitHub Pages を利用できる GitHub リポジトリ

## ローカルで確認する

```powershell
npm install
npm test
npm run build
npm run dev
```

開発サーバーを終了するには `Ctrl+C` を押します。`npm run build` の出力先は `dist/` です。

## Issue シナリオ

リポジトリ作成後、**Issues** から「小規模な変更」テンプレートを選択します。

> 残席が3席以下のイベントを分かりやすくする

完了条件はテンプレートに記載されています。主な変更対象は `src/availability.js` と `src/availability.test.js` です。まずテストを追加し、失敗を確認してから実装を変更します。

## ブランチと Pull Request の流れ

Issue 番号が `1` の例です。

```powershell
git switch -c develop
git push -u origin develop
git switch -c feature/1-low-availability
```

1. `feature/1-low-availability` でコードとテストを変更し、push します。
2. `feature/1-low-availability` から `develop` への Pull Request を作成します。
3. CI とレビューを確認し、Merge commit で `develop` へマージします。
4. `develop` から `main` への Pull Request を作成します。
5. CI とレビューを確認し、Merge commit で `main` へマージします。
6. `main` への push で GitHub Pages workflow が実行されます。

## GitHub Pages の設定

リポジトリの **Settings > Pages > Build and deployment > Source** で **GitHub Actions** を選択します。`vite.config.js` の `base: './'` により、ユーザーサイトと任意の名前のプロジェクトサイトのどちらでも静的アセットを相対 URL で参照できます。

Pages workflow は次の公式アクションを使用します。

- `actions/configure-pages@v5`
- `actions/upload-pages-artifact@v4`
- `actions/deploy-pages@v4`

Secrets は不要です。

## トラブルシュート

- `npm ci` が lock file の不一致で失敗する: ローカルでは `npm install` を実行して `package-lock.json` を更新し、その変更も commit します。
- Node.js の要件エラーが出る: `node --version` を確認し、Node.js 22 を使用します。
- Pages が `404` になる: Pages の Source が **GitHub Actions** であることと、`Deploy GitHub Pages` workflow の `deploy` job が成功していることを確認します。
- CI が動かない: Pull Request の base が `develop` または `main` であること、Actions がリポジトリで許可されていることを確認します。
- `develop` が Pull Request の選択肢にない: `git push -u origin develop` を先に実行します。
