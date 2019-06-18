# Quorum Block Explorer

An Ethereum and Quorum private blockchain explorer

## Quick start

### Download and install dependencies

`git clone https://github.com/SkyTradeInc/quorumblockexporer.git`

`cd quorumblockexporer`

`npm i`

`touch .env`

Edit the `.env` file and add the following

```
SERVER_PORT=
MONGO_USERNAME=
MONGO_PASSWORD=
MONGO_HOST=
MONGO_DB=
WEB3_HTTP=
WEB3_WS=
SYNC_REQUESTS=
API_LIMIT_BLOCKS=
API_LIMIT_TRANSACTIONS=
```

Run `node daemon` to start

### First run

![Syncing](docs/images/sync.png?raw=true "Block syncing ")


### Restart Sync

To reset block data start with flag '--resync'

`node daemon --resync`

# General API Information

* The default base endpoint is: http://localhost:2000 for REST API and socket streams unless specified SERVER_PORT in the `.env` file
* All endpoints return either a JSON object or array.
* Data is returned in **descending** order. Newest first, oldest last.
* All time and timestamp related fields are in milliseconds.
* HTPP `4XX` return codes are used for for malformed requests; the issue is on the sender's side. issue
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `5XX` return codes are used for internal errors; the issue is on server's side.
* HTTP`200` return code signals response has been sent back to the client
* Any endpoint can return an ERROR; the error payload is as follows:

```
  {
    success: false,
    timestamp: 1560818595133,
    data: "error message"
  }
```

# REST API endpoints

### GET /ping

**Parameters:** None

**Example:** http://localhost:2000/ping

**Response:**
```javascript
{
    success: true,
    timestamp: 1560810605811,
    data: "pong"
}
```

### GET /api/limits

**Parameters:** None

**Example:** http://localhost:2000/api/limits

**Response:**
```javascript
{
  success: true,
  timestamp: 1560869747818,
  data: {
    paths: [
      {
      path: "/api/latestTransactions/:limit",
      limit: 100,
      default: 1
      },
      {
      path: "/api/latestBlocks/:limit",
      limit: 10,
      default: 1
      }
    ]
  }
}

```

###  GET/api/latestBlock

**Parameters:** None

**Example:** http://localhost:2000/api/latestBlock

**Response:**
```javascript
{
    success: true,
    timestamp: 1560810632182,
    data: {
        transactions: [ ],
        uncles: [ ],
        validators: [
        "0xc1836760988668eea5275cd562755ec0b82cef92",
        "0xcb680d316b281aebb68e73e813b7a53ec93c8f05",
        "0x86dc44a5ae35a09a62585f2e3de29f14dce2ff2c"
        ],
        _id: "5d01e52c96124674c2cf164c",
        difficulty: 1,
        extraData: "0xd98301080c846765746889676f312e31312e3130856c696e7578000000000000f90164f854941a67eea756b9c074219dbbd1a68b7a69194126459486dc44a5ae35a09a62585f2e3de29f14dce2ff2c94c1836760988668eea5275cd562755ec0b82cef9294cb680d316b281aebb68e73e813b7a53ec93c8f05b84122b41d47c00dfdf57092f2269c1dd773e18aa3c84c4a529f69fbbea1defdb6b12b872291dd835fe52ddd8bc7deccd9ba7c0911e47236b58b0374f0a05635374701f8c9b84127ebee4aaa2c01d78a6cc049db3e2723ee56023a8116ac16009346822eddfdff0b8103009a20956f1f87d99aa54c6af7d3a6621d41ae1163e2f37b55845c658400b841e0a5c5abdd4b5e06b6e89e87a8b143eea0d98c52bd5134550e00c1cb87ea742e5125cc9b2c6bc55d13576d22ddc22dca230941e0d52e56ee63e0d7c08fb3778900b8413436a96424b61e692854492040fdd60ca404470708f5e4995e02d735efa4c89628dd84abdaf28f17d5fbb768e6ec6f0bd6a4ae598354e07932d09658c1672d3801",
        gasLimit: 9007199254740000,
        gasUsed: 0,
        hash: "0x4c3d2ba8468890cc4221d592b4c1d010190847d0e2cdfb2635685d0d82267ff6",
        logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        miner: "0xc1836760988668eeA5275CD562755eC0B82CeF92",
        mixHash: "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
        nonce: "0x0000000000000000",
        number: 14658,
        parentHash: "0x075e8064e7ae554afe4be6069f352dff9dd1c676c5035e46276ef8378ea350f7",
        receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        size: 907,
        stateRoot: "0xf6a5ead70c9644249ee338546038c604887cf97b69769c67ac57582eb5885511",
        timestamp: 1560405289,
        totalDifficulty: 14659,
        transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        __v: 0
    }
}
```

