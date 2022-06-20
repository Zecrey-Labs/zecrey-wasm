# zecrey-legend-wasm
## prerequisites
- Node: v15.10.0

## SDK

### getAccountNameHash

#### Description

get account name hash for ZNS

#### Params

| Name        | Type   | Comment                                       |
| ----------- | ------ | --------------------------------------------- |
| accountName | string | account name, such as sher.legend or sher.eth |

#### Example

```bash
> globalThis.getAccountNameHash("sher.legend")
```

### getEddsaPublicKey

#### Description
generate public key

#### Params
| Name | Type   | Comment                                       |
| ---- | ------ | --------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |


#### Example
```bash
# Call F12 in chrome
> globalThis.getEddsaPublicKey("seed phrase")
# '22fc6f5d74c8639245462a0af6b5c931bd209c04034b28421a60336635ab85950a3163e68ec29319ca200fac009408369b0a1f75200a118aded920cd240e1358'
```

### getEddsaCompressedPublicKey

#### Description

generate public key

#### Params

| Name | Type   | Comment                                       |
| ---- | ------ | --------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |


#### Example

```bash
# Call F12 in chrome
> globalThis.getEddsaCompressedPublicKey("seed phrase")
# '58130e24cd20d9de8a110a20751f0a9b36089400ac0f20ca1993c28ee663318a'
```

### generateEddsaKey

#### Description
generate private key

#### Params
| Name | Type   | Comment                                       |
| ---- | ------ | --------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |

#### Example
```bash
# Call F12 in chrome
> globalThis.eddsaSign("seed phrase", "hello world")
# '88b80d96d3eadb67ad7e8d86b731ce4b1bab5a46bc8c33fbf8b346a658007e90024f87ff59fb24adbd8ae56c6880b24c9937807eda097afcb26f0590e4eca860'
```


### eddsaSign
#### Description
sign message and generate signature

#### Params
| Name | Type   | Comment                                       |
| ---- | ------ | --------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| message | string | message string  |

#### Example
```bash
# Call F12 in chrome
> globalThis.generateEddsaKey("seed phrase")
# '06cdb3200f1e0e7dd7ea789b41d88662a1b4c213075633088773025289bdbd05137c9eeead2d4286a9779a71ed25f5da61fe2790e21f2437b909dfd76eeb00ce53896c61c2948dd9018661b0928cca52ca05a2e764c81a7f8badc9b27c4ca144'
```

### eddsaVerify
#### Description
verify signature with public key and message

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| pk   | string | public key, 256 bits, base 16  |
| signature   | string | signature string  |
| message   | string | message string  |

#### Example
```bash
# Call F12 in chrome
> globalThis.eddsaVerify("06cdb3200f1e0e7dd7ea789b41d88662a1b4c213075633088773025289bdbd05", "88b80d96d3eadb67ad7e8d86b731ce4b1bab5a46bc8c33fbf8b346a658007e90024f87ff59fb24adbd8ae56c6880b24c9937807eda097afcb26f0590e4eca860", "hello world")
# true
```

### signAddLiquidity
#### Description
sign addLiquidity transaction

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct

```go
type AddLiquiditySegmentFormat struct {
    FromAccountIndex  int64  `json:"from_account_index"`
    PairIndex         int64  `json:"pair_index"`
    AssetAId          int64  `json:"asset_a_id"`
    AssetAAmount      string `json:"asset_a_amount"`
    AssetBId          int64  `json:"asset_b_id"`
    AssetBAmount      string `json:"asset_b_amount"`
    LpAmount          string `json:"lp_amount"`
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    ExpiredAt         int64  `json:"expired_at"` 
	// transaction expire time in milli-second type  
	// eg. current timestamp + 10 minutes
    Nonce             int64  `json:"nonce"`  
	// transaction amount +1 for fromAccountIndex 
}
```
#### Example
```bash
# Call F12 in chrome
> globalThis.signAddLiquidity("seed phrase", '{"from_account_index":0,"pair_index":0,"asset_a_id":1,"asset_a_amount":"10000","asset_b_id":2,"asset_b_amount":"100","lp_amount":"1000","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"FromAccountIndex":0,"PairIndex":0,"AssetAId":1,"AssetAAmount":10000,"AssetBId":2,"AssetBAmount":100,"LpAmount":1000,"KLast":null,"TreasuryAmount":null,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"QgkTDbEq3Pq7AjidooPyfHmlSa1VuBAgqv57XjOT7yQC6OzNBv6YQLSm6U1BmPKA/qzFhfpnVFR8jL64kX/W+g=="}'
```

