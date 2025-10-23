## シーケンス図

```mermaid
sequenceDiagram
    participant User
    participant App as Application
    participant ICEWALL as IBM ICEWALL (RP)
    participant LINE as LINE Platform (OP)

    title LINEログイン認証フロー (ICEWALL経由)

    %% 1. ユーザーのアクションとICEWALLへのリダイレクト
    User->>App: 1. "LINEでログイン"ボタンクリック
    App->>ICEWALL: 2. 認証リクエスト (ICEWALLへリダイレクト)
    
    %% 2. LINEへのリダイレクトと認証・認可
    ICEWALL->>LINE: 3. 認可リクエスト (OAuth/OIDC)
    note right of ICEWALL: client_id, response_type=code, scope, redirect_uri(ICEWALL), state, nonce
    
    LINE->>User: 4. LINEログイン画面/同意画面表示
    User->>LINE: 5. 認証情報入力とアクセス承認
    
    %% 3. ICEWALLによるトークン取得と検証
    LINE->>ICEWALL: 6. 認可コード付与 (コールバック)
    note left of LINE: code, state(ICEWALL)を付与
    
    ICEWALL->>ICEWALL: 7. state検証 (CSRF対策)
    ICEWALL->>LINE: 8. トークンリクエスト (認可コードとclient_secretを使用)
    LINE-->>ICEWALL: 9. アクセストークン & IDトークン発行 (ID TokenはJWT)
    
    ICEWALL->>ICEWALL: 10. IDトークン検証 (署名, iss, aud, nonce, expなど)
    ICEWALL->>LINE: 11. ユーザー情報取得リクエスト (Userinfo Endpoint, Access Tokenを使用)
    LINE-->>ICEWALL: 12. ユーザー情報レスポンス
    
    %% 4. アプリケーションへのセッション確立
    ICEWALL->>ICEWALL: 13. ICEWALLセッション確立 (LINE情報に基づき)
    ICEWALL->>App: 14. 最終リダイレクト (認証済みセッション情報付与)
    App->>User: 15. アプリケーションへログイン完了
```


## 補足

このシーケンス図は、LINEログインで一般的に使用されるOAuth 2.0の認可コードフローとOpenID Connectの組み合わせに基づいています。

ICEWALL (RP): ICEWALLは**リライングパーティ（Relying Party）**として機能し、LINEが発行したIDトークンを検証し、アプリケーションへのアクセスを許可します。

LINE (OP): LINEは**OpenIDプロバイダ（OpenID Provider）**として機能し、ユーザー認証とトークン発行を行います。

バックチャネル通信: ステップ8～12は、ユーザーのブラウザを介さず、ICEWALLとLINE間で直接行われる安全なバックチャネル通信です。