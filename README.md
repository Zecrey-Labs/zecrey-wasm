# zecrey-wasm
## prerequisites

- Node: v15.10.0
- install live-server: `npm install -g live-server`

## Have a try

```sh
$ live-server static

# if you want to see the running time, add console.time('a'); before the command and console.timeEnd('a'); after the command
# Call F12 in chrome
> elgamalDec('PSjJUWon0WV1gQA/SxovPRS0XCyVdGpq0O+Ttjh+ZBe38+GdDtFhVNMrDZQKtfaZ5e1oPkgO2LRoxTMGp1pPoQ==','959120971630916186538306178761323785168020249928470126158942387495545940088',0,20000)
# it will output 10000
> proveWithdraw(1,1,1,1,'0x99AC8881834797ebC32f185ee27c2e96842e1a47','{"enc_balance":"ehn2xKgpIHu5SMwlMxcuuvZSc0hcdTLz5JjFn/QMEq8opyxNzUPvAz64jPYn3WIhivJVuVs5l3oalK4yRYRvDA==", "pk":"Jt9amF32qNqu1AqkImUIiu+jqPVtlgzJSAMONS8LbRU=", "b_star":2,"balance":8,"sk":"291506282145866059790720920090307253831111469240911238719036914525276664321"}')
# it will output withdraw proof and cost 700ms, native version only cost 10ms
> proveTransfer(1,1,1,'[1,2,3]','[{"enc_balance":"YFreXNLjjupfrN/6nBExMjpj9Mj4tYIWP14QN5O86R6skYFlwtMgVpnfJwwgHgOxrxHiz2qLV3pXN8JVr7WEiQ==", "balance":8,"pk":"IeC1BydfXzJ7Ve+AbeahpOSyUX8oi3+VyR6zJT8fHw0=", "b_delta":-5,"sk":"499949885387816668586237702202862308748107211143318739581312585461274422492"},{"enc_balance":"oszpzl4/u0907OG3rnisQnnpJ3jBo/TTy7rS5Dc3lI7tK35ImqWlFErYeAmqZc3S2pnDei0uzMCyuXNeulcYlQ==", "pk":"SCmI6f5AwYXR6MkNH9xlzS9LRjmbYx2iMtqnS56jAC8=", "b_delta":1},{"enc_balance":"hjzILvNsNV5zBkEb+szn1N6O4SHlxKxl5VpA8nf0RyrRirf/lcTJrWFVYe5+7FjZnUakPtN0lLnQN0zJyi9oGA==", "pk":"6zOn8d30IZoV4Yc9m4j/BMq1qvQOuhIxICjVrq4t5gY=", "b_delta":3}]')
# it will output transfer proof and cost 2s, native version only cost 20ms
> getL2PublicKey('80827862280522504726693779669318903631091212680164437996679970328808864133111112')
# it will output "qCAriSoHPX89biS6uXqFgRZRw7EqvGp6hkEDUVhE3CU="
> proveSwap(1,2,1,2,1,1,1,'{"enc_balance":"8Q1NcIhLUy/g46edGtzjtgRdsKUyMsWFxA7j41le/Zf0ai14YrdtPkO87TbI/YYuZv5xoWyBvblpNSbL3XCgkw==","balance":8,"pk":"F6YftwB+pLeIhgl5Vm77JIUe2b+Cg+kZUiu0qBl7ooU=","b_star_from":1,"b_star_to":8,"sk":"1017724826210560602038762767050968580741611470742637863248271997587646338143","receiver_enc_balance":"P3XYxDwnPMUPjmoiddxFOuX8ZJQrdyb2toHwaLK1EZFY9s5mW1B3dtzZR/fY4iw2ZYy/YkjOgMF7rq2ZSQ4+lQ==","receiver_pk":"E7uhoHsRk0RH3T9+I0BEP4N3mmccQoTB5gjoI13goAI="}')
# it will cost nearly 700ms

> proveL1PrivacyTransfer('0xb1c297bBb2DC33F3c68920F02e88d2746b2F456d',3,10,'0x001',1,0,2,'[3,1,5]','[{"enc_balance":"e/Ic2q8QlxnJ626mTjKBfHgfUr39ekiSJa93F4OXnq6qmCA1gkLK/ZqUYLjB67dkl6XU3+tAvx0DgHnH0lIfrw==", "balance":100,"pk":"VKASf3Li/fLPdUlUYvOHJxVO4GBMEd8S7tVczCWNIwA=", "b_delta":-12,"sk":"307595295600029199081694065758883148461167161923180057764109328721502007902"},{"enc_balance":"Wo3uv2rpgHNMrhemH/MlM72i/qOQt92PaRdo2PARvylGK7yGvwpOgpkc+qcIK0QfIBIBeFeOUHak1QDOooNAlg==", "pk":"W+xuOsgTYAVn0xPGShbKM63qcGZPflB4EdgqO2bJ9Co=", "b_delta":10},{"enc_balance":"YwoTmOIccu8Ioug7tOu7xF+O9kUT6q5GC4Jt80XWf64CzaBIKm81EG+gU2tI2MTqdx3MNeulFrZfDPbt64pVKQ==", "pk":"uri0rrhcxsIcoEdVlIQzt06IhKwE61Q7q0gQEgr0mSg=", "b_delta":0}]')

```