### signRemoveLiquidity
#### Description
sign removeLiquidity transaction


#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type RemoveLiquiditySegmentFormat struct {
    FromAccountIndex  int64  `json:"from_account_index"`
    PairIndex         int64  `json:"pair_index"`
    AssetAId          int64  `json:"asset_a_id"`
    AssetAMinAmount   string `json:"asset_a_min_amount"`
	// minimum amount assetA 'from' user will receive by burning 'lpAmount' lp tokens , it depends on slippage
    AssetBId          int64  `json:"asset_b_id"`
    AssetBMinAmount   string `json:"asset_b_min_amount"`
	// minimum amount assetB 'from' user will receive by burning 'lpAmount' lp tokens , it depends on slippage
    LpAmount          string `json:"lp_amount"`
    AssetAAmountDelta string `json:"asset_a_amount_delta"`
	// AssetA amount delta in theory
    AssetBAmountDelta string `json:"asset_b_amount_delta"`
	// AssetB amount delta in theory
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    ExpiredAt         int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 10 minutes
    Nonce             int64  `json:"nonce"`
	// transaction amount +1 for fromAccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signRemoveLiquidity("seed phrase", '{"from_account_index":0,"pair_index":0,"asset_a_id":1,"asset_a_min_amount":"9000","asset_b_id":2,"asset_b_min_amount":"90","lp_amount":"1000","asset_a_amount_delta":"10000","asset_b_amount_delta":"100","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"FromAccountIndex":0,"PairIndex":0,"AssetAId":1,"AssetAMinAmount":9000,"AssetBId":2,"AssetBMinAmount":90,"LpAmount":1000,"AssetAAmountDelta":10000,"AssetBAmountDelta":100,"KLast":null,"TreasuryAmount":null,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"KCrHHfiioPSTtm3FKWnJAEDPAYEyjEGnN+LOHyaZAIUFgTYZNdk+HsYppD+Cz1/Ekd7tR7onrGESbFqRAAnsWg=="}'
```

### signSwap
#### Description
sign swap transaction


#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type SwapSegmentFormat struct {
    FromAccountIndex  int64  `json:"from_account_index"`
    PairIndex         int64  `json:"pair_index"`
    AssetAId          int64  `json:"asset_a_id"`
    AssetAAmount      string `json:"asset_a_amount"`
    AssetBId          int64  `json:"asset_b_id"`
    AssetBMinAmount   string `json:"asset_b_min_amount"`
	// minimum amount assetB 'from' user will receive by swapping 'AssetAAmount' AssetA , it depends on slippage
    AssetBAmountDelta string `json:"asset_b_amount_delta"`
	// AssetB amount delta in theory

    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    ExpiredAt         int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 10 minutes
    Nonce             int64  `json:"nonce"`
	// transaction amount +1 for fromAccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signSwap("seed phrase", '{"from_account_index":0,"pair_index":0,"asset_a_id":1,"asset_a_amount":"10000","asset_b_id":2,"asset_b_min_amount":"95","asset_b_amount_delta":"99","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"FromAccountIndex":0,"PairIndex":0,"AssetAId":1,"AssetAAmount":10000,"AssetBId":2,"AssetBMinAmount":95,"AssetBAmountDelta":99,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"cVvwCUOydTtp6ofqJfSz+rzs9OU9/tzACj1QSUSX84sCFRkYw0zoJPQr0sN7btHKZFxNj4ARb6tqNiUybKgm5g=="}'
```

### signTransfer
#### Description
sign transfer transaction


