# API Keyを発行する

API　Keyの発行申請は [こちら](https://ir9l8pcvcmm.typeform.com/to/FSREILsN?typeform-source=hokusai.app). You will receive the key, which contains `HOKUSAI_API_KEY` and `HOKUSAI_CONTRACT_ID`. 発行申請から2~3営業日以内にAPI発行いたします。

## 認証
Hokusai API では全てのリクエストに対して、有効性を確認するために認証を要求します。

ユーザはリクエストの認証を行うために、URLクエリの `key` パラメータに API Key を指定します。
以下はリクエストの例です。

```:bash
https://api.hokusai.app/v1/nfts/{contractAddress}/mint?key={apiKey}
```

このように、全てのエンドポイントには、API Key を値として持つ `key` パラメータの指定が必要となります。
All endpoints must include the `key` parameter with the API key.
