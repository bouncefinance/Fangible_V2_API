## Fangible V2 API

> Editor: homie_xu						
>
> Email: homie_xu@163.com ( If you have any interface problem, please contact me )
>
> Update main content: ( 2021-5-23 )
>
> - Rewriting the paging interface for the Marketplace list and the brands list can effectively solve the problem of too much data being loaded slowly in a single request
> - The back-end data is optimized and integrated, and the key data can be obtained through an interface, without the need to do too much data integration work in the front end

### One、 Marketplace List

##### 1.1 Support network

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |



##### 1.2 Interface details

```jsx
# /api/v2/main/getautionpoolsbypage

params = {
	"category": "",
	"channel": "FineArts",
	"currency": "0x55d398326f99059ff775485246999027b3197955",
	"limit": 100,
	"offset": 0,
	"orderfield": 1
}

result = {
    code: 1,
    data: [{
        	"poolid": 282,
			"tokenid": 17428,
			"itemname": "Treecko5",
			"fileurl": "https://ap1-cfs3-media-bounce.bounce.finance/609ef966a8e9cfb4a6680b7932f08b42-1619772236.png",
			"token0": "0x479FCe86f116665b8a4d07165a0eB7799A4AEb30",
			"token1": "0x0000000000000000000000000000000000000000",
			"likecount": 0,
			"price": "100000000000000",
			"state": 0,
			"creatorurl": "https://ap1-cfs3-media-bounce.bounce.finance/54f56b9f06c042c9ea75095e3554d266-1618457831.png",
			"username": "hustler",
			"created_at": "2021-05-23T04:08:43Z"
		}]
}
```

**Params introduce**

- **category**: The category value passed in when creating the NFT

  | Enumeration | Field meaning                            |
  | ----------- | ---------------------------------------- |
  | " "         | Represents a query for all categories    |
  | Image       | Query only the pool list in image format |
  | Video       | Query only the pool list in video format |

- **channel**: The channel value passed in when creating the NFT

  | Enumeration | Field meaning                                                |
  | ----------- | ------------------------------------------------------------ |
  | " "         | Query pool information for all channels                      |
  | FineArts    | Only the pool information for which the NFT channel  is FineArts is queried |
  | Sports      | Only the pool information for which the NFT channel  is Sports is queried |
  | Conicbooks  | Only the pool information for which the NFT channel is Conicbooks is queried |

- **currency**: The address of the contract for the payment token
- **limit**: How many pieces of data are displayed on a single page?
- **offset**: Query the number of pages of data，The index starts at 0
- **orderfield**: sortord, Enum-type( 1:  New  create time 2:  Popular weight)



### Two、 Brand List

##### 2.1 Support network

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |

##### 2.2 Interface details

```jsx
# /api/v2/main/getbrandsbypage

params = {
	"limit": 10,
	"offset": 0,
	"orderfield": 1
}

result = {
	"code": 1,
	"data": [
		{
			"id": 116, // 
			"creator": "0x8fc625474765e10d16e0fc4295388b5af5d1ec5a",
			"ownername": "",
			"owneraddress": "0x8fc625474765e10d16e0fc4295388b5af5d1ec5a",
			"contractaddress": "0x75a4c2eb0bfc3c5e52deeaa2c09b6fd3cc6e35f9",
			"brandname": "test1-721-Brand",
			"brandsymbol": "",
			"description": "test1-721-Brand",
			"imgurl": "https://market-test.bounce.finance/jpgfileget/a00f10f5843172738e74451b267fe907-1617850166.jpg",
			"standard": 1,
			"status": 0,
			"popularweight": 133,
			"bandimgurl": "",
			"ownerimg": "",
			"totalcount": 0
		}]
}
```

**Params introduce**

- **limit**: How many pieces of data are displayed on a single page?
- **offset**: Query the number of pages of data，The index starts at 0
- **orderfield**: sortord, Enum-type ( 1:  New  create time 2:  Popular weight)