#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type TransferSegmentFormat struct {
    FromAccountIndex  int64  `json:"from_account_index"`
    ToAccountIndex    int64  `json:"to_account_index"`
    ToAccountNameHash string `json:"to_account_name"`
	// AccountName SHA256 Hash
	/*
	Code in JS

	const { createHash } = require('crypto');

	function hash(string) {
		return createHash('sha256').update(string).digest('hex');
	}	
	console.log(hash('gavin.zecrey'));
	# ddc6171f9fe33153d95c8394c9135c277eb645401b85eb499393a2aefe6422a6
	*/

    AssetId           int64  `json:"asset_id"`
    AssetAmount       string `json:"asset_amount"`
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    Memo              string `json:"memo"`
	// Memo length range: [0:100] 
    CallData          string `json:"call_data"`
    ExpiredAt         int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 10 minutes
    Nonce             int64  `json:"nonce"`
	// transaction amount +1 for fromAccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signTransfer("seed phrase", '{"from_account_index":0,"to_account_index":1,"to_account_name":"ddc6171f9fe33153d95c8394c9135c277eb645401b85eb499393a2aefe6422a6","asset_id":0,"asset_amount":"100","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","memo":"transfer memo","call_data":"","expired_at":1654656781000,"nonce":1}')
# '{"FromAccountIndex":0,"ToAccountIndex":1,"ToAccountNameHash":"ddc6171f9fe33153d95c8394c9135c277eb645401b85eb499393a2aefe6422a6","AssetId":0,"AssetAmount":100,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"Memo":"transfer memo","CallData":"","CallDataHash":"Dd56AihX/sG4/6dmSpN6JQ065o81YGF1TTUx4mdBA9g=","ExpiredAt":1654656781000,"Nonce":1,"Sig":"qkZAheH1WxQDSKxZlp+wPyWrVZxgqjkNQYbbHfSyLCgCsgjwbXI9s0hS+e+ZB0XRYBEJi/KZ/otXtmF/o67n0Q=="}'
```


### signWithdraw
#### Description
sign withdraw transaction


#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type WithdrawSegmentFormat struct {
    FromAccountIndex  int64  `json:"from_account_index"`
    AssetId           int64  `json:"asset_id"`
    AssetAmount       string `json:"asset_amount"`
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    ToAddress         string `json:"to_address"`
	// L1 address based on ecdsa. 
	// eg. 0x507Bd54B4232561BC0Ca106F7b029d064fD6bE4c
    ExpiredAt         int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 10 minutes
    Nonce             int64  `json:"nonce"`
	// transaction amount +1 for fromAccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signWithdraw("seed phrase", '{"from_account_index":0,"asset_id":0,"asset_amount":"100","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","to_address":"0x507Bd54B4232561BC0Ca106F7b029d064fD6bE4c","expired_at":1654656781000,"nonce":1}')
# '{"FromAccountIndex":0,"AssetId":0,"AssetAmount":100,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ToAddress":"0x507Bd54B4232561BC0Ca106F7b029d064fD6bE4c","ExpiredAt":1654656781000,"Nonce":1,"Sig":"nvZFgteheqYDAqRipkE5VtpkQt0ppu1VVlqpOnQn8o0BNkq0yvwQFhrEOTtgBliP3aaVqNSkHmEqtUVID5DOlg=="}'
```

### signOffer
#### Description
sign offer nft transaction


#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type OfferSegmentFormat struct {
    Type         int64  `json:"type"`
	// 0 : buy/ 1: sell
    OfferId      int64  `json:"offer_id"`
	// maxOfferId +1 for AccountIndex 
    AccountIndex int64  `json:"account_index"`
    NftIndex     int64  `json:"nft_index"`
    AssetId      int64  `json:"asset_id"`
    AssetAmount  string `json:"asset_amount"`
    ListedAt     int64  `json:"listed_at"`
	// transaction list time in milli-second type  
	// eg. current timestamp
    ExpiredAt    int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 1 week
    TreasuryRate int64  `json:"treasury_rate"`
	// treasury fee rate, it can be found by query api. 1 is corresponding to one ten thousandth. 
	// eg. 30 => 0.3% ; 2000 => 20%
}
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signOffer("seed phrase", '{"type":0,"offer_id":1,"account_index":1,"nft_index":1500,"asset_id":1,"asset_amount":"10000","listed_at":1654656761000,"expired_at":1654656781000,"treasury_rate":200}')
# '{"Type":0,"OfferId":1,"AccountIndex":1,"NftIndex":1500,"AssetId":1,"AssetAmount":10000,"ListedAt":1654656761000,"ExpiredAt":1654656781000,"TreasuryRate":200,"Sig":"f7EryTm0P7xCgDYsyB+R+Of3ZHHyVa4uEI721shjoQgFdYuoMst49X0NFf9MraQevweNVH+728FHh0c1hEz20A=="}'
```


### signAtomicMatch
#### Description
sign atomicMatch transaction


#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type AtomicMatchSegmentFormat struct {
	AccountIndex int64  `json:"account_index"`
	BuyOffer     string `json:"buy_offer"`
	// OfferTxInfo JSON String
	SellOffer string `json:"sell_offer"`
	// OfferTxInfo JSON String
	GasAccountIndex   int64  `json:"gas_account_index"`
	GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
	GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
	Nonce             int64  `json:"nonce"`
	// transaction amount +1 for AccountIndex 
	ExpiredAt int64 `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 1 week
}

