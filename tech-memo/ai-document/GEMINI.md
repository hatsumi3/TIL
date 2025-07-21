# Gemini 作業再開用サマリー

## 1. プロジェクト概要

広告配信用メールマガジンの受信許諾（オプトイン/アウト）を管理するWebシステム。

*   **バックエンド**: Node.js, Hono, TypeScript
*   **データベース**: PostgreSQL
*   **テスト**: Vitest, hono/testing

## 2. 現在までの進捗

*   **機能実装**:
    *   `API-01: 許諾ステータス更新API` の基本ロジックを実装済み。
    *   上記APIの処理完了後、`nodemailer`とEtherealを利用したダミーの通知メール送信機能を実装済み。
*   **テスト**:
    *   `Vitest`と`hono/testing`によるバックエンドAPIのテスト環境を構築済み。
    *   `API-01`の正常系・異常系の基本的なテストケースを実装済み。

## 3. 次回作業予定

`9-todo.md`に基づき、以下のタスクから再開する。

*   **タスク**: `3. 管理者機能のセキュリティ強化`
*   **具体的な内容**: 管理者向けAPI (`/api/v1/admin/users`) に`@hono/basic-auth`を利用したBasic認証を導入する。

## 4. 再開時に確認すべき主要ファイル

*   **全体像の把握**:
    *   `1-sa.md` (要件定義書)
    *   `2-uc.md` (基本設計書)
    *   `3-ss.md` (詳細設計書)
    *   `9-todo.md` (TODOリスト)
*   **実装コードの確認**:
    *   `project/api/src/index.ts` (メインロジック)
    *   `project/api/src/index.test.ts` (テストコード)
    *   `project/api/package.json` (依存ライブラリ)

## 5. 開発環境の起動コマンド

```bash
# 1. データベースの起動 (初回のみ or 未起動の場合)
cd /Users/hatsumi3/TIL/tech-memo/ai-document/project
docker compose up -d

# 2. バックエンドAPIの起動
cd /Users/hatsumi3/TIL/tech-memo/ai-document/project/api
npm run dev
```