### GET /api/latestBlocks/:limit

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
:limit | INT | NO | Default 1; max defined by `.env`

**Example:** http://localhost:2000/api/latestBlocks/1

**Response:**
```javascript
{
    success: true,
    timestamp: 1560817599208,
    data: [
        {
        transactions: [ ],
        uncles: [ ],
        validators: [
        "0xc1836760988668eea5275cd562755ec0b82cef92",
        "0xcb680d316b281aebb68e73e813b7a53ec93c8f05",
        "0x86dc44a5ae35a09a62585f2e3de29f14dce2ff2c"
        ],
        _id: "5d01e52c96124674c2cf164c",
        difficulty: 1,
        extraData: "0xd98301080c846765746889676f312e31312e3130856c696e7578000000000000f90164f854941a67eea756b9c074219dbbd1a68b7a69194126459486dc44a5ae35a09a62585f2e3de29f14dce2ff2c94c1836760988668eea5275cd562755ec0b82cef9294cb680d316b281aebb68e73e813b7a53ec93c8f05b84122b41d47c00dfdf57092f2269c1dd773e18aa3c84c4a529f69fbbea1defdb6b12b872291dd835fe52ddd8bc7deccd9ba7c0911e47236b58b0374f0a05635374701f8c9b84127ebee4aaa2c01d78a6cc049db3e2723ee56023a8116ac16009346822eddfdff0b8103009a20956f1f87d99aa54c6af7d3a6621d41ae1163e2f37b55845c658400b841e0a5c5abdd4b5e06b6e89e87a8b143eea0d98c52bd5134550e00c1cb87ea742e5125cc9b2c6bc55d13576d22ddc22dca230941e0d52e56ee63e0d7c08fb3778900b8413436a96424b61e692854492040fdd60ca404470708f5e4995e02d735efa4c89628dd84abdaf28f17d5fbb768e6ec6f0bd6a4ae598354e07932d09658c1672d3801",
        gasLimit: 9007199254740000,
        gasUsed: 0,
        hash: "0x4c3d2ba8468890cc4221d592b4c1d010190847d0e2cdfb2635685d0d82267ff6",
        logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        miner: "0xc1836760988668eeA5275CD562755eC0B82CeF92",
        mixHash: "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
        nonce: "0x0000000000000000",
        number: 14658,
        parentHash: "0x075e8064e7ae554afe4be6069f352dff9dd1c676c5035e46276ef8378ea350f7",
        receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        size: 907,
        stateRoot: "0xf6a5ead70c9644249ee338546038c604887cf97b69769c67ac57582eb5885511",
        timestamp: 1560405289,
        totalDifficulty: 14659,
        transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        __v: 0
        }
        ]
}

```

### GET /api/latestTransactions/:limit

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
limit | INT | NO | Default 1; max defined by `.env`

**Example:** http://localhost:2000/api/latestTransactions/1


