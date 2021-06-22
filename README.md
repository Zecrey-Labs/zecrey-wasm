# zecrey-wasm
## prerequisites

- Node: v15.10.0
- install live-server: `npm install -g live-server`

## Have a try

```sh
$ live-server static

# Call F12 in chrome
> elgamalDec('1401dbd7badfacbd1a1223fd6ce342f0c18f2b6562bd69654a6328c61dfb21a9e731bc07581c07a9c1b4aad3629941ef66c883584cbd60b8b87d35663944a485','808278622805225047266937796693189036310912126801644379966799703288088641332',0,100)
# it will output 8
> proveWithdraw(1,'sher','0xE9b15a2D396B349ABF60e53ec66Bcf9af262D449','{"enc_val":"1401dbd7badfacbd1a1223fd6ce342f0c18f2b6562bd69654a6328c61dfb21a9e731bc07581c07a9c1b4aad3629941ef66c883584cbd60b8b87d35663944a485", "pk":"6ebe89760a2d5a1759b96c87e2e013ebfd1e659ccd8aadfb21d9cd16d9bab80e", "b_star":-2,"sk":"808278622805225047266937796693189036310912126801644379966799703288088641332"}')
# it will output withdraw proof and cost 600ms, native version only cost 20ms
> proveTransfer(1,'["sher","lin","lzp"]','[{"enc_val":"595b82141788f8f1a96a560ad6140f659682d728b77c8ee70ccf96c5be03960b0899e37cba03fad3c052bccf746926a4fab3352a118965c797d67e04993b50ae", "pk":"731f4e1e9c839688c6baac16601b558022c3b27a1f5eee6b633439927a979fa7", "b_delta":-4,"sk":"367676241680898931196669992568589436958624636382082109130432125664493906855"},{"enc_val":"d03f7eb303260882ea703f0c65043c4b2baacddba403db93fba6c96341ba8b87f824e60aa144b91d8e71aa5547e684317a4ca13c407b35e9732afb373259998d", "pk":"2f73fcea812db185cd7447d80fcd81d82cc93b45a4c3533afb0f59d0186a15b0", "b_delta":1},{"enc_val":"b83921e4d8cee3e462b13b8051f942a4ccf7eefb540a5ad953d141bef4970104f98640ad11d42a34a929259705835d2348d8e1b379f27a2b1ab60f18d781109d", "pk":"ad2a965336945fa6bbbd88dc3c8f4c2d35da3015d72673b570d7d524e0c3069e", "b_delta":3}]')
# it will output transfer proof and cost 2.3s, native version only cost 70ms
> getL2PublicKey('80827862280522504726693779669318903631091212680164437996679970328808864133111112')
# it will output "a8202b892a073d7f3d6e24bab97a85811651c3b12abc6a7a864103515844dc25"
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

| Name        | Type                                                         | Comment                                                      |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| tokenId     | int                                                          | unique token id                                              |
| l2address   | string                                                       | layer 2 address                                              |
| l1address   | string                                                       | layer 1 address that the users wants to withdraw to          |
| segmentJSON | string<br />{<br />enc_val: string<br />pk:string<br />b_star: int<br />sk: string} | enc_val: encryption value, 64B<br />pk: public key, 32B <br />b_star: withdraw amount, should be negative<br />sk: private key, 251bits, type is BigInt, base 10 |

### proveTransfer

| Name        | Type                                                         | Comment                                                      |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| tokenId     | int                                                          | unique token id                                              |
| l2addresses | string                                                       | user addresses(including the sender), original type is []string, use JSON.stringify() |
| segmentJSON | string:<br />{<br />enc_val: string<br />pk:string<br />b_delta: int<br />sk: string} | enc_val: encryption value, 64B<br />pk: public key, 32B <br />b_delta: transfer amount, if sk is null, this value should bigger than 0, otherwise, smaller than 0<br />sk: private key, 251bits, type is BigInt, base 10 |

