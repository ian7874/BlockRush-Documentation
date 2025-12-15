# BlockRush Documentation

BlockRush is a Solana transaction acceleration infrastructure designed for high-performance applications with ultra-low latency optimization.

## Core Features

Everything you need to accelerate your Solana transactions.

## Price & Rate Limits
| Plan             | Entry     | Intermediate     | Advanced       |
|------------------|-----------|------------------|---------------|
| Price            | FREE!     | FREE!            | FREE!         |
| TPS              | 5 TPS     | 50 TPS           | 100 TPS       |
| SWQoS            | Dedicated SWQoS | Dedicated SWQoS  | Dedicated SWQoS |
| Transaction Tip  | 0.001 SOL | 0.001 SOL        | 0.0001 SOL    |


## Getting Started
Most of our service endpoint APIs require an API key for authentication.
To obtain an API key, please contact us directly and we’ll set you up with the appropriate access credentials.


## Authorization

* Protocol：HTTP/HTTPS
* Response Format：JSON (the health endpoint returns plain text “ok”)
* Timezone: UTC
* Header：
```
Content-Type: application/json
x-api-key: <YOUR_API_KEY>
```

* The health endpoint does not require x-api-key header, all other write operation endpoints require it.


## API Overview
| Endpoint         | Description                                        | Method  |
|------------------|-------------------------------------------|-------|
| health           | Check the health status of the current API endpoint.                   | GET   |
| sendTransaction  | Native Solana JSON-RPC sendTransaction method   | POST  |
| submitBatch      | Non-atomic batch submission of multiple signed transactions (each transaction processed independently) | POST  |
| sendBundle       | Atomic bundle submission of multiple signed transactions (all fail if any transaction fails) | POST  |

1. health
* Description：Check the health status of the current API endpoint.
* Method：GET/POST
* Path：/health

Parameters
N/A

Response

| Field   | Type   | Description                    |
|--------|--------|------------------------|
| result | string | Fixed as “OK” to indicate the endpoint is available |

Example：
* Request GET http://ny1.rpc.blockrush.io/health
* Response 200 OK

2. sendTransaction
* Description：Proxy Solana’s sendTransaction JSON-RPC, directly forwarding a single signed transaction to Staked Validator for processing.
* Method：POST
* Path：/
* Header：Content-Type: application/json
  
Parameters

| Field   | Type   | Description                    |         Example      |
|-----------|---------|------------------------------|-----------|
| params    | string       | Fully signed transaction, Base64 encoded string |   See below      |
| encoding  | string       | Encoding configuration object  | base64    |

Response