type OfferTxInfo struct {
    Type         int64
    OfferId      int64
	// maxOfferId +1 for AccountIndex 
    AccountIndex int64
    NftIndex     int64
    AssetId      int64
    AssetAmount  *big.Int
    ListedAt     int64
	// transaction list time in milli-second type  
	// eg. current timestamp
    ExpiredAt    int64
	// transaction expire time in milli-second type  
	// eg. current timestamp + 1 week
    TreasuryRate int64
	// treasury fee rate, it can be found by query api. 1 is corresponding to one ten thousandth. 
	// eg. 30 => 0.3% ; 2000 => 20%
    Sig          []byte
}

/*
	signOffer returns OfferTxInfo String:

	Buy offer:
	'{"Type":0,"OfferId":1,"AccountIndex":1,"NftIndex":1500,"AssetId":1,"AssetAmount":10000,"ListedAt":1654656761000,"ExpiredAt":1654656781000,"TreasuryRate":200,"Sig":"f7EryTm0P7xCgDYsyB+R+Of3ZHHyVa4uEI721shjoQgFdYuoMst49X0NFf9MraQevweNVH+728FHh0c1hEz20A=="}'

	Sell offer:
	'{"Type":1,"OfferId":1,"AccountIndex":2,"NftIndex":1500,"AssetId":1,"AssetAmount":10000,"ListedAt":1654656751000,"ExpiredAt":1654656791000,"TreasuryRate":200,"Sig":"cCh08P8RloU+uNZESVVbl5mqOFiiXR2JRJaAnmqxz6gCBXny2J9OUh5X7tRHaEBxDRRXQ1mQGMVMoe1/ncw3sQ=="}'
*/
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signAtomicMatch("seed phrase", "{\"account_index\":0,\"buy_offer\":\"{\\\"Type\\\":0,\\\"OfferId\\\":1,\\\"AccountIndex\\\":1,\\\"NftIndex\\\":1500,\\\"AssetId\\\":1,\\\"AssetAmount\\\":10000,\\\"ListedAt\\\":1654656761000,\\\"ExpiredAt\\\":1654656781000,\\\"TreasuryRate\\\":200,\\\"Sig\\\":\\\"f7EryTm0P7xCgDYsyB+R+Of3ZHHyVa4uEI721shjoQgFdYuoMst49X0NFf9MraQevweNVH+728FHh0c1hEz20A==\\\"}\",\"sell_offer\":\"{\\\"Type\\\":1,\\\"OfferId\\\":1,\\\"AccountIndex\\\":2,\\\"NftIndex\\\":1500,\\\"AssetId\\\":1,\\\"AssetAmount\\\":10000,\\\"ListedAt\\\":1654656751000,\\\"ExpiredAt\\\":1654656791000,\\\"TreasuryRate\\\":200,\\\"Sig\\\":\\\"cCh08P8RloU+uNZESVVbl5mqOFiiXR2JRJaAnmqxz6gCBXny2J9OUh5X7tRHaEBxDRRXQ1mQGMVMoe1/ncw3sQ==\\\"}\",\"gas_account_index\":1,\"gas_fee_asset_id\":3,\"gas_fee_asset_amount\":\"3\",\"nonce\":1,\"expired_at\":1654656781000}")
# '{"AccountIndex":0,"BuyOffer":{"Type":0,"OfferId":1,"AccountIndex":1,"NftIndex":1500,"AssetId":1,"AssetAmount":10000,"ListedAt":1654656761000,"ExpiredAt":1654656781000,"TreasuryRate":200,"Sig":"f7EryTm0P7xCgDYsyB+R+Of3ZHHyVa4uEI721shjoQgFdYuoMst49X0NFf9MraQevweNVH+728FHh0c1hEz20A=="},"SellOffer":{"Type":1,"OfferId":1,"AccountIndex":2,"NftIndex":1500,"AssetId":1,"AssetAmount":10000,"ListedAt":1654656751000,"ExpiredAt":1654656791000,"TreasuryRate":200,"Sig":"cCh08P8RloU+uNZESVVbl5mqOFiiXR2JRJaAnmqxz6gCBXny2J9OUh5X7tRHaEBxDRRXQ1mQGMVMoe1/ncw3sQ=="},"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"CreatorAmount":null,"TreasuryAmount":null,"Nonce":1,"ExpiredAt":1654656781000,"Sig":"IvNyf5kQDJugu6gxlJi1pBhLsdhm6bU5lmwipOm4bKgAjbLEYpmf3PxFpMvKLI1a1H1syHzrBFy0QD3K4CMJ4g=="}'
```



### signCancelOffer
#### Description
sign cancelOffer transaction

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type CancelOfferSegmentFormat struct {
    AccountIndex      int64  `json:"account_index"`
    OfferId           int64  `json:"offer_id"`
	// maxOfferId +1 for AccountIndex 
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    ExpiredAt         int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 1 week
    Nonce             int64  `json:"nonce"`
	// transaction amount +1 for AccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome
> globalThis.signCancelOffer("seed phrase", '{"account_index":0,"offer_id":1,"gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"AccountIndex":0,"OfferId":1,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"cqwa4w8tjlvB4WKqs0Di/p97reaoH4TVh+h0TtRP8QsE1r+/QQOdptBUd2A9uVp8FATciJNxP4EpYouM2daPSg=="}'
```