## SDK

### elgamalEnc

| Name | Type   | Comment                                       |
| ---- | ------ | --------------------------------------------- |
| sk   | string | private key, 251bits, type is BigInt, base 10 |
| b    | int    | amount                                        |

### elgamalDec

| Name  | Type   | Comment                                       |
| ----- | ------ | --------------------------------------------- |
| CStr  | string | encryption value, 64B                         |
| sk    | string | private key, 251bits, type is BigInt, base 10 |
| start | int    | dec start value, default use 0                |
| end   | int    | dec end value ,default use $2^{32}$           |

### getL2PublicKey

| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| sk   | string | private key, length range 251~256, type is BigInt, base 10 |

### proveWithdraw

| Name         | Type                                                         | Comment                                                      |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| chainId      | int                                                          | chain id                                                     |
| assetId      | int                                                          | unique token id                                              |
| fee          | int                                                          | transaction fee                                              |
| accountIndex | int                                                          | layer 2 account index                                        |
| l1address    | string                                                       | layer 1 address that the users wants to withdraw to          |
| segmentJSON  | string<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_star: int<br />sk: string} | enc_balance: encryption value, 64B<br />balance: balance related to enc_balance<br />pk: public key, 32B <br />b_star: withdraw amount, should be negative<br />sk: private key, 251bits, type is BigInt, base 10 |

### proveTransfer

| Name          | Type                                                         | Comment                                                      |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| chainId       | int                                                          | chain id                                                     |
| assetId       | int                                                          | unique token id                                              |
| fee           | int                                                          | transaction fee                                              |
| accountsIndex | string                                                       | user accounts indexes(including the sender), original type is []int, use JSON.stringify() |
| segmentJSON   | string:<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_delta: int<br />sk: string} | enc_balance: encryption value, 64B<br />*balance: balance related to enc_balance, only pass when sk != null<br />pk: public key, 32B <br />b_delta: transfer amount, if sk is null, this value should bigger than 0, otherwise, smaller than 0<br />sk: private key, 251bits, type is BigInt, base 10 |

### proveSwap

| Name         | Type                                                         | Comment                                                      |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| chainIdFrom  | int                                                          | swap from which chain                                        |
| chainIdTo    | int                                                          | swap to which chain                                          |
| fromAssetId  | int                                                          | swap from token id                                           |
| toAssetId    | int                                                          | swap to token id                                             |
| fee          | int                                                          | transaction fee                                              |
| accountIndex | int                                                          | sender account index                                         |
| segmentJSON  | string:<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_star_from: int<br />b_star_to:int<br />sk: string} | enc_balance: encryption value, 64B<br />balance: balance releated to enc_balance<br />pk: public key, 32B <br />b_star_from: swap from amount<br />b_star_to: swap to amount<br />sk: private key, 251bits, type is BigInt, base 10 |

### proveL1PrivacyTransfer

| Name             | Type                                                         | Comment                                                      |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| withdrawTo       | string                                                       | withdraw to which layer 1 address                            |
| accountIndex     | int                                                          | account index for the sender                                 |
| depositAmount    | int                                                          | deposit amount(Zecrey Unit)                                  |
| depositTxHash    | string                                                       | deposit tx hash                                              |
| chainId          | int                                                          | chain id                                                     |
| assetId          | int                                                          | asset id                                                     |
| fee              | int                                                          | privacy fee(Zecrey Unit)                                     |
| accountsIndexStr | string                                                       | user accounts indexes(including the sender), original type is []int, use JSON.stringify() |
| segmentJSON      | string:<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_delta: int<br />sk: string} | enc_balance: encryption value, 64B<br />*balance: balance related to enc_balance, only pass when sk != null<br />pk: public key, 32B <br />b_delta: transfer amount, if sk is null, this value should bigger than 0, otherwise, smaller than 0<br />sk: private key, 251bits, type is BigInt, base 10 |

### Error Types

| Name                             | Value                            |
| -------------------------------- | -------------------------------- |
| Success                          | success                          |
| ErrUnmarshal                     | ErrUnmarshal                     |
| ErrInvalidWithdrawParams         | ErrInvalidWithdrawParams         |
| ErrParseEnc                      | ErrParseEnc                      |
| ErrParsePoint                    | ErrParsePoint                    |
| ErrParseBigInt                   | ErrParseBigInt                   |
| ErrInvalidWithdrawRelationParams | ErrInvalidWithdrawRelationParams |
| ErrProveWithdraw                 | ErrProveWithdraw                 |
| ErrMarshalTx                     | ErrMarshalTx                     |
| ErrInvalidTransferParams         | ErrInvalidTransferParams         |
| ErrInvalidTransferRelationParams | ErrInvalidTransferRelationParams |
| ErrProveTransfer                 | ErrProveTransfer                 |
| ErrL2SkParams                    | ErrL2SkParams                    |
| ErrInvalidEncParams              | ErrInvalidEncParams              |
| ErrElGamalEnc                    | ErrElGamalEnc                    |
| ErrInvalidDecParams              | ErrInvalidDecParams              |
| ErrElGamalDec                    | ErrElGamalDec                    |
| ErrInvalidSwapParams             | ErrInvalidSwapParams             |
| ErrInvalidSwapRelationParams     | ErrInvalidSwapRelationParams     |