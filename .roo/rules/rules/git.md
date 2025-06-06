# Git ワークフローベストプラクティス

このドキュメントでは、効果的な Git ワークフローのためのベストプラクティスを説明します。

## ブランチ戦略

### メインブランチの管理

- `main`/`master`ブランチは常に安定した状態を維持する
- 直接コミットは避け、必ずプルリクエスト経由でマージする
- CI/CD テストに合格したコードのみをマージ許可する

### ブランチの命名規則

- `feature/<機能名>` - 新機能開発用
- `bugfix/<バグID>` - バグ修正用
- `hotfix/<問題名>` - 緊急の修正用
- `release/<バージョン>` - リリース準備用
- ブランチ名は具体的かつ簡潔に作成する

### ブランチワークフロー選択

- 小規模チーム: GitHub Flow（シンプルで効率的）
- 大規模チーム: Gitflow（より構造化された管理）
- 選択したワークフローはチーム全体で統一する

## コミットの作成

### コミットの準備段階

1. **変更の確認**

   ```bash
   # 未追跡ファイルと変更の確認
   git status

   # 変更内容の詳細確認
   git diff
   ```

2. **変更の分析**
   - 変更または追加されたファイルの特定
   - 変更の性質（新機能、バグ修正、リファクタリングなど）の把握
   - 不要なファイル（ログ、一時ファイル）が含まれていないか確認
   - 機密情報の有無確認

### コミットの原則

1. **原子的なコミット**

   - 一つのコミットには一つの論理的変更/機能のみを含める
   - レビューしやすく、問題発生時にロールバックしやすくする

2. **定期的なコミット**

   - 長時間作業せずに小さな単位で頻繁にコミット
   - 変更履歴の追跡がしやすくなる

3. **コミットの実行**

   ```bash
   # 関連ファイルのみをステージング
   git add <files>

   # コミットの実行
   git commit -m "✨ feat: ユーザー認証機能を追加"
   ```

### コミットメッセージの書き方

1. **形式と構造**

   - 形式: `<絵文字> <種類>: <簡潔な説明>`
   - 種類の例: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
   - 必要に応じて本文で詳細説明

2. **絵文字の活用**

   - 絵文字を使用して視覚的に変更の種類を識別しやすくする
   - チーム内で絵文字の意味を統一して使用する
   - 過度な使用は避け、一貫性を保つ

3. **メッセージの内容**

   - **「なぜ」に焦点を当てる**
   - 明確で簡潔な言葉を使用
   - 変更の目的を正確に反映
   - 一般的な表現を避ける

4. **コミットメッセージの例**

   ```plaintext
   # 新機能の追加
   ✨ feat: Result型によるエラー処理の導入

   # 既存機能の改善
   🚀 update: キャッシュ機能のパフォーマンス改善

   # バグ修正
   🐛 fix: 認証トークンの期限切れ処理を修正

   # リファクタリング
   ♻️ refactor: Adapterパターンを使用して外部依存を抽象化

   # テスト追加
   ✅ test: Result型のエラーケースのテストを追加

   # ドキュメント更新
   📝 docs: エラー処理のベストプラクティスを追加

   # セキュリティ関連の修正
   🔒 security: SQLインジェクション脆弱性を修正

   # パフォーマンス改善
   ⚡ perf: データ取得処理の最適化

   # UI/デザイン関連
   💄 ui: ボタンスタイルの統一
   ```

## プルリクエストの作成

### プルリクエストの準備段階

1. **ブランチの状態確認**

   ```bash
   # 未コミットの変更確認
   git status

   # mainからの差分確認
   git diff main...HEAD

   # コミット履歴の確認
   git log
   ```

2. **変更の分析**
   - main から分岐後のすべてのコミットの確認
   - 変更の性質と目的の把握
   - プロジェクトへの影響評価

### プルリクエストの原則

1. **適切なサイズ**

   - 大きすぎる PR は避ける（レビューが困難になる）
   - 機能単位で分割可能な場合は分割する

