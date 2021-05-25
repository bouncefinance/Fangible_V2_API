# Fangible V2 API

> Editor: homie_xu						
>
> Email: homie_xu@163.com ( If you have any interface problem, please contact me )
>
> Update main content: ( 2021-5-25 )
>
> 1. added 2.2 2.3 Three interfaces on the pool details page
> 2. added 2.5 Get NFT details function
> 3. three new Typing

## Function roadmap

![image-20210524201947290](http://oss.yitian2019.cn/img/image-20210524201947290.png)



---

## Function page

> Please refer to Function roadmap for understanding of the function page classification given here

### 一、Home

#### 1.1 Fast moves

```jsx
# /api/v2/main/getfastmoves

params = {
	"limit": 8,				// Amount of each page
}

result = {
    code: 1, 				// 1: success 0: error
    data: IPoolData[]		// import { IPoolData } from Typing
}
```

#### 1.2 Hot Brands

Use `# 3.1 **Brand List**` endpoint

```jsx
# /api/v2/main/getbrandsbypage

params = {
	"limit": 4,						// Amount of each page
	"offset": 0,	
	"orderfield": 2					// Sort according to the background setting weight
}

result = {
	"code": 1,						// 1: success	2: error
	"data": IBrandData[]			// import { IBrandData } from Typing
}
```

#### 1.3 Banner Brands

testing...

------

### 二、Marketplace

#### 2.1  Marketplace List

**Support network**

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |



**Interface details**

```jsx
# /api/v2/main/getauctionpoolsbypage

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
    data: IPoolData[]		// import { IPoolData } from Typing
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

#### 2.2 Pool Detail (Pool Info)

```jsx
# /api/v2/main/getpoolinfo

params = {
	"poolid": 3,
	"pooltype": 1 			// 1:fixedpool 2:EnglishAuction,
}

result = {
    code: 1,
    data: IPoolData		// import { IPoolData } from Typing
}
```

#### 2.3 Pool Detail (Offers List)

```jsx
# /api/v2/main/getpoolbids

params = {
	"poolid": 3,
	"pooltype": 1 			// 1:fixedpool 2:EnglishAuction
}

result = {
    code: 1,
    data: IOfferData_EnglishAuction | IOfferData_FixedSwap		
    // import { IOfferData_EnglishAuction, IOfferData_FixedSwap } from Typing
}
```

#### 2.4 Pool Detail (Trading History)

testing...

```jsx
# /api/v2/main/getpoolactivities

params = {
	"poolid": 3,
	"pooltype": 1 			// 1:fixedpool 2:EnglishAuction
}

result = {
    code: 1,
    data: IHistoryData		// import { IHistoryData } from Typing
}
```

#### 2.5 Get NFT Info

```jsx
# /api/v2/main/getonenftinfo

params = {
	"contractaddress": "0xaAAeAe4283635358946E653883cD12E5c06cC5E3",
	"tokenid": 16791
}

result = {
    code: 1,
    data: INftData		// import { INftData } from Typing
}
```



--------

### 三、Brand & My Brand

#### 3.1  Brand List

**Support network**

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |



**Interface details**

```jsx
# /api/v2/main/getbrandsbypage

params = {
	"limit": 10,					// Amount of each page
	"offset": 0,					
	"orderfield": 1					// 1: Sort by latest creation	2：Sort by Popular weight
}

import { IBrandData } from Typing
result = {
	"code": 1,						// 1: success	2: error
	"data": IBrandData[]			
}
```

#### 3.2 Create Brand

> Modified the previous creation process. Previously, after the contract was successfully called, the sub-contract address was spliced with the brand information and stored in the backend. After the contract is upgraded, there is no guarantee that the address of the newly created sub-contract can be obtained in real time, but the TxHash can be easily obtained, so it is only necessary to store the transaction hash after the call is successful.

1. Get TxHash

```jsx
const newTx = Contract.methods[ FunctionName ].send()

newTx.on('transactionHash', hash => {
    console.log('TxHash is ' + hash)
})
```

2. Submit Brand info

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |

```jsx
# /api/v2/main/getaccountbrands

params = {
  "brandname": "string",				// brand name
  "brandsymbol": "string",		
  "contractaddress": "string",			// set null 
  "description": "string",
  "imgurl": "string",
  "owneraddress": "string",
  "ownername": "string",
  "standard": number,					// 1: ERC721 	2：ERC1155
  "status": number,
  "txid": "string"						// TxHash obtained by previous step
}

result = {
    code: 1,
    data: 'success message'
}
```

#### 3.3 My Brand List

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |

```jsx
# /api/v2/main/getbrandsbypage

header = {
    token: string				// auth sign token
}

params = {
  accountaddress: string		// user addr
}

result = {
    code: 1,
    data: 'success message'
}
```



-----

## Typing



### 1. IPoolData

```tsx
interface IPoolData {
    "poolid": number				// Pool ID
    "tokenid": number				// NFT TokenId
    "itemname": string				// NFT name
    "fileurl": string				// NFT metadata file link
    "token0": string				// NFT contract address
    "token1": string				// Contract address of erc20 token (currency)
    "likecount": number				// Amount of people who save this address
    "price": string					// Current price
    "state": number					// 0: Live		1: Close
    "creatorurl":string
    "username": string
    "created_at": string			// Pool creation time
}
```

### 2. IBrandData

```tsx
interface IBrandData {
    "id": number  					// Brand ID
	"creator": string 
    "ownername": string	 			// Username of the minter (artist)
    "owneraddress": string
    "contractaddress": string		// Sub-contract address
    "brandname": string				// Brand name
    "brandsymbol": string
    "description": string			// Brand description
    "imgurl": string				// Brand cover image link
    "standard": number				// 1: ERC721 	2：ERC1155
    "status": number				// 0: Live		1: Close
    "popularweight": number			// Pool weight
    "bandimgurl": string			// Brand background image link	
    "ownerimg": string				// The link of avatar image of minter (artist)
    "totalcount": number
}
```

- popularweight: Each time the pool is accessed, the weight will increase by 1, and the weight can be changed directly in the background

### 3.IOfferData

```tsx
interface IOfferData_EnglishAuction {
    "id": number,						
    "height": number,				// Block height
    "sender": string as address,	
    "pool_id": number,				// Pool ID
    "amount1": string,				// offer a price
    "ctime": number,				// timetamp
    "created_at": string
}

interface IOfferData_FixedSwap {
    "id": number,
    "height": number,
    "sender": string as address,
    "pool_id": number,
    "swapped_amount0": number,		// NFT number
    "swapped_amount1": string,		// Pay the total price
    "ctime": number,
    "created_at": string
}
```

### 4.IHistoryData

testing...

```tsx
interface IHistoryData {
    contract: string as address,
    event: string,					// Event name Enum: Created Swap
    from: string as address,
    id: number,
    price: string,
    quantity: number,
    timestamp: number,
    to: string as address,
    tokenId: number
}
```

### 5.INftData

```tsx
interface INftData {
   	"id": number,
    "contractaddress": string as address,
    "tokenid": number,
    "metadata": string,
    "category": string,					// Enum: image video
    "channel": string,					// Enum: FineArts Sports Conicbooks 
    "itemsymbol": string,
    "standard": number,
    "itemname": string,
    "externallink": string,
    "description": string,
    "fileurl": string,
    "litimgurl": string,
    "supply": number,
    "creator": string as address,
    "popularweight": number,
    "poolweight": number,
    "likecount": number,
    "artistpoolweight": number,
    "created_at": string as address,
    "updated_at": string
}
```

