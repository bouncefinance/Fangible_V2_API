# Fangible V2 API

> Editor: homie_xu						
>
> Email: homie_xu@163.com ( If you have any interface problem, please contact me )
>
> Update main content: ( 2021-5-24 )
>
> 1. 重新梳理了一下文档接口结构，新增 Typing
> 2. 新增 Fast Moves、Host Brands 的首页接口
> 3. 新增创建 brand 的接口，以及获取 my brand List 接口
> 4. 修正 `# /api/v2/main/getauctionpoolsbypage` 中的 `auction` 拼写错误

## Function roadmap

![image-20210524201947290](http://oss.yitian2019.cn/img/image-20210524201947290.png)



---

## 功能页面 

> 这里给出的功能页面分类请参照 Function roadmap 进行理解

### 一、Home

#### 1.1 Fast moves

```jsx
# /api/v2/main/getfastmoves

params = {
	"limit": 8,				// 单页显示数量
}

result = {
    code: 1, 				// 1: success 0: error
    data: IPoolData[]		// import { IPoolData } from Typing
}
```

#### 1.2 Hot Brands

使用 `# 3.1 Brand List` 接口

```jsx
# /api/v2/main/getbrandsbypage

params = {
	"limit": 4,						// 单页显示数量
	"offset": 0,	
	"orderfield": 2					// 根据后台设置权重排序
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
	"limit": 10,					// 单页显示数量
	"offset": 0,					
	"orderfield": 1					// 1: 按最新创建排序	2：按 Popular weight 排序
}

import { IBrandData } from Typing
result = {
	"code": 1,						// 1: success	2: error
	"data": IBrandData[]			
}
```

#### 3.2 Create Brand

> 修改了之前的创建流程，之前是调用合约成功后将子合约地址拼接上 brand 信息，存储至后端。由于合约进行升级后，无法保证实时取到刚创建的子合约地址，但是能很方便取到  TxHash , 所以这里只需要调用成功后存储交易哈希即可

1. Get TxHash

```jsx
const newTx = Contract.methods[ FunctionName ].send()

newTx.on('transactionHash', hash => {
    console.log('TxHash is ' + hash)
})
```

2. 上传 Brand 信息

| environment | BSC-STAGE                          | BSC-MAIN | HECO | ETH  |
| ----------- | ---------------------------------- | -------- | ---- | ---- |
| HOST        | https://market-test.bounce.finance |          |      |      |
| Support     | √                                  | ×        | ×    | ×    |

```jsx
# /api/v2/main/getaccountbrands

params = {
  "brandname": "string",				// brand name
  "brandsymbol": "string",		
  "contractaddress": "string",			// 这里可以不用传了
  "description": "string",
  "imgurl": "string",
  "owneraddress": "string",
  "ownername": "string",
  "standard": number,					// 1: ERC721 	2：ERC1155
  "status": number,
  "txid": "string"						// 上一步获取的 TxHash
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
  accountaddress: string		// 用户地址
}

result = {
    code: 1,
    data: 'success message'
}
```

### 

-----

## Typing



### 1. IPoolData

```tsx
interface IPoolData {
    "poolid": number				// Pool 的 ID
    "tokenid": number				// NFT TokenId
    "itemname": string				// NFT name
    "fileurl": string				// NFT 存储的媒体信息在线链接
    "token0": string				// NFT 合约地址
    "token1": string				// 支付货币的合约地址
    "likecount": number				// 共有多少人点击收藏本池子
    "price": string					// 当前价格
    "state": number					// 0: Live		1: Close
    "creatorurl":string
    "username": string
    "created_at": string			// 池子创建时间
}
```

### 2. IBrandData

```tsx
interface IBrandData {
    "id": number  					// Brand 的 ID
	"creator": string 
    "ownername": string	 			// 铸造者（艺术家）昵称
    "owneraddress": string
    "contractaddress": string		// 子合约地址
    "brandname": string				// Brand name
    "brandsymbol": string
    "description": string			// Brand 描述信息
    "imgurl": string				// Brand 主图在线链接
    "standard": number				// 1: ERC721 	2：ERC1155
    "status": number				// 0: Live		1: Close
    "popularweight": number			// 池子的权重信息
    "bandimgurl": string			// Brand 背景图在线链接	
    "ownerimg": string				// 铸造者（艺术家）头像
    "totalcount": number
}
```

- popularweight: 每次访问池子权重会增加1，后台可直接更改权重

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

