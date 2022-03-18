# API Keyを発行する

API Keyの発行申請は [こちら](https://0xhokusai.notion.site/Hokusai-API-Application-form-a6d8118d416b41d88632396e3156cddb). You will receive the key, which contains `HOKUSAI_API_KEY` and `HOKUSAI_CONTRACT_ID`. 発行申請から2~3営業日以内にAPI発行いたします。

## 認証
Hokusai では全てのリクエストに対して、有効性を確認するために認証を要求します。

ユーザはリクエストの認証を行うために、URLクエリの `key` パラメータに API Key を指定します。
以下はリクエストの例です。

<!--
type: tab
title: v2
-->

```:bash
https://api.hokusai.app/v2/{network}/nft/{contractVersion/{contractAddress}/mint?key={apiKey}
```

<!--
type: tab
title: v1
-->

```:bash
https://api.hokusai.app/v1/nfts/{contractAddress}/mint?key={apiKey}
```

<!-- type: tab-end -->


このように、全てのエンドポイントには、API Key を値として持つ `key` パラメータの指定が必要となります。