**Response:**
```javascript
{
    success: true,
    timestamp: 1560817650693,
    data: [
        {
        _id: "5d01eb1f77e61f7927861835",
        blockHash: "0x2b64deccebae2a7a2abca056114797516e08018f410e7aeac0a60d83649d1ac0",
        blockNumber: 14599,
        from: "0x1a67eeA756B9c074219dBBd1a68b7A6919412645",
        gas: 4700000,
        gasPrice: 2000000000,
        hash: "0x85a16d46df13f5e3576ecc187e08aaa65a28ce2882cdb996a0342f14b7f0075b",
        input: "0x186047b000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000016000000000000000000000000000000000000000000000000000000000000001a000000000000000000000000000000000000000000000000000000000000001e000000000000000000000000000000000000000000000000000000000000002200000000000000000000000000000000000000000000000000000000000000280000000000000000000000000000000000000000000000000000000000000001876616c696461746f722d6465766e65746164646f6e3031300000000000000000000000000000000000000000000000000000000000000000000000000000001876616c696461746f722d6465766e65746164646f6e30313000000000000000000000000000000000000000000000000000000000000000000000000000000008506565724e6f6465000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000831302e382e382e3100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000053537313030000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002a3078306562306538666462383531633637656434623433623561316530663166633531333232323739300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000806237376135323236343963353866346237323137313436333135346534303335663430653839386632656362326539313033383732323536313365613132666664343664393433363563326637623461386538353638663666636631343633373535356637613664636164646662333766373065613366303537383939303431",
        nonce: 20,
        to: "0x0000000000000000000000000000000000002023",
        transactionIndex: 0,
        value: 0,
        v: "0x1c",
        r: "0xae01020d98961868315f308949a652a8cb060e28b19a4457409435366718883b",
        s: "0x118b1b2996d0782231e05e5dba113d04c2488118c4221e9dc839d23082d18028",
        __v: 0
      }
    ]
}
```

### GET /api/address/:address

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
address | STRING | YES | Look up an address

**Example:** http://localhost:2000/api/address/0xF232A4BF183cF17D09Bea23e19CEfF58Ad9dbFED

**Response:**
```javascript
{
    success: true,
    timestamp: 1560866329317,
    data: {
        type: 0,
        _id: "5d08dea55821af200e156ef6",
        address: "0xF232A4BF183cF17D09Bea23e19CEfF58Ad9dbFED",
        balance: 2000000000000000000,
        blockNumber: 30738,
        __v: 0
    }
}
```

### GET /api/tx/:hash

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
:hash | STRING | YES | Look up a transaction hash


**Example:** http://localhost:2000/api/tx/0x85a16d46df13f5e3576ecc187e08aaa65a28ce2882cdb996a0342f14b7f0075b

**Response:**
```javascript
{
  success: true,
  timestamp: 1560817697349,
  data: {
      _id: "5d01eb1f77e61f7927861835",
      blockHash: "0x2b64deccebae2a7a2abca056114797516e08018f410e7aeac0a60d83649d1ac0",
      blockNumber: 14599,
      from: "0x1a67eeA756B9c074219dBBd1a68b7A6919412645",
      gas: 4700000,
      gasPrice: 2000000000,
      hash: "0x85a16d46df13f5e3576ecc187e08aaa65a28ce2882cdb996a0342f14b7f0075b",
      input: "0x186047b000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000016000000000000000000000000000000000000000000000000000000000000001a000000000000000000000000000000000000000000000000000000000000001e000000000000000000000000000000000000000000000000000000000000002200000000000000000000000000000000000000000000000000000000000000280000000000000000000000000000000000000000000000000000000000000001876616c696461746f722d6465766e65746164646f6e3031300000000000000000000000000000000000000000000000000000000000000000000000000000001876616c696461746f722d6465766e65746164646f6e30313000000000000000000000000000000000000000000000000000000000000000000000000000000008506565724e6f6465000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000831302e382e382e3100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000053537313030000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002a3078306562306538666462383531633637656434623433623561316530663166633531333232323739300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000806237376135323236343963353866346237323137313436333135346534303335663430653839386632656362326539313033383732323536313365613132666664343664393433363563326637623461386538353638663666636631343633373535356637613664636164646662333766373065613366303537383939303431",
      nonce: 20,
      to: "0x0000000000000000000000000000000000002023",
      transactionIndex: 0,
      value: 0,
      v: "0x1c",
      r: "0xae01020d98961868315f308949a652a8cb060e28b19a4457409435366718883b",
      s: "0x118b1b2996d0782231e05e5dba113d04c2488118c4221e9dc839d23082d18028",
      __v: 0
  }
}
```

