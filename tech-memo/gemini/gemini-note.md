# gemini note

## 起動コマンド

```bash
# 対話モード
gemini
# 非対話モード
gemini -p "sssss"
```

## コマンドオプション

- `--all-files` (短縮形: -a)
  - カレントディレクトリ以下のすべてのファイルを再帰的に読み込み、コンテキストとしてプロンプトに含めます。
- `--sandbox` (短縮形: -s)
  - ツール（特にシェルコマンド）の実行を安全なサンドボックス環境（Docker）内で行います。
- `--yolo`
  - "You Only Live Once"モード。ツールの実行前に確認を求めず、すべて自動で承認します。サンドボックスが有効になっていることが推奨されます。
- `--checkpointing`
  - ツールの実行前にファイルの状態を保存（チェックポイントを作成）し、`/restore` コマンドで復元できるようにします。

## 特殊コマンド

- `/chat`
- `/memory`
- `/compress`
  - 現状の会話履歴の要約
  - トークンを節約できる
- `restore`
  - チェックポイントの復元
- `@<file_path|directory_path>`
  - ファイルなどの参照
  - `.gitignore`などは無視される
- `!`
  - shellモードとの切り替え
  - 別ターミナル起動が不要に

## リンク

- [Gemini CLI の簡単チュートリアル](https://zenn.dev/schroneko/articles/gemini-cli-tutorial)