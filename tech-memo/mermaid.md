# mermaid

とりあえずシーケンス図。

## 記載方法

- `sequenceDiagram` から始める
- `participant cock as コック` でライフラインに命名できる
- `Note over lifeline1, lifeline2: text` でコメントを入れる
- `alt else end`で条件分岐
- `opt end`で条件指定
- `loop end`でループ
- `rect rgba(255, 0, 255, 0.2) end` で背景色指定
- `%% コメントコメント` で背景色指定

### 矢印

- `->>` 矢印
- `-->>` 点線矢印

### 実行仕様

- `->>+` 開始
- `->>-` 終了

## 例

### no.1

```mermaid
sequenceDiagram
    participant cock as コック
    participant flypan as フライパン
    cock ->>+ flypan:cocking
    Note over flypan: 8 minutes later
    Note over cock, flypan: synchronize
    flypan -->>- cock: burn

```

### no.2

```mermaid
sequenceDiagram
    participant cook as シェフ
    participant kitchenware1 as 鍋1
    participant kitchenware2 as 鍋2

    alt ビーフカレー
        cook ->> kitchenware1:牛肉を入れる
    else チキンカレー
        cook ->> kitchenware2:鶏肉を入れる
    end

    opt 辛いもの好き
        cook ->> kitchenware1: チリペッパーを入れる
        cook ->> kitchenware2: チリペッパーを入れる
    end

    %% コメントコメント
    rect rgba(255, 0, 255, 0.2)
        loop ときどき
            cook ->> kitchenware1: 混ぜる
        end
    end
```

### no.3

```mermaid

graph TD
    A[Enter Chart Definition] --> B(Preview)
    B --> C{decide}
    C --> D[Keep]
    C --> E[Edit Definition]
    E --> B
    D --> F[Save Image and Code]
    F --> B

```

## link

- [qiita 記事](https://qiita.com/konitech913/items/90f91687cfe7ece50020)
- [公式](https://mermaid.js.org/intro/getting-started.html)