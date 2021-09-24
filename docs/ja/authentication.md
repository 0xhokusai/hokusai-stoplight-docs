# API Keyを発行する

API　Keyの発行申請は [こちら](https://ir9l8pcvcmm.typeform.com/to/FSREILsN?typeform-source=hokusai.app). You will receive the key, which contains `HOKUSAI_API_KEY` and `HOKUSAI_CONTRACT_ID`. 発行申請から2~3営業日以内にAPI発行いたします。

## 認証
Hokusai authenticates all API requests using API keys. 
To authenticate requests, you should specify the API key in the `key` parameter of the URL query.
Here is an example of a request.
```:bash
https://api.hokusai.app/v1/nfts/{contractAddress}/mint?key={apiKey}
```
All endpoints must include the `key` parameter with the API key.