### signCreateCollection
#### Description
sign create collection transaction

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type CreateCollectionSegmentFormat struct {
    AccountIndex      int64  `json:"account_index"`
    CollectionId      int64  `json:"collection_id"`
    Name              string `json:"name"`
    Introduction      string `json:"introduction"`
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    ExpiredAt         int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 1 week
    Nonce             int64  `json:"nonce"`
	// transaction amount +1 for AccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome   
> globalThis.signCreateCollection("seed phrase", '{"account_index":0,"collection_id":1,"name":"crypto punk","introduction":"crypto punk is the king of jpeg nft","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"AccountIndex":0,"CollectionId":1,"Name":"crypto punk","Introduction":"crypto punk is the king of jpeg nft","GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"cqwa4w8tjlvB4WKqs0Di/p97reaoH4TVh+h0TtRP8QsE1r+/QQOdptBUd2A9uVp8FATciJNxP4EpYouM2daPSg=="}'
```


### signMintNft
#### Description
sign mint nft transaction

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type MintNftSegmentFormat struct {
    CreatorAccountIndex int64  `json:"creator_account_index"`
	// CreatorAccountIndex can be found by quering api. 
    ToAccountIndex      int64  `json:"to_account_index"`
    ToAccountNameHash   string `json:"to_account_name_hash"`
    NftContentHash      string `json:"nft_content_hash"`
	// NftContentHash can be found by quering api. 
    NftCollectionId     int64  `json:"nft_collection_id"`
	// NftCollectionId can be found by quering api. 
    CreatorTreasuryRate int64  `json:"creator_treasury_rate"`
	// treasury fee rate, it can be found by quering api. 1 is corresponding to one ten thousandth. 
	// eg. 30 => 0.3% ; 2000 => 20%
    GasAccountIndex     int64  `json:"gas_account_index"`
    GasFeeAssetId       int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount   string `json:"gas_fee_asset_amount"`
    ExpiredAt           int64  `json:"expired_at"`
	// transaction expire time in milli-second type  
	// eg. current timestamp + 1 week
    Nonce               int64  `json:"nonce"`
	// transaction amount +1 for AccountIndex 
}
```

#### Example
```bash
# Call F12 in chrome   
> globalThis.signMintNft("seed phrase", '{"creator_account_index":15,"to_account_index":1,"to_account_name_hash":"ddc6171f9fe33153d95c8394c9135c277eb645401b85eb499393a2aefe6422a6","nft_content_hash":"7eb645401b85eb499393a2aefe6422a6ddc6171f9fe33153d95c8394c9135c27","nft_collection_id":65,"creator_treasury_rate":30,"gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"CreatorAccountIndex":15,"ToAccountIndex":1,"ToAccountNameHash":"ddc6171f9fe33153d95c8394c9135c277eb645401b85eb499393a2aefe6422a6","NftIndex":0,"NftContentHash":"7eb645401b85eb499393a2aefe6422a6ddc6171f9fe33153d95c8394c9135c27","NftCollectionId":65,"CreatorTreasuryRate":30,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"tbFr0GpwghhEqsW62a1sJ2Dn5gxPK1b3n+xDFrtvIpcEAhWH9SXu3nAr70k+l6BirwDjU83qXodgowXqCLXlWg=="}'
```

### signTransferNft
#### Description
sign transfer nft transaction

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type TransferNftSegmentFormat struct {
    FromAccountIndex  int64  `json:"from_account_index"`
    ToAccountIndex    int64  `json:"to_account_index"`
    ToAccountNameHash string `json:"to_account_name"`
    NftIndex          int64  `json:"nft_index"`
    GasAccountIndex   int64  `json:"gas_account_index"`
    GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
    GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
    CallData          string `json:"call_data"`
    ExpiredAt         int64  `json:"expired_at"`
    Nonce             int64  `json:"nonce"`
}
```

#### Example
```bash
# Call F12 in chrome   
> globalThis.signMintNft("seed phrase", '{"from_account_index":0,"to_account_index":1,"to_account_name":"ddc6171f9fe33153d95c8394c9135c277eb645401b85eb499393a2aefe6422a6","nft_index":15,"gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","call_data":"","expired_at":1654656781000,"nonce":1}')
# '{"CreatorAccountIndex":0,"ToAccountIndex":1,"ToAccountNameHash":"","NftIndex":0,"NftContentHash":"","NftCollectionId":0,"CreatorTreasuryRate":0,"GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"tHDjAyffLKgKbr83YfqhdSzhe8qmBi3jAJzBfZAqiwkAf6oicA1c7enenGtc9lW8ejjG5pqQv8MyiYOVaiFvMw=="}'
```

### signWithdrawNft
#### Description
sign withdraw nft transaction

#### Params
| Name | Type   | Comment                                                    |
| ---- | ------ | ---------------------------------------------------------- |
| seed | string | seed phrase, 256bits, type is BigInt, base 16 |
| segmentstr | string | formatted segment string |

- segment format struct
```go
type WithdrawNftSegmentFormat struct {
	AccountIndex      int64  `json:"account_index"`
	NftIndex          int64  `json:"nft_index"`
	ToAddress         string `json:"to_address"`
	// L1 address based on ecdsa. 
	// eg. 0x507Bd54B4232561BC0Ca106F7b029d064fD6bE4c
	GasAccountIndex   int64  `json:"gas_account_index"`
	GasFeeAssetId     int64  `json:"gas_fee_asset_id"`
	GasFeeAssetAmount string `json:"gas_fee_asset_amount"`
	ExpiredAt         int64  `json:"expired_at"`
	Nonce             int64  `json:"nonce"`
}
```

#### Example
```bash
# Call F12 in chrome   
> globalThis.signWithdrawNft("seed phrase", '{"account_index":1,"nft_index":15,"to_address":"0x507Bd54B4232561BC0Ca106F7b029d064fD6bE4c","gas_account_index":1,"gas_fee_asset_id":3,"gas_fee_asset_amount":"3","expired_at":1654656781000,"nonce":1}')
# '{"AccountIndex":1,"CreatorAccountIndex":0,"CreatorAccountNameHash":null,"CreatorTreasuryRate":0,"NftIndex":15,"NftContentHash":null,"NftL1Address":"","NftL1TokenId":null,"CollectionId":0,"ToAddress":"0x507Bd54B4232561BC0Ca106F7b029d064fD6bE4c","GasAccountIndex":1,"GasFeeAssetId":3,"GasFeeAssetAmount":3,"ExpiredAt":1654656781000,"Nonce":1,"Sig":"tN/62k2rtjMgo6/tnx2ILq3kXD/Jzv3bxWN/f7r4mxACT1mknNhMUTNQuP4ulYJEgab+xe4Vy283Dv6DzcTXjQ=="}'
```

### Error Types