2. **完全性の確保**

   - テストが含まれているか
   - ドキュメントが更新されているか
   - CI/CD パイプラインが通過するか

3. **プルリクエストの作成**

   ```bash
   # GitHub CLIを使用する場合
   gh pr create --title "✨ feat: カスタムフックとクエリキャッシュの実装" --body "..."
   ```

### プルリクエストの説明文

1. **構造と内容**

   - 概要: 変更の目的と背景
   - 変更内容: 具体的な変更点のリスト
   - 技術的な詳細: 実装の詳細説明（必要に応じて）
   - レビューのポイント: レビュアーに着目してほしい点

2. **プルリクエストの例**

   ```markdown
   ## 概要

   React Query を使用したデータフェッチングの効率化と、アプリケーション全体でのエラーハンドリングの一貫性を高めるために、カスタムフックとクエリキャッシュを実装しました。

   ## 変更内容

   - `useApiQuery` カスタムフックの実装
     - タイプセーフなクエリキー生成
     - エラーハンドリングの統一
     - ローディング状態の管理
   - React Query の QueryClientProvider の設定
   - キャッシュ無効化戦略の実装
   - ユニットテストの追加

   ## 技術的な詳細

   - React Query v4 を使用したデータフェッチング
   - zod によるランタイム型検証の統合
   - API レスポンスの結果を Result 型で包む設計
   - テスト環境での MSW によるモック実装

   ## レビューのポイント

   - カスタムフックのインターフェース設計
   - キャッシュ戦略の適切さ
   - エラー処理の一貫性
   - ジェネリック型の制約

   ## テスト結果

   - ユニットテスト: ✅ 全 15 テスト通過
   - E2E テスト: ✅ 関連シナリオ通過
   - 型チェック: ✅ エラーなし
   ```

## コード品質の確保

1. **レビュー文化の構築**

   - コードレビューを義務化
   - 建設的なフィードバックの推奨
   - 知識共有の機会として活用

2. **自動化の活用**

   - CI/CD パイプラインの設定
   - 自動テスト、リント、静的解析の実行
   - 品質基準を満たさないコードのマージを防止

3. **継続的な改善**
   - 定期的なコードベースの見直し
   - 技術的負債の計画的な返済
   - プロジェクト固有のガイドラインの更新

## トラブルシューティングとベストプラクティス

1. **マージ競合の防止と解決**

   - 作業開始前に最新の変更を取得する
   - 長期間にわたるブランチの使用を避ける
   - 競合発生時は関係者と協力して解決

2. **リポジトリの健全性維持**

   - 大きなバイナリファイルの管理には Git LFS を検討
   - 不要なブランチの定期的な削除
   - 適切な `.gitignore` ファイルの管理

3. **避けるべき操作**

   - パブリックな履歴の書き換え（force push の乱用）
   - 大量のファイル変更を含む単一コミット
   - リモートリポジトリへの直接プッシュ
   - 機密情報のコミット

4. **効率化のヒント**
   - エイリアスの活用
   - GUI ツールの適切な使用
   - チーム内での知識共有

## 絵文字ガイド

コミットメッセージや PR タイトルに使用する絵文字の意味を統一するためのガイド：

| 絵文字 | 種類     | 説明                 |
| ------ | -------- | -------------------- |
| ✨     | feat     | 新機能               |
| 🐛     | fix      | バグ修正             |
| 📝     | docs     | ドキュメント更新     |
| 💄     | ui       | UI/UX/スタイルの変更 |
| ♻️     | refactor | リファクタリング     |
| 🚀     | update   | 機能改善             |
| ✅     | test     | テスト追加・修正     |
| 🔧     | chore    | 開発環境・ビルド関連 |
| ⚡     | perf     | パフォーマンス改善   |
| 🔒     | security | セキュリティ関連     |
| 🚧     | wip      | 作業中               |
| 🔥     | remove   | 機能・ファイルの削除 |
| 🔀     | merge    | マージ               |
