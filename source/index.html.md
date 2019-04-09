---
title: KeyChain Documentation

language_tabs: # must be one of https://git.io/vQNgJ

  - json
  - javascript
  
toc_footers:
  - <a href='http://keychain.array.io'>KeyChain official website</a>

includes:
  - errors

search: true
---

# KeyChain Documentation
## Introduction

**KeyChain** is a standalone app for signing transactions and generating key pairs. It stores private keys in an isolated environment where no logger, debugger or spyware can intercept them because of the [three-layer security](#three-security-layers-of-keychain) protecting each action of the system.
**KeyChain** supports transactions from and to various blockchains, including Ethereum and Ethereum classic, Litecoin, Bitcoin, Bitcoin Cash, and Bitshares. 

## Installation

Download and install KeyChain for [macOS](https://github.com/arrayio/array-io-keychain/releases/). Windows and Linux installers are coming soon. 

*Try out KeyChain on the [demo page](https://arrayio.github.io/array-io-keychain/demo/).*

After installation, connect to the demo-page: [http://localhost:16384/](http://localhost:16384/) to check if the installation was successful and to test the KeyChain commands. In case everything went well, you will see the following page and you will be able to see responses to the commands in the "Response" box when you click on them.

<button class="show btn btn-info btn-sm" data-image='21'>show</button>

<img id='21' alt="image_2019-02-15_13-24-52" src="https://user-images.githubusercontent.com/34011337/52852054-a6e52a00-3128-11e9-91aa-6eb8cfc64b1f.png">


If you are having trouble connecting to the page, [contact us](#contact) and we will do our best to help you.

Below you can find comprehensive installation guides for [macOS](#macos), [Windows](#windows), [Linux](#linux).

## Getting started

After you have installed KeyChain for [macOS](https://github.com/arrayio/array-io-keychain/releases/download/), you can start using it with web3. Just install the `web3override` library from this [source](https://www.npmjs.com/package/web3override) and follow these simple steps (see the right panel in javascript).

If you launch KeyChain for the first time, you need to get a public key with the "select_key" command. 
You can save the public key to local storage. Then you will not need to use "select_key" command again. 

NB: Do not forget to install the library and require it:

1) Install the library
`npm install keychain.js`

2) Use an overridden web3 function

Now you can turn to the right panel where you use an overridden web3 function.
 
```json
//go to javascript
```

```javascript
const { Keychain, KeychainWeb3 } = require('keychain.js');
const Web3 = require('web3');
const web3 = new Web3('YOUR_API_URL'); // https://ropsten.infura.io/v3/046804e3dd3240b09834531326f310cf
const tx = {
  to: '0xE8899BA12578d60e4D0683a596EDaCbC85eC18CC',
  value: 100,
  gas: 21000
};
const keychain = new Keychain();
const keychainWeb3 = new KeychainWeb3(keychain, web3);
keychain.selectKey()
  .then(publicKey => keychainWeb3.signTransaction(tx, publicKey))
  .then(result => web3.eth.sendSignedTransaction(result.rawTransaction));
```

* `keychain.js` - Keychain class for working with the KeyChain WebSocket
* `keychainWeb3.js` - KeychainWeb3 class with methods `sign` and `singTransaction` for substituting `web3.eth.accounts.sign` and `web3.eth.accounts.signTransaction` methods

**Run tests**

If you wish to see KeyChain in action, install KeyChain, then install the library from this [source](https://www.npmjs.com/package/keychain.js) and import key to the `key_data` folder.


1) Add key to your `key_data`:

- public_key: `a7aea4bd112706655cb7014282d2a54658924e69c68f5a54f2cd5f35c6fcba9b610d6ae8549f960ae96e23ffc017f305c1d8664978c8ba8a1cc656fd9d068ef5`
- password: `qwe`

2) `npm run test`

<button class="show btn btn-info btn-sm" data-image='30'>show</button>

<img id='30' alt="keychain2" src="https://user-images.githubusercontent.com/2520008/55244335-1125da00-5252-11e9-971e-c7edfb208c4c.gif">


## Contact

If you have questions or enquiries about KeyChain, please do not hesitate to contact us:

- [Telegram](https://t.me/arrayio)
- [Stackoverflow](https://stackoverflow.com/users/10429540/array-io)
- [Twitter](https://twitter.com/ProjectArray)
- Or you can write us an email to [support@array.io](mailto:support@array.io). 

If you want to report a security issue, include the word "security" in the subject field.

We take security issues very seriously and we'll be looking forward to hearing from you. We hope you enjoy using KeyChain! 

## License

MIT License

Copyright (c) 2018 KeyChain

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


# Protocol

## Sign a transaction

### Command
sign_trx

> JSON Request

```json
{
  "command": "sign_trx",
  "params":
  {
    "transaction": "871689d060721b5cec5a010080841e00000000000011130065cd1d0000000000000000",
    "blockchain_type": "array",
    "public_key": "62826b9c7b6bbfcd89456c1e8068e141d6a46b2c1c0166ed25ba8ad6ede320f4454ff116d13f4e679e8224fcca49f49d50c279ed88513a1db7185946e26811ab01",
    "unlock_time": 45 
  }
}
```
```javascript
//go to json
```
### Query parameters
**Parameter**|**Type**|**Description**
---|---|---
transaction | ```hex string```|Hex representation of the transaction.
blockchain_type | ```enumeration string```|Inserts the name of blockchain you’re using. Possible options: array, bitcoin, ethereum, bitshares.
public_key | ```hex string```|64-byte public key in hex format
unlock_time  | ```integer```|This parameter is experimental and optional! If this parameter is defined and if it is greater than zero, it unlocks the key for a set number of seconds. While the key is unlocked, the transactions will be signed without the user's approval.

### Response format

> Response example

```json
{
  "result":"6cc47faa3778d15efeb470cd4154fdceb80633beaed15f0816d93951ffd7629a5fae3fe83c030f5f8a0cea82c1907f85418b93e820ea3b043c116053afae20a300"
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description**
---|---|---
result|`hex string`|65-byte signature in hex string format.

## Sign hash of transaction

This request is suited best for advanced users who are eager to work on a low level. You should have knowledge of how to calculate hash of transaction, given the type of blockchain, and know the type of signature and its packaging.

### Command
sign_hash

> JSON Request

```json
{
  "command": "sign_hash",
  "params":
  {
    "sign_type": "VRS_canonical",
    "hash": "fe5e4a8974715e20f47c8bb609547c9e66b0b9e31d521199b3d8d6af6da74cb1",
    "public_key": "a7aea4bd112706655cb7014282d2a54658924e69c68f5a54f2cd5f35c6fcba9b610d6ae8549f960ae96e23ffc017f305c1d8664978c8ba8a1cc656fd9d068ef5"
  }
}
```
```javascript
//go to json
```
### Query parameters
**Parameter**|**Type**|**Description**
---|---|---
sign-type |```enumeration string```|Customizes the way secp256 library is used by choosing one of its arguments through sign-type parameter. It has two possible value options: RSV_noncanonical and VRS_canonical. Default value is RSV_noncanonical. Prefix RSV/VRS means signature struct: [R, S, v] or [v, R, S].
hash |```hex string```|Hash calculated from the transaction. It can be the result of the first calculation or the second - depending on the type of the blockchain. For example, Bitcoin uses two calculations: to sign a bitcoin transaction you need to transmit the final (second) hash. Ethereum and Array make only one calculation to get the hash.
public_key|```hex string```|64-byte public key in hex format

### Response format

> Response example

```json 
{
  "result":"62826b9c7b6bbfcd89456c1e8068e141d6a46b2c1c0166ed25ba8ad6ede320f4454ff116d13f4e679e8224fcca49f49d50c279ed88513a1db7185946e26811ab01"
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description** 
---|---|---
result|`hex string`|65-byte signature in hex format.


## Select a key

### Command
select_key

> JSON Request

```json
{ 
  "command": "select_key"
}
```
```javascript
//go to json
```
### Query parameters
No

### Response format

64-byte public key. The length of the public key depends on the type of the blockchain. For example, in Ethereum, the length of a public key is 64 bytes.

> Response example

```json
{
  "result":"a7aea4bd112706655cb7014282d2a54658924e69c68f5a54f2cd5f35c6fcba9b610d6ae8549f960ae96e23ffc017f305c1d8664978c8ba8a1cc656fd9d068ef5"
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description**
---|---|---
result|`hex string`| 64-byte public key.

## Lock all unlocked keys 
This command protects you from any hostile intervention into the KeyChain while you have left your computer without supervision.

### Command
lock

> JSON Request

```json
{
  "command": "lock"
}
```
```javascript
//go to json
```
### Query parameters
No
### Response format

> Response example

```json
{
  "result":"true"
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description**
---|---|---
result|`bool`|bool result.

## Unlock private key

Unlock your key when you are ready to use it. 

<aside class="warning">
This parameter is experimental! Allowing the user to sign transactions without extra authorization is not secure and can be harmful. If <code>unlock_time</code> is greater than zero, you will see an insecure action warning. We do not recommend to use it. Although it might be needed if you want to speed up signing transactions.
</aside>

<aside class="notice">
<code>Sign_trx</code> and <code>sign_hash</code> commands assume implicit unlocking of your keys. 
</aside>


### Command
unlock 

> JSON Request

```json
{
  "command": "unlock"
  "params":
  {
    "public_key": "a7aea4bd112706655cb7014282d2a54658924e69c68f5a54f2cd5f35c6fcba9b610d6ae8549f960ae96e23ffc017f305c1d8664978c8ba8a1cc656fd9d068ef5",
    "unlock_time": 45
  }
}
```
```javascript
//go to json
```
### Query parameters

**Parameter**|**Type**|**Description**
---|---|---
public_key|```hex string```|64-byte public key in hex format
unlock_time|```integer```|When this parameter is specified, it unlocks the key for a set number of seconds. While the key is unlocked, the pass entry window will not appear and the transactions will be signed without the user's approval.

### Response format

> Response example

```json
{
  "result":"true"
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description**
---|---|---
result|`bool`|bool result.

## KeyChain version details

You can request the details of the current KeyChain version you are using.

### Command
about

> JSON Request

```json
{
  "command":"about"
}
```
```javascript
//go to json
```
### Query parameters
No

### Response format

> Response example

```json
{
  "result":
  {
    "version":"0.9.114","git_revision_sha":"59861769dca634d08d5442cb0074d40d8f544e66","git_revision_age":"9 minutes ago","compile_date":"compiled on Dec 12 2018 at 08:11:44","boost_version":"1.66","openssl_version":"OpenSSL 1.1.1  11 Sep 2018","build":"linux 64-bit"
  }
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description**
---|---|---
result|`json object`|version details as a json object with the following parameters.
version|`string`|number of the current version.
git_revision_sha|`string`|hash of the commit.
git_revision_age|`string`|age of the commit.
compile_date|`string`|time of compilation.
boost_version|`string`|required version of the boost library.
openssl_version|`string`|required openssl version.
build|`string`|required operating system.

## KeyChain version number

You can request the number of the current version you are using.

### Command
version

> JSON Request

```json
{
  "command":"version"
}
```
```javascript
//go to json
```
### Query parameters
No

### Response format

> Response example

```json
{
  "result":"0.9.114"
}
```
```javascript
//go to json
```
**Field name**|**Type**|**Description**
---|---|---
result|`string`|current version number which has the form of "[major].[minor].[build number]".

# Installation guides

## macOS

### System requirements

- macOS [10.12](#http://support.apple.com/downloads/DL1917/en_US/macosupd10.12.5.dmg) or [newer](https://support.apple.com/downloads/macos).

### How to install 

Download [KeyChain](https://github.com/arrayio/array-io-keychain/releases/) and follow the steps of the graphic installer. 

<link rel="stylesheet" type="text/css" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css">

1 Click "next" to start installation <button class="show btn btn-info btn-sm" data-image='1'>show</button>

<img id='1' width="562" alt="2018-12-13 19 42 06" src="https://user-images.githubusercontent.com/34011337/50005299-fbe6b180-ffba-11e8-97a9-0d8fc5145db8.png">

2 Accept the terms of the License Agreement <button class="show btn btn-info btn-sm" data-image='2'>show</button>

<img id='2' width="562" alt="2018-12-13 19 42 08" src="https://user-images.githubusercontent.com/34011337/50005305-fee1a200-ffba-11e8-9373-409a58a8f8a6.png">

3 Choose a folder and click "next" <button class="show btn btn-info btn-sm" data-image='3'>show</button>

<img id='3' width="562" alt="2018-12-13 19 42 10" src="https://user-images.githubusercontent.com/34011337/50005310-030dbf80-ffbb-11e8-967c-d5eadf1cfec0.png">

4 Click "install" for installation to start <button class="show btn btn-info btn-sm" data-image='4'>show</button>

<img id='4' width="562" alt="2018-12-13 19 42 13" src="https://user-images.githubusercontent.com/34011337/50005315-0608b000-ffbb-11e8-8dea-0fd9375b4aa7.png">

5 You will need to authorize the installation <button class="show btn btn-info btn-sm" data-image='5'>show</button>

<img id='5' width="555" alt="mac-4" src="https://user-images.githubusercontent.com/34011337/49649378-0db7da00-fa3a-11e8-95b3-3ae9f8152204.png">

6 Wait until the setup is complete <button class="show btn btn-info btn-sm" data-image='6'>show</button>

<img id='6' width="518" alt="2018-12-13 19 42 24" src="https://user-images.githubusercontent.com/34011337/50005317-086b0a00-ffbb-11e8-8d81-7ecebd54dabb.png">
  
7 Congratulations! You have installed KeyChain <button class="show btn btn-info btn-sm" data-image='7'>show</button>

<img id='7' width="532" alt="image_2019-02-15_13-24-52" src="https://user-images.githubusercontent.com/34011337/49649380-0e507080-fa3a-11e8-8645-b86dbd139dbc.png">



### Check if KeyChain is installed

After installation, connect to the demo-page: [http://localhost:16384/](http://localhost:16384/) to check if the installation was successful and to test the KeyChain commands. In case everything went well, you will see the following page and you will be able to see responses to the commands in the "Response" box when you click on them.

<button class="show btn btn-info btn-sm" data-image='8'>show</button>

<img id='8' alt="image_2019-02-15_13-24-52" src="https://user-images.githubusercontent.com/34011337/52852054-a6e52a00-3128-11e9-91aa-6eb8cfc64b1f.png">


## Windows

Windows installer will be accessible at v.1.0.

We are passionate about KeyChain and seek to make it as soon as possible, so that you could enjoy its wonderful features on any operating system you like.

### System requirements

- Windows 7 or newer.

 

<script src="https://code.jquery.com/jquery-3.3.1.min.js" integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=" crossorigin="anonymous"></script>
<script>
  var button = $(".show");
  var image = $("img");
  image.hide();
  button.on("click", function(){
    var id = $(this).data('image')
    $("#"+id).toggle();
  })
</script>

## Linux 

Linux installer will be accessible soon.

We are passionate about KeyChain and seek to make it as soon as possible, so that you could enjoy its wonderful features on any operating system you like.

### System requirements

- Ubuntu 16.04 or newer

- Debian 9 or newer

- Linux Mint 18.3 or newer



# Sign an Ethereum transaction

Here you can find an instruction on how to sign an Ethereum transaction with KeyChain.

On this [demo page](https://arrayio.github.io/array-io-keychain/eth_signer/) you can try out signing Ethereum transactions with KeyChain.

## Step-by-step guide

### 1. Install KeyChain for macOS

Download KeyChain installer for [macOS](https://github.com/arrayio/array-io-keychain/releases/).

### 2. Request public key

Start with the command `wscat -c ws://localhost:16384/`

 
> Select a  key 

```json
{ 
  "command": "select_key"
}
```
```javascript
//go to json
```

### 3. Get the address 

The address can be calculated from the public key.

> Get address from public key

```json
//go to javascript
```
```javascript
const ethUtil = require('ethereumjs-util');
const publicKey = 'YOUR_PUBLIC_KEY';
const fromAdd = ethUtil.publicToAddress(publicKey).toString('hex');
```

### 4. Make a transaction 

Now you can transfer money to the address corresponding to the public key.

In case you work with ropsten - 
https://faucet.ropsten.be/

### 5. Check the balance 

Check the balance of the address - it should have enough ether for a successful transfer.

> Check balance

```json
//go to javascript
```
```javascript
web3.eth.getBalance(fromAdd) 
.then(console.log);
```

### 6. Sign transaction 

You can now use the key that you have generated to sign a transaction.

You can find an example of the code [here](https://github.com/arrayio/keychain.js/blob/master/example/signETH.js).

###  7. Check Etherscan

[Etherscan](https://ropsten.etherscan.io/address/0x1ba05dad1abe91fdea3afffe9676b59076ce0ece)

![screen shot 2018-11-30 at 19 19 52](https://user-images.githubusercontent.com/34011337/49658738-d35b3680-fa53-11e8-9d49-7739492ecab7.png)

# WebSocket API

**KeyChain** contains a WebSocket API. This API can be used to stream information from a KeyChain instance to any client that implements WebSockets. Implementations in different languages:

- NodeJS
- Python
- JavaScript/HTML
- etc

Install KeyChain for [macOS](https://github.com/arrayio/array-io-keychain/releases/).

## Demo pages in JavaScript

### For testing the KeyChain commands

- On this [demo page](https://arrayio.github.io/array-io-keychain/demo/) you can test all the KeyChain commands.

- [Here](https://github.com/arrayio/array-io-keychain/tree/master/docs/demo) you will find the **code** of the KeyChain demo page. It will be automatically installed together with the KeyChain.

### For signing transactions

- [Here](https://arrayio.github.io/array-io-keychain/eth_signer/) you can try out signing Ethereum transactions with KeyChain.

- You can find an example of the code [here](https://github.com/arrayio/keychain.js/blob/master/example/signETH.js).

## Message format

> For example, here is a request for requesting the current version: 

```json
{"command":"version"}
```

```javascript
//go to json
```

Each WebSocket API message is a json serialized object containing a command with the corresponding parameters.

For full comprehensive descriptions of the commands, acceptable parameters and values, go to the [Protocol](#generate-a-key-pair). 

## WebSocket integration guide

### NodeJS integration example

The following will show the usage of websocket connections. 

> We make use of the `wscat` application available via npm:

```json
//go to javascript
```

```javascript
$ npm install -g wscat
```

> A simple call would take the form:

```json
//go to javascript
```

```javascript
$ wscat -c ws://127.0.0.1:16384
>{"command":"version"}
```

#### Successful Calls
The API will return a properly JSON formated response carrying the same id as the request to distinguish subsequent calls.

```json
{"result":  ..data..}
```

```javascript
//go to json
```

#### Error

In case of an error, the resulting answer will carry an error attribute and a description in human-readable format:

```json
{"error":"Error: keyfile could not be found by public_key"}
```

```javascript
//go to json
```

### JavaScript integration example

Before you proceed with the integration, you need to install KeyChain for [macOS](https://github.com/arrayio/array-io-keychain/releases/).

#### Test in the web

After installing KeyChain, open the browser and connect to the demo page via [http://localhost:16384/](http://localhost:16384/). 

When the connection is established, you will see the following KeyChain page. If you click on one of the commands on the left panel, you will see its json request and response in the white boxes below:

![image_2019-02-15_13-24-52](https://user-images.githubusercontent.com/34011337/52852054-a6e52a00-3128-11e9-91aa-6eb8cfc64b1f.png) 

#### Test from a terminal application:

You can see if the installation was successful by going to the terminal app, opening KeyChain and trying one of the commands that you can take from the [Protocol](#generate-a-key-pair):


c:\Users\User-1>cd "C: Program Files\WsKeychain"
c:\Program Files\WSKeychain>keychain 
{"command":"version"}

A successful response will take the following format:

#### Response format

> Response example

```json
{
  "result":"0.9.114"
}
```
```javascript
//go to json
```

**Field name**|**Type**|**Description**
---|---|---
result|`string`|current version number which has the form of "[major].[minor].[build number]".


### Build a page that connects to WebSocket

[Here](https://github.com/arrayio/array-io-keychain/blob/master/docs/demo/index.html) you will find an example of how to build a web-page that connects to WebSocket.

Save this to the folder  where you will be running the `websocket` command.

Open this page via localhost: [http://localhost:16384/](http://localhost:16384/)

You can find the code for the demo page and the page itself up in the [Demo pages in JavaScript](#demo-pages-in-javascript) section.

# Pipe API

**KeyChain** has an I/O stream (pipe) integration option. Interaction with **KeyChain** through pipes requires a terminal application as an input module that sends requests (STDIN) to the security layer that shows you the details of your request. When you approve the request through an interative gialogue window, the request goes back through the signing module and gives you an answer (STDOUT). 

For detailed description of the process, see [Three security layers](#three-security-layers-of-keychain) section.

## Message format

Each Pipe API message is a json serialized object containing a command with the corresponding parameters.

For full comprehensive descriptions of the commands, acceptable parameters and values, go to the [Protocol](#generate-a-key-pair).

## Pipe integration guide

Before you proceed with the integration, you need to install KeyChain for [macOS](https://github.com/arrayio/array-io-keychain/releases/).

When the installation is complete, you can open stream input to start sending json requests through STDIN - STDOUT pipes.

###### Sign transaction

```javascript 
const { spawn } = require('child_process');
const path = require('path');
const keychain = spawn(path.join(__dirname, 'keychain'));
const queue = [];

const sendCommand = (command, callback) => {
  keychain.stdin.write(JSON.stringify(command));
  queue.push(callback)
};

keychain.stdout.on('data', data => {
  queue.shift()(JSON.parse(data))
});

sendCommand({command: 'select_key'}, response => {
  const selectedKey = response.result;
  console.log('Selected key: ', selectedKey);
  const signCommand = {
    command: 'sign_trx',
    params: {
      transaction: 'eb0885098bca5a00825208948ec6977b1255854169e5f9f8f163f371bcf1ffd287038d7ea4c6800080038080',
      blockchain_type: 'ethereum',
      public_key: selectedKey,
    }
  };

  sendCommand(signCommand, response => console.log('Sign result: ', response));
});
``` 
For descriptions of all the commands and parameters, see [KeyChain Protocol](https://github.com/arrayio/array-io-keychain/wiki/KeyChain-Protocol).

# Restore your keys with master key

KeyChain allows its users to restore their keys using a master key.

If you have lost or forgotten one of your public keys, you can restore it in the Key Manager by entering your seed phrase that you get when creating your first key. KeyChain will derive your keys from the master key.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<p align="center"><strong>NB: restoring keys is impossible without a seed phrase</a></strong></p>
<p align="center"><strong>Memorize your seed when you create your first key</a></strong></p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

The seed is created after KeyChain collects entropy data to protect the master key. 

KeyChain starts collecting entropy data when it has checked whether the user who downloaded KeyChain has any keys at their disposal. The process which leads to launching the Entropy window can be described as the following UML diagram:

![keychain_launch](https://user-images.githubusercontent.com/34011337/54205772-08aa7100-44e8-11e9-85fb-c4097f841f77.png)

## Step 1: Collecting entropy data

![entropy](https://user-images.githubusercontent.com/34011337/54205056-9e450100-44e6-11e9-94f9-da8ae6c0c791.jpg)

## Step 2: Displaying the seed

![seed phrase](https://user-images.githubusercontent.com/34011337/54205086-b288fe00-44e6-11e9-949f-7cd250c2786f.jpg)

# KeyChain security

## About KeyChain security

Choosing a means of storing keys is an important and responsible task that everybody needs to address when considering making **transactions** - irrespective of the type of  blockchain they use. That is why here we show you the **detailed structure** of our KeyChain - so that your choice of key storage is an informed decision based on real knowledge of what stands behind the system.

## How KeyChain ensures safe key storage

KeyChain encrypts private keys using AES256 algorithm and stores the keys in an isolated environment that is protected with [three security layers](#three-security-layers-of-keychain). 

**AES256** was first adopted by the U.S. government and now is used worldwide as a secure and reliable way of protecting information. It stands for **Advanced Encryption Standard** which handles **256-bit keys**. This is a symmetric encryption algorithm that creates an output (ciphertext) from the input (plaintext) in 14 rounds which involve several steps of encryption. These steps combine the procedures of other symmetric encryption algorithms: substitution cipher with a reference table, adding round key,  shifting rows, and mixing columns - all performed multiple times. 

## OS-specific KeyChain security

### Unix-like operating systems

For Linux, we use a unique mechanism created by our team.

Typically, Linux offers the following algorithm of interacting with the user:


**1)** Client app = > connects => to X-server

**2)** Client app => sends some X-proto specified commands => X-server receives the commands => X-server renders an interface window

**3)** User enters a passphrase into the window => X-server catch the passphrase from the user => sends X-proto commands to the client => client app processes the passphrase from the user


However, around 1984, at the time when X11 was created, there existed no such task as performing secure operations via the Internet. The developers of X11 did not set out to protect the user’s data from someone capturing it. Even now, there is still no real mechanism against this kind of attack.
To solve the problem of protecting the data, we have decided to look past the standard solutions. Instead of receiving a passphrase from a user through the X-server, we have chosen to receive the passphrase from the keyboard driver. This serves as a shortcut that allows KeyChain to work without connecting to the X-server, thus minimizing the risk of someone stealing the passphrase.

Therefore, now instead of the following sequence…


- The user enters the passphrase into the window => X-server catches the passphrase => it sends the passphrase through X-proto => Client app processes the request


… we have:


- The user enters the passphrase => KeyChain catches the keyboard’s events directly


The shorter the path, the fewer weak points can be found. We exclude the weakest link (X-server) from the process of entering the passphrase. Thus, for the third party to compromise the passphrase, they will need to intercept it right at the keyboard level, which requires to have root access and hence makes it almost impossible.

You might be asking yourself: if KeyChain functions without connecting to the X-server, why does the user see the dialogue window?

The answer is simple and is motivated by our concern with the user experience. Working through the command line is rather inconvenient for most people. That is why we use an emulator program that imitates the process of inputting the passphrase by receiving events from the KeyChain daemon. Note that no secret data goes into the emulator – like a mirror that only reflects light without absorbing any of it. This allows KeyChain to minimize the risks to the minimum.

### Windows

Users of **Windows 10 Enterprise Edition** can benefit from a new security feature - **Isolated User Mode (IUM)**. It employs a set of modes called **Virtual Trusted Levels (VTL)** to run processes separately, without accessing each other’s memory. 
We launch KeyChain on VTL1 (SecureMode). Any malware that is launched on VTL0 (NormalMode) does not have access to KeyChain. The mechanism of isolating the kernels is executed as a Windows OS process. Learn more about IUM processes [here](https://docs.microsoft.com/en-us/windows/desktop/procthread/isolated-user-mode--ium--processes).

To ensure secure passphrase entry, **Windows Vista/7/8** and **Windows 10 (not Enterprise Edition)** use a mechanism similar to the one used for **User Access Control (UAC)**. In particular, UAC is used at a program setup to avoid giving a malware access to the system context.

KeyChain gets access to the system environment when it is being installed. 
A malware can only access the user context (unless it is installed in the system as a service). The processes that are launched in the user context do not have access to the applications that are launched in the isolated context.Hence, a malware cannot get access to KeyChain data because of the mechanism of separating the access between the levels of the OS. For more information, please refer to [Microsoft documentation](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works).

### macOS

macOS has in innate security mechanism that does not allow any other program to interfere with a process if it was not the one that started it. Therefore, all we needed to do was to take care of the intermediary step that takes place at the passphrase entry on the keyboard. That is why we incorporated the `EnableSecureEventInput` function that provides a means for a process to protect sensitive data from being intercepted by other processes. Learn more on the [Apple developers portal](https://developer.apple.com/library/archive/technotes/tn2150/_index.html#//apple_ref/doc/uid/DTS10004249).

## Three security layers of KeyChain

![diagram keychain fin 1](https://user-images.githubusercontent.com/34011337/49658582-719acc80-fa53-11e8-97bc-e957ef604075.png)

Apps or websites send requests to the KeyChain through two types of communication - standard I/O streams (mostly called pipes), and the WebSocket. 
The architecture of the KeyChain software consists of the three independent layers:

1. **API layer** which integrates with your app, website or any external application. It is language-neutral. The protocol for the terminal application operates with the JSON format in synchronous request/response way. The main function of the **API layer** is to transmit and parse commands for given API. 
Each request carries information about commands, the type of key user wants to use to sign transactions and other relevant parameters which you can find in the [Protocol](#generate-a-key-pair). 

2. **Security layer** receives the commands from the API layer and acts as an OS-specific  protection mechanism for the **interface window** (third layer). It serves as a shield from potential attacks at sensitive data and information. **Security layer** is tailored for the Mac OS, Linux, and Windows OS and operates only with permitted files (through admin access). 
The request, transmitted to the **Signing module** which holds the private keys, works simultaneously with the Secured input module that uses OS-specific mechanism. The **Secured input module** protects the passphrase from key grabbers and malware.

3. **Representation layer** is the **UI window** which notifies the user about the details of transactions and necessary actions. The **interface window** is initiated from **Security layer**. Once the user inputs the correct passphrase, it sends the permission to the **Signing module** to unlock the demanded key. Passphrase input field is protected by the secured input module. **Security layer** decrypts the given key with the correct passphrase entered by the user.  In this instance **Signing module** can operate with the open private key, for example it can extract information, sign transactions, therefore responding to given requests.

# Useful links

This page contains links to the scholarly and publicistic sources that we find useful concerning KeyChain.

- Building a Transaction By Hand: [guide](https://klmoney.wordpress.com/bitcoin-dissecting-transactions-part-2-building-a-transaction-by-hand/)

- Cryptography Docs: [documentation of cryptographic algorithms used in banking](https://www.adjoint.io/docs/cryptography.html)

- Default nonce function: [blog thread on the topic "What is RFC6979?"](https://www.reddit.com/r/Bitcoin/comments/1mwzo2/what_is_rfc6979/)

- Guide to Elliptic Curve Digital Signatures: [guide with thorough explanations of technical aspects](http://royalforkblog.github.io/2014/09/04/ecc/)

- Доступно о криптографии на эллиптических кривых: [about elliptic curves in Russian](https://habr.com/post/335906/)