| Field   | Type   | Description                  |
|--------|--------|------------------------|
| result | string | Transaction signature. The result format matches the [Solana RPC documentation](https://solana.com/docs/rpc/http/sendtransaction) |


Example Request：
```
curl -X POST 'https:// ny.rpc.blockrush.io ' \
-H 'Content-Type: application/json' \
-H 'x-api-key: <YOUR_API_KEY>' \
-d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sendTransaction",
  "params": [
    "<base64_encoded_tx>",
    { "encoding": "base64" }
  ]
}'
```
Example Response：
```
{
  "jsonrpc": "2.0",
  "result": "<tx_signature>"
  "id": 1
}
```

3. submitBatch
* Description：Non-atomic batch submission of multiple signed transactions. Each transaction is processed independently, and failure of one does not affect others.
* Method：POST
* Path：/submit-batch
* Header：Content-Type: application/json


* Non-atomicity: Each transaction in the batch is confirmed and executed independently. If one transaction fails, it does not affect the processing of other transactions in the batch.


Parameters

| Field   | Type   | Description                    |         Example      |
|-----------|---------|--------------------------------------------|-----------|
| transactions    | array[string]       | Array of fully signed transactions, Base64 encoded strings. Maximum 5 transactions per batch. | See below      |
| options  | array[string]       | Encoding configuration object  |  []     |

Response

| Field   | Type   | Description     | 
|--------|--------|------------------------|
| signatures  | array[string] | Array of transaction signatures corresponding to the submitted transactions, in the same order |

Example Request：
```
curl -X POST 'https:// ny.rpc.blockrush.io/submit-batch ' \
-H 'Content-Type: application/json' \
-H 'x-api-key: <YOUR_API_KEY>' \
-d '{
    "options": [],
    "transactions": [
      "<base64_encoded_tx_001>",
      "<base64_encoded_tx_002>",
      …
      "<base64_encoded_tx_005>",
    ]
  }
}
```
Example Response：
```
{
  "code": 1,
  "message": "success"
  "signatures": [
"<tx_001_signature>",
"<tx_002_signature>",
…
"<tx_005_signature>"
  ]
}
```

4. sendBundle
* Description: Atomic bundle submission of multiple signed transactions executed in sequence. All transactions fail if any single transaction fails.
* Method：POST
* Path：/
* Header：Content-Type: application/json

* NOTE: By default, API Keys do not have sendBundle enabled. Please contact customer service to enable this feature for your API key.
* Strong Atomicity: All transactions in the bundle are executed atomically in sequence. If any transaction in the bundle fails, the entire bundle will be rejected and no transactions will be processed.

Parameters


| Field   | Type   | Description                    |         Example      |
|-----------|---------|--------------------------------------------|-----------|
| params    | array[string] | Array of fully signed transactions, Base64 encoded strings. Maximum 5 transactions per bundle. | 见下     |
| encoding  | string        | Transaction encoding format, must be base64          | base64   |


Response
| Field   | Type   | Description                  |
|--------|--------|------------------------|
| result | string | Bundle ID used to identify this bundle. This is the SHA-256 hash of the bundle transaction signatures. |

Example Request：
```
curl -X POST 'https:// ny.rpc.blockrush.io ' \
-H 'Content-Type: application/json' \
-H 'x-api-key: <YOUR_API_KEY>' \
-d '{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "sendBundle",
  "params": [
[
  "<base64_encoded_tx_001>",
  "<base64_encoded_tx_002>",
  …
  "<base64_encoded_tx_005>",
],
    { "encoding": "base64" }
  ]
}'
```
Example Response：
```
{
  "jsonrpc": "2.0",
  "result": [
     "<tx_001_signature>",
     "<tx_002_signature>",
     …
     "<tx_005_signature>"
  ]
  "id": 1
}
```


## SDK

We have prepared commonly used SDK tools for developers:


### Rust

Explore the BlockRush Rust JSON-RPC, a high-performance SDK designed to leverage the power of Rust for interacting with the BlockRush infrastructure on Solana. Environment Support: Rust 1.75+, Tokio asynchronous runtime Feature Suggestions: Enable rustls or native-tls, use connection pooling and timeout control in production environments. 

#### Dependencies and Imports

```
[dependencies]  
blockrush-sdk = { path = "path/to/blockrush-sdk-0.1.0.crate" }  
tokio = { version = "1", features = ["full"] }  
serde = { version = "1", features = ["derive"] }  
serde_json = "1"  
```

```rust
#[tokio::test]  
async fn test_blockrush_sdk() {  
    let sdk = BlockrushSdk::new(Region::JP, "xxxxxx").await.unwrap();  
    assert_eq!(sdk.region(), Region::JP);  

    let tx_data = "ASN=".to_string();  

    let resp: SdkResponse<serde_json::Value> = sdk.send_transaction(tx_data).await.unwrap();  
    println!("{:?}", resp.body);  

    let resp2: SdkResponse<serde_json::Value> = sdk.submit_batch(&vec!["ASN=".to_string()]).await.unwrap();  
    println!("{:?}", resp2.body);  
}
```


### JavaScript and TypeScript

The BlockRush JS JSON-RPC library provides similar functionality tailored for both Node.js and browser environments, optimized for efficient SWQoS interactions. Environment Support: Node.js 20+, modern browsers (with fetch/AbortController support) Security Tip: Do not hardcode real production API keys in the browser. You can use backend-issued short-term tokens or proxy forwarding.

  

## Quick Start

```javascript
import { BlockrushSDK, Region } from "../blockrush-nodejs-sdk/index.js";  

const sdk = new BlockrushSDK({  
  region: Region.JP,  
  api_key: "YOUR_API_KEY", // Example key, use environment variables, config files, etc. in production
});  

// This request demonstrates the submit_batch API
const res = await sdk.submit_batch([  
  "base64_string_of_fully_signed_transaction_1",  
  "base64_string_of_fully_signed_transaction_2",  
]);  
console.log(res);  

// This request demonstrates the send_transaction API
const res2 = await sdk.send_transaction(
"base64_string_of_fully_signed_transaction_3"
);  
console.log(res2);

sdk.destroy();  
```


## Priority Fee & Tip

When using our SWQoS, you can use the default priority fee (no special setup required), with tips as the priority option. When building your transaction, you need to add a tip transfer instruction to your transaction to speed up its inclusion in a block.

## Example Code

* The base64 encoded tx data generated by the example code can be directly used by the SDK's methods, for example, passed to send_transaction.

### Rust
```rust
pub const MEMO_PROGRAM_ID: &str = "MemoSq4gqABAXKb96qnH8TysNcWxMyWCqXgDLGmfcHr";
pub const BLOCKRUSH_TIP_WALLET: &str = "AhMVT9KWLGjBbzvdqQkTt4vHwpawnGNoGSqaqXYftNDR";
pub const DEFAULT_TIP_AMOUNT: u64 = 1_000_000; // 0.001 SOL

pub fn create_blockrush_tip_ix(pubkey: Pubkey, tip_amount:u64) -> Instruction {
    let tip_ix =
        instruction::transfer(
            &pubkey,
            &Pubkey::from_str(BLOCKRUSH_TIP_WALLET).unwrap(),
            tip_amount,
        );
    tip_ix
}

fn create_memo_tx_with_tip(msg: &[u8], payer: &Keypair, blockhash: Hash) -> Transaction {
    let memo = Pubkey::from_str(MEMO_PROGRAM_ID).unwrap();
    let instruction = Instruction::new_with_bytes(memo, msg, vec![]);
    let mut instructions = vec![instruction];
    let tip_ix = create_blockrush_tip_ix(payer.pubkey(), TIP_AMOUNT);
    instructions.push(tip_ix);
    let message = Message::new(&instructions, Some(&payer.pubkey()));
    Transaction::new(&[payer], message, blockhash)
}

let test_tx = create_memo_tx_with_tip(b"this is a demo tx", &keypair, latest_hash);
let serialized_test_tx = bincode::serialize(&test_tx)?;
let base64_test_tx = BASE64_STANDARD.encode(&serialized_test_tx);
```

### JavaScript and TypeScript
```javascript
const ixs = {your instructions ixs};
const keypair = {your_actual_keypair};
const TIP_AMOUNT = 1_000_000; // 0.001 SOL;
const BLOCKRUSH_TIP = new PublicKey("AhMVT9KWLGjBbzvdqQkTt4vHwpawnGNoGSqaqXYftNDR");

// Append Tip payment instruction
ixs.push(
    SystemProgram.transfer({
        fromPubkey: keypair.publicKey,
        toPubkey: BLOCKRUSH_TIP,
        lamports: TIP_AMOUNT,
    }),
);

// Compile and sign the set of transaction instructions
const messageV0 = new TransactionMessage({
    payerKey: keypair.publicKey,
    recentBlockhash: latestBlockhash.blockhash,
    instructions: ixs,
}).compileToV0Message();
const transaction = new VersionedTransaction(messageV0);
transaction.sign([keypair]);

// Get the Base64 encoded string of the fully signed transaction
const transactionRaw = Buffer.from(transaction.serialize()).toString('base64');
```


## Error Handling

Error responses usually contain an error description and an optional error code. It is recommended to log the request ID/error message for troubleshooting.

#### Success：
```
{
  "jsonrpc": "2.0",
  "result": "3TEjEHSHPfNp5frqxVW...",
  "id": 1
}
```
#### Failure：
API key is invalid or does not exist
```
{
  "jsonrpc": "2.0",
  "error":  { “code”: -32602, message: "Invalid API Key. “, data: None }
  "id": 1
}
```
#### Method Error：
```
{
  "jsonrpc": "2.0",
  "error":  { “code”: -32601, message: "Method no found.“, data: None }
  "id": 1
}
```

## Rate Limiting：

Returns HTTP code 429.


## Regions

| Location           | Protocol  | Endpoint                        |
|---------------|-------|---------------------------------|
| 🇺🇸 New York   | https | https://ny.rpc.blockrush.io     |
| 🇩🇪 Frankfurt  | https | https://fra.rpc.blockrush.io    |
| 🇯🇵 Tokyo      | https | https://jp.rpc.blockrush.io     |





