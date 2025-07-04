# アクセストークンの取得方法

EpicGamesのAPIの認証には**OAuth2**を利用します。

手順1:認証を行うためにはまず、どのクライアントに対して認証するかを決めないといけません。クライアントリストは[こちら](./Clients.md)をご参照ください。

手順2:アクセストークンを取得するためには、ヘッダーに以下の要素を構築する必要があります。以下の文字列をBase64でエンコードしないといけません。`{clientId}:{clientSecret}`（それぞれEpicGamesDevポータルで取得したクライアントID/シークレットIDに置き換えてください）

手順3:次にクライアントとして認証するか、アカウントとして認証するかを決めないといけません。多くのEpicGamesのAPIではアカウント認証を必要とします。場合によっては、eg1（JWT）アクセストークンも必要になる可能性があることを覚えておいてください。アクセストークンはトークンリクエストにこのパラメータを含めることで取得できます。**パラメータ**:`token_type`


## クライアント認証

クライアントとして認証するためには、 [`client_credentials`](./GrantTypes/client_credentials.md) Grant Type を使用する必要があります。


以下はクライアントに対するHTTPリクエストの例になります。

```http
POST /account/api/oauth/token HTTP/1.1
Host: account-public-service-prod.ol.epicgames.com
Content-Type: application/x-www-form-urlencoded
Authorization: Basic ZWM2ODRiOGM2ODdmNDc5ZmFkZWEzY2IyYWQ4M2Y1YzY6ZTFmMzFjMjExZjI4NDEzMTg2MjYyZDM3YTEzZmM4NGQ=

grant_type=client_credentials
```

レスポンスはこちらです。

```json
{
  "access_token": "266e2719635f4a899e94bd19c4422a90",
  "expires_in": 14400,
  "expires_at": "2023-11-12T13:29:34.070Z",
  "token_type": "bearer",
  "client_id": "ec684b8c687f479fadea3cb2ad83f5c6",
  "internal_client": true,
  "client_service": "prod-fn",
  "product_id": "prod-fn",
  "application_id": "fghi4567FNFBKFz3E4TROb0bmPS8h1GW"
}
```

## アカウント認証

アカウントとして認証する場合は、認証方法（プレイステーションなどの外部認証、認証コードなど）を選択しないといけません。

Using the authorization code is usually the easiest:

手順1: `epicgames.com`にログイン

手順2:認証コードを作成するには [こちらのドキュメント](../../Web/Id/Auth/Redirect.md) を参照してください。

手順3:認証セッションのコード交換リクエストを送信する
```http
POST /account/api/oauth/token HTTP/1.1
Host: account-public-service-prod.ol.epicgames.com
Content-Type: application/x-www-form-urlencoded
Authorization: Basic ZWM2ODRiOGM2ODdmNDc5ZmFkZWEzY2IyYWQ4M2Y1YzY6ZTFmMzFjMjExZjI4NDEzMTg2MjYyZDM3YTEzZmM4NGQ=

grant_type=authorization_code&code=REPLACE_THIS
```

レスポンスはこちら

```json
{
  "access_token": "7c0aa5ad4721454a81a0f48683715978",
  "expires_in": 7200,
  "expires_at": "2023-11-12T13:06:47.813Z",
  "token_type": "bearer",
  "refresh_token": "a2f033647d414de2bbe04b1c12b5b5ee",
  "refresh_expires": 28800,
  "refresh_expires_at": "2023-11-12T19:06:47.814Z",
  "account_id": "94b1569506b04f9f8557af611e8c5e47",
  "client_id": "ec684b8c687f479fadea3cb2ad83f5c6",
  "internal_client": true,
  "client_service": "prod-fn",
  "scope": ["basic_profile"],
  "displayName": "lele stw moment",
  "app": "prod-fn",
  "in_app_id": "94b1569506b04f9f8557af611e8c5e47",
  "device_id": "63e3b71af76e4fc48841b92c5eb3d69d",
  "product_id": "prod-fn",
  "application_id": "fghi4567FNFBKFz3E4TROb0bmPS8h1GW"
}
```