### GET /api/block/:number

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
:number | STRING | YES | Look up a block by number

**Example:** http://localhost:2000/api/block/1

```javascript
{
    success: true,
    timestamp: 1560839721138,
    data: {
        transactions: [ ],
        uncles: [ ],
        validators: [
        "0xcb680d316b281aebb68e73e813b7a53ec93c8f05",
        "0x86dc44a5ae35a09a62585f2e3de29f14dce2ff2c",
        "0x1a67eea756b9c074219dbbd1a68b7a6919412645"
        ],
        _id: "5d01cf55fad35e74a7ed31ed",
        difficulty: 1,
        extraData: "0xd98301080c846765746889676f312e31312e3130856c696e7578000000000000f90164f854941a67eea756b9c074219dbbd1a68b7a69194126459486dc44a5ae35a09a62585f2e3de29f14dce2ff2c94c1836760988668eea5275cd562755ec0b82cef9294cb680d316b281aebb68e73e813b7a53ec93c8f05b841a5ee0c1f1d2fcbb07d92952d7ad06dd7f47bffd155f30493f9ceae7a867889a51f58c0203f9d270189b22d838668fc5e38d1d079f9ff0e0161a615e9f2f887e200f8c9b841b2773ee92b54b4dfd5f77a88f7fe7e9c884ee1cf56521218705aebadc2b8647f4478a994a945943f0710d2fb05cfbef4bf458d21bb04d173d590a7acba81f62901b84189c98d79dcd7fbeee9545c9541fe5bd9c904ccf6798c9630d5c9e76eaf8c1c6332974e187822a26d780e1bbf12f66c7c092f1f1444178b472a306cee0044090e00b84100b62171a64bd66e0ee26d4a761ba320185e5aca2fd376065af7cbc822462d8f3a85cc3d3c970ee0b833b01419087bf2f224a4d66d848c1a52dcc5760fd0ea5801",
        gasLimit: 9007199254740000,
        gasUsed: 0,
        hash: "0x0d344db719b03f269623356fc47fb91c9b79e58a30f749c08334ea734283900b",
        logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        miner: "0x86DC44A5Ae35A09a62585F2e3dE29F14DcE2fF2C",
        mixHash: "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
        nonce: "0x0000000000000000",
        number: 1,
        parentHash: "0xea26600e5c9c7568bdb0980741eff06499f37b741d58e900e6dfbdba49a5aa34",
        receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        size: 905,
        stateRoot: "0xba81e143c7cba1ee061c36bc562bb8a1dc3abfce698aba0ef9a849b022e12c2f",
        timestamp: 1560331999,
        totalDifficulty: 2,
        transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        __v: 0
    }
}

```
### GET /api/balance/:address

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
address | STRING | YES | Get balance for an address in wei


**Example:** http://localhost:2000/api/balance/0x0000000000000000000000000000000000002023

**Response:**
```javascript
{
    success: true,
    timestamp: 1560839800641,
    data: "1048575"
}
```


### GET /api/peers

**Parameters:** None

**Example:** http://localhost:2000/api/peers

**Response:**
```javascript
{
    success: true,
    timestamp: 1560817433799,
    data: 9
}
```

### GET /api/nodes

**Parameters:** None

**Example:** http://localhost:2000/api/nodes

**Response:**
```javascript
{
    success: true,
    timestamp: 1560817457031,
    data: [
        {
        id: "2ff2174822a779671b2873cbb390ad069d2683c94c565b73dd578fd7e13920b487ba4a7aefa802724edf324a4ba20ab0114f0a6c1c5d9c6b4abf76bc022000ef",
        name: "Geth/validator-cn-hk-app010/v1.8.12-stable-62a3b6c1(quorum-v2.2.1)/linux-amd64/go1.11.5",
        caps: [
          "istanbul/64"
        ],
        network: {
          localAddress: "xxx.xxx.xxx.xxx:xxxxx",
          remoteAddress: "xxx.xxx.xxx.xxx:xxxxx",
          inbound: true,
          trusted: false,
          static: false
        },
        protocols: {
            istanbul: {
            version: 64,
            difficulty: 1,
            head: "0xff950b5dc6d5309153d7f5a38055117be753ee54d30a8b5c3b22f66f663c42b8"
          }
      }
    }]
}
```

