# 広告メール許諾管理システム 詳細設計書

| 項目 | 内容 |
| :--- | :--- |
| ドキュメントバージョン | 1.0 |
| 作成日 | 2025/07/20 |
| 作成者 | Gemini |
| 関連ドキュメント | 1-sa.md, 2-uc.md |

-----

## 1. 概要

本ドキュメントは、基本設計書を元に、実際の開発・実装における技術的な詳細仕様を定義するものである。

-----

## 2. 技術スタック

本システムの構築に使用するライブラリ、フレームワーク、および実行環境は以下の通り。

| 分類 | 技術名 | バージョン/備考 |
| :--- | :--- | :--- |
| **バックエンド** | Node.js | - |
| | Hono | ^4.8.5 |
| | TypeScript | ^5.8.3 |
| | node-postgres (pg) | ^8.16.3 |
| **フロントエンド** | SvelteKit | ^2.22.0 |
| | Vite | ^7.0.4 |
| **データベース** | PostgreSQL | 15 (Docker Image) |
| **開発環境** | Docker Compose | - |
| **コード品質** | ESLint, Prettier | - |

-----

## 3. プロジェクト構成

```
/Users/hatsumi3/TIL/tech-memo/ai-document/
└── project/
    ├── .gitignore            # プロジェクト全体のGit無視設定
    ├── api/                  # バックエンド (Hono)
    │   ├── .gitignore        # バックエンドのGit無視設定
    │   ├── db/
    │   │   └── init.sql      # DB初期化SQL
    │   ├── src/
    │   │   └── index.ts      # APIエントリーポイント
    │   ├── node_modules/
    │   ├── package.json
    │   └── tsconfig.json
    ├── web/                  # フロントエンド (SvelteKit)
    │   ├── .gitignore        # フロントエンドのGit無視設定
    │   ├── src/
    │   ├── node_modules/
    │   ├── .eslintrc.cjs     # ESLint設定
    │   ├── .prettierrc       # Prettier設定
    │   └── package.json
    └── docker-compose.yml    # Docker Compose設定
```

-----

## 4. データベーススキーマ (詳細)

`project/api/db/init.sql` にて定義。基本設計に加え、パフォーマンスとデータ整合性のための項目を追加している。

```sql
CREATE TABLE IF NOT EXISTS users_opt_status (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    opt_status VARCHAR(10) NOT NULL CHECK (opt_status IN ('Opted-in', 'Opted-out')),
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- emailカラムにインデックスを作成し、検索パフォーマンスを向上させる
CREATE INDEX IF NOT EXISTS idx_email ON users_opt_status(email);

-- レコード更新時に updated_at を自動で更新するための関数
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

-- users_opt_status テーブルのUPDATE操作の前に上記関数を実行するトリガー
CREATE TRIGGER update_users_opt_status_updated_at
BEFORE UPDATE ON users_opt_status
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();
```

-----

## 5. API実装詳細

### 5.1. API-01: 許諾ステータス更新API

- **ファイル**: `project/api/src/index.ts`
- **エンドポイント**: `POST /api/v1/opt-status`
- **ロジック詳細**:
    1. リクエストボディから `email` と `action` をJSON形式で受け取る。
    2. `email` の存在、`action` が `'opt_in'` または `'opt_out'` のいずれかであるかを検証する。不正な場合はステータスコード `400` を返す。
    3. `pg` ライブラリの `Pool` を介してデータベース接続を取得する。
    4. `action` の値に応じて処理を分岐する:
        - **`opt_in` の場合**: `INSERT ... ON CONFLICT (email) DO UPDATE ...` (UPSERT) を実行し、レコードが存在しない場合は新規作成、存在する場合はステータスを `'Opted-in'` に更新する。
        - **`opt_out` の場合**: `UPDATE users_opt_status SET ... WHERE email = $1` を実行する。更新対象のレコードが存在しなかった場合 (`rowCount === 0`)、ステータスコード `404` を返す。
    5. データベース処理でエラーが発生した場合は、ステータスコード `500` を返す。
    6. 処理完了後、データベース接続をプールに返却する。
    7. 成功時は、処理内容に応じたメッセージをJSON形式で返す。

-----

## 6. 開発環境のセットアップと実行コマンド

### 6.1. データベースの起動

```bash
# プロジェクトルートで実行
cd project
docker compose up -d
```

### 6.2. データベースの初期化

```bash
# プロジェクトルートで実行
# SQLファイルをコンテナにコピー
docker compose cp api/db/init.sql db:/tmp/init.sql

# コンテナ内でSQLを実行
docker compose exec -T db psql -U user -d ad_opt_db -f /tmp/init.sql
```

### 6.3. バックエンドAPIの起動

```bash
cd project/api
npm install
npm run dev # http://localhost:3000 で起動
```

### 6.4. フロントエンドの起動

```bash
cd project/web
npm install
npm run dev # http://localhost:5173 で起動
```

### 6.5. コード整形と静的解析

```bash
# project/web ディレクトリで実行
npm run format # Prettierで整形
npm run lint   # ESLintでチェック
```
