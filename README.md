# zecrey-wasm
## prerequisites

- Node: v15.10.0
- install live-server: `npm install -g live-server`

## Have a try

```sh
$ live-server static

# if you want to see the running time, add console.time('a'); before the command and console.timeEnd('a'); after the command
# Call F12 in chrome
> elgamalDec('1401dbd7badfacbd1a1223fd6ce342f0c18f2b6562bd69654a6328c61dfb21a9e731bc07581c07a9c1b4aad3629941ef66c883584cbd60b8b87d35663944a485','808278622805225047266937796693189036310912126801644379966799703288088641332',0,100)
# it will output 8
> proveWithdraw(1,1,1,'0x99AC8881834797ebC32f185ee27c2e96842e1a47','{"enc_balance":"01bb17ef693c71e1ed706e2b7423974db3fadd302b0552539918298ae7db858a6b6f9d9710e2fa8d6434f38f99bf787467fee7b1026b55fd358720a173091204", "pk":"675fbcb52e33ce755f68e41b9f2cae583dfbee4837b3356a48a4bc25b634f685", "b_star":2,"balance":8,"sk":"1399489033395740343038180545196200507902571842419537327615823798933191405004"}')
# it will output withdraw proof and cost 700ms, native version only cost 14ms
> proveTransfer(1,1,'[1,2,3]','[{"enc_balance":"595b82141788f8f1a96a560ad6140f659682d728b77c8ee70ccf96c5be03960b0899e37cba03fad3c052bccf746926a4fab3352a118965c797d67e04993b50ae", "balance":8,"pk":"731f4e1e9c839688c6baac16601b558022c3b27a1f5eee6b633439927a979fa7", "b_delta":-5,"sk":"367676241680898931196669992568589436958624636382082109130432125664493906855"},{"enc_balance":"d03f7eb303260882ea703f0c65043c4b2baacddba403db93fba6c96341ba8b87f824e60aa144b91d8e71aa5547e684317a4ca13c407b35e9732afb373259998d", "pk":"2f73fcea812db185cd7447d80fcd81d82cc93b45a4c3533afb0f59d0186a15b0", "b_delta":1},{"enc_balance":"b83921e4d8cee3e462b13b8051f942a4ccf7eefb540a5ad953d141bef4970104f98640ad11d42a34a929259705835d2348d8e1b379f27a2b1ab60f18d781109d", "pk":"ad2a965336945fa6bbbd88dc3c8f4c2d35da3015d72673b570d7d524e0c3069e", "b_delta":3}]')
# it will output transfer proof and cost 2s, native version only cost 40ms
> getL2PublicKey('80827862280522504726693779669318903631091212680164437996679970328808864133111112')
# it will output "a8202b892a073d7f3d6e24bab97a85811651c3b12abc6a7a864103515844dc25"
> proveSwap(1,2,1,1,'{"enc_balance":"db7c675ca3b471ee07ceadac4ed02a35bc88536f29b19c4e52ee81fad8002219b54bbc61f5f8ffe6d3884a68440c815c064542c3ea0f49771980aed31aadeb05","balance":8,"pk":"870a03c72aa3e9da57474efaa5608908fce95b3d9a081bee238c090363dc8620","b_star_from":1,"b_star_to":8,"sk":"612800936073666486462885240316406204259890570277023800874304252835913873306","receiver_enc_balance":"a872da9d92500f261e06035731a3d1a6dcaa2cf6e77198b8334c746ada1cdc980171c1043d86dff15adfe3b3ec1d754706f81efc05b9417a8245037a9c22dd80","receiver_pk":"d0b68342cf0bf2b3b363fc8e02918b91c8df1cbdb7b613db6f92febd091f72a2"}')
# it will cost nearly 700ms
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
| tokenId      | int                                                          | unique token id                                              |
| fee          | int                                                          | transaction fee                                              |
| accountIndex | int                                                          | layer 2 account index                                        |
| l1address    | string                                                       | layer 1 address that the users wants to withdraw to          |
| segmentJSON  | string<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_star: int<br />sk: string} | enc_balance: encryption value, 64B<br />balance: balance related to enc_balance<br />pk: public key, 32B <br />b_star: withdraw amount, should be negative<br />sk: private key, 251bits, type is BigInt, base 10 |

### proveTransfer

| Name          | Type                                                         | Comment                                                      |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| tokenId       | int                                                          | unique token id                                              |
| fee           | int                                                          | transaction fee                                              |
| accountsIndex | string                                                       | user accounts indexes(including the sender), original type is []int, use JSON.stringify() |
| segmentJSON   | string:<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_delta: int<br />sk: string} | enc_balance: encryption value, 64B<br />*balance: balance related to enc_balance, only pass when sk != null<br />pk: public key, 32B <br />b_delta: transfer amount, if sk is null, this value should bigger than 0, otherwise, smaller than 0<br />sk: private key, 251bits, type is BigInt, base 10 |

### proveSwap

| Name         | Type                                                         | Comment                                                      |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| fromTokenId  | int                                                          | swap from token id                                           |
| toTokenId    | int                                                          | swap to token id                                             |
| fee          | int                                                          | transaction fee                                              |
| accountIndex | int                                                          | sender account index                                         |
| segmentJSON  | string:<br />{<br />enc_balance: string<br />balance: int<br />pk:string<br />b_star_from: int<br />b_star_to:int<br />sk: string} | enc_balance: encryption value, 64B<br />balance: balance releated to enc_balance<br />pk: public key, 32B <br />b_star_from: swap from amount<br />b_star_to: swap to amount<br />sk: private key, 251bits, type is BigInt, base 10 |

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