# Socket Streams

## Detailed Stream information

### .on('newBlockHeaders')

Emits incoming block headers. This can be used as timer to check for changes on the blockchain.

**Response:**
```javascript
{
    transactions: [ ],
    uncles: [ ],
    validators: [
    "0xc1836760988668eea5275cd562755ec0b82cef92",
    "0xcb680d316b281aebb68e73e813b7a53ec93c8f05",
    "0x86dc44a5ae35a09a62585f2e3de29f14dce2ff2c"
    ],
    _id: "5d01e52c96124674c2cf164c",
    difficulty: 1,
    extraData: "0xd98301080c846765746889676f312e31312e3130856c696e7578000000000000f90164f854941a67eea756b9c074219dbbd1a68b7a69194126459486dc44a5ae35a09a62585f2e3de29f14dce2ff2c94c1836760988668eea5275cd562755ec0b82cef9294cb680d316b281aebb68e73e813b7a53ec93c8f05b84122b41d47c00dfdf57092f2269c1dd773e18aa3c84c4a529f69fbbea1defdb6b12b872291dd835fe52ddd8bc7deccd9ba7c0911e47236b58b0374f0a05635374701f8c9b84127ebee4aaa2c01d78a6cc049db3e2723ee56023a8116ac16009346822eddfdff0b8103009a20956f1f87d99aa54c6af7d3a6621d41ae1163e2f37b55845c658400b841e0a5c5abdd4b5e06b6e89e87a8b143eea0d98c52bd5134550e00c1cb87ea742e5125cc9b2c6bc55d13576d22ddc22dca230941e0d52e56ee63e0d7c08fb3778900b8413436a96424b61e692854492040fdd60ca404470708f5e4995e02d735efa4c89628dd84abdaf28f17d5fbb768e6ec6f0bd6a4ae598354e07932d09658c1672d3801",
    gasLimit: 9007199254740000,
    gasUsed: 0,
    hash: "0x4c3d2ba8468890cc4221d592b4c1d010190847d0e2cdfb2635685d0d82267ff6",
    logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    miner: "0xc1836760988668eeA5275CD562755eC0B82CeF92",
    mixHash: "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
    nonce: "0x0000000000000000",
    number: 14658,
    parentHash: "0x075e8064e7ae554afe4be6069f352dff9dd1c676c5035e46276ef8378ea350f7",
    receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    size: 907,
    stateRoot: "0xf6a5ead70c9644249ee338546038c604887cf97b69769c67ac57582eb5885511",
    timestamp: 1560405289,
    totalDifficulty: 14659,
    transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    __v: 0
}
```

## .on('pendingTransaction')

Emits incoming pending transactions.


**Response:**
```javascript
{
    _id: "5d08f6e65821af200e16cf0f",
    blockHash: "0x2e9b5d1c200fde10e6d705e13c82c72f18dfd95f9c12f360c3a73ba6ba5b5400",
    blockNumber: 120850,
    from: "0xF232A4BF183cF17D09Bea23e19CEfF58Ad9dbFED",
    gas: 4700000,
    gasPrice: 1000000000,
    hash: "0x2421b91d8722bc2fca95d79e2de1f4124a424ae6cfaa116c8b9d380dbdfc69b6",
    input: "0x",
    nonce: 59,
    to: "0xef1A770489b340B308EF58087affa01F1863CeB4",
    transactionIndex: 0,
    value: 1e+23,
    v: "0x1b",
    r: "0xe3b688bd3c8f7dd71cbce692a4474f23e339a74abab6e4b17173965b9a41ef5b",
    s: "0x3ed288b8c77053091f7df92b57c33cd2f0eff521a999e917b8f460f88db07efa",
    __v: 0
},
```
