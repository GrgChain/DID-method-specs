# DID-method-specs
#### Author: grgchain

## DID Introduction


#### 1. Example

```
1. Did document
{
	"@context": "https://w3id.org/did/v1",
	"id": "did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3",
	"version": 1,
	"created": "2020-08-25T15:05:02.364Z",
	"updated": "2020-08-25T15:05:02.364Z",
	"publicKey": [{
		"id": "did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3#keys-1",
		"type": "SM2",
		"publicKeyHex": "04712eb2f39204e9cf44728fe7e8191ad043091d0fc7d860be1339c565834b95862ed9956fe56caf9ddd46ae287d973f7d2ce974cdf474e56f1d204a65836f9882"
	}, {
		"id": "did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3#keys-2",
		"type": "SM2",
		"publicKeyHex": "048c89b67d48d9245ac20e600108b6dbb3a07f9275b0b0f0ad0b7457f8cb6aeb4c178c93f1d7fb7e0d7ee8b55261b28a4eba14158a6dd2d1b9a152fa9d2839c8bb"
	}],
	"authentication": ["did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3#keys-1"],
	"recovery": ["did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3#keys-2"],
	"service": [{
		"id": "",
		"type": "",
		"serviceEndpoint": ""
	}],
	"proof": {
		"type": "SM2",
		"creator": "did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3#keys-1",
		"signatureValue": "f12493e632f91e4f415732de62871fb3c3ffb943ce6e52682154837f0d8109bd65a183c8cf5b39d9c06e0a415ead1230b1d88db877fd6b0b2460acc503ead04b"
	}
}

2. Claim
{
	"@context": "https://w3id.org/did/v1",
	"id": "XXX",
	"type": ["ProofClaim"],
	"issuer": "did:grg:0x802B718AF528214931E2642C2b113436C847a16b",
	"issuanceDate": "2020-02-27T15:55:42.956Z",
	"expirationDate": "2020-04-01T12:01:20Z",
	"credentialSubject": {
		"id": "did:ccp:ceNobbK6Me9F5zwyE3MKY88QZLw",
		"shortDescription": "实名认证声明",
		"longDescription": "该用户经过了我司的实名认证",
		"type": "RealNameAuthentication"
	},
	"revocation": {
		"id": "https://example.com/v1/claim/revocations",
		"type": "SimpleRevocationListV1"
	},
	"proof": {
		"creator": "did:grg:0x802B718AF528214931E2642C2b113436C847a16b",
		"type": "Secp256k1",
		"signatureValue": "0x49a714001405bc39c12764a44e678854e388dca1edcce1384054d1e62244751358996e7593a46ccb6b35a2bfa85cea1ae1f90e98e81e2943e9c655f14d9ebb7b1c"
	}
}

3. Did Info
    {
      did: "did:grg:0x584b244f03BCE0e1e9766a880d0713410B955Fb3",
      mnemonic: "play smile scatter pigeon uncover among dismiss scatter fever rose wrestle rotate m/1 m/2",
      mainPrivateKey: "82b28ff4626e3151f873c1af9b3a28a8723fd5a7a74092812c92435f88f6cf9c",
      recoveryPrivateKey: "19863244cce676ccaf7507b49b2ae725d4603f8a7bbfe505978144129b56fa59"
    },
```

#### 2. DID wallet create 

create wallet mnemonic by Bip39， here is the example：

```
eg: device leg summer source assault drill pilot include excess sausage immense year m/1 m/2
```

the last two words m/1 m/2 is used to tag the path of two wallet:

1. mainwallet 
2. recoverywallet

for example there is a mnemonic：device leg summer source assault drill pilot include excess sausage immense year

the path of main wallet is : m/44'/60'/0'/0/1

the path of recovery wallet is ：m/44'/60'/0'/0/2

## SDK install

npm install 

## JS code build for app

npm run build 

#### use did sdk in app：

```
let options = {
    ResolverURL: 'http://10.1.51.29:3000/api/v1'
}
let didClient = new DidClient(options);
```

#### 1. Create DID

```
let res = await didClient.createDid();

@return:
{
	"code": 0,
	"content": {
		"didInfo": didInfo,
		"didDocument": didDocument
	},
	"msg": "success"
}
```

#### 2. get Login  Token

```
@param didInfo

let res = didClient.getLoginToken(didInfo);

@return:
{
	"code": 0,
	"content": {
		"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkaWQiOiJkaWQ6Z3JnOjB4MjJiQzNGMzY1QkMxM0M4RTc3NWQzRjhlZjUxYjJCN0NjMUE4MDYwNSIsImlhdCI6MTU4Mjc4NzY2MCwiZXhwIjoxNTgyODc0MDYwfQ.4Kphw5tFKsO1Z7QzWM2m9tABgZloqv1avtn3s-RDUZY"
	},
	"msg": "success"
}

```

#### 3. Update Document Service 

```
@param
1. didInfo
2. service
3. token

let service = [
    {
      id: "service-1",
      type: "real name auth",
      serviceEndpoint: "http://example.com"
    }
  ];

let res = await didClient.updateDidService(
    didInfo,
    service,
    token
  );

@return:
{"code":0,"content":{},"msg":"success"}
```

#### 4. reset Document publickey

```
@param
1. didInfo
2. type : "mainWallet" or "recoveryWallet"
3. token

let res = await didClient.updateDidPubkey(didInfo, type, token);

@return:
{"code":0,"content":{didDocument:{},didInfo:{}},"msg":"success"}
```

#### 5. recover DID Document

```
@param
1. mnemonic

let res = await didClient.recoverDid(mnemonic);

@return:
{
	"code": 0,
	"content": {
		"didInfo": didInfo,
		"didDocument": didDocument
	},
	"msg": "success"
}
```

#### 6. get DID Document

```
@param
1. id (Did id)

let res = await didClient.resolver.getDid({did:id});

@return
{
	"code": 0,
	"content": {
		"document": document
	},
	"msg": "success"
}
```

#### 7. delete DID

```
@param
1. didInfo
2. token

let res = await didClient.removeDid(didInfo, token);

@return
{code: 0, content: {}, msg: "success"}
```

#### 8. to be issuer

```
@param
1. issuerProvideData = 
{
  did:"did:grg:0xd1E2541aB74EFbE3f6A077DB4D1D28f2C411C5d2",
  description:"GrgBanking DID Project",
  company_name:"GrgBanking",
  person:"Nick",
  phone:"138000001111",
  email:"138000001111@163.com"
}

let res = await didClient.addIssuer(issuerProvideData);

@return
{code: 0, content: {}, msg: "success"}
```

## Method for issuer

#### 1. Sign for DID document

```
@param: didDocument（Json）


let res = adidClient.verifyDid(didDocument)

@return
{code: 0, content: true, msg: "success"}
```

#### 2. verify provideData post by user

```
@param
1. apply:
eg:
{
  did: did,
  provideData: {
	Name: "lilei",
  	MobilePhone: "13088888888",
  	ClaimType: "RealNameAuthentication"
  },
  signature: signature
};

let res await didClient.didClaim.verifyApplyMsg(apply);

@return
{code: 0, content: true, msg: "success"}
```

#### 3. Issue claim

```
@param
1. didInfo
2. rawClaim
eg:
{
  id: "aaa",
  type: ["ProofClaim"],
  expirationDate: "2020-04-01T12:01:20Z",
  credentialSubject: {
    id: "did:ccp:ceNobbK6Me9F5zwyE3MKY88QZLw",
    shortDescription: "auth claim",
    longDescription: "this user is verified by us",
    type: "RealNameAuthentication"
  },
  revocation: {
    id: "https://example.com/v1/claim/revocations",
    type: "SimpleRevocationListV1"
  }
};

let issuerRes = await didClient.didClaim.issueClaim(didInfo,rawClaim);

@return
{
	"code": 0,
	"content": Claim
	"msg": "success"
}

```

#### 3. verify claim
```
@param
1. Claim

let res = await didClient.didClaim.verifyClaim(Claim);

@return
{code: 0, content: true, msg: "success"}
```

#### 4. encrypt by sm2

```
@param 
1. nonce (origin msg)
2. publicKey

let encryptStr = didClient.utils.encryptByPubkey(nonce, publicKey);

@return string:  encrypt msg

```
#### 5. decrypt by sm2

```
@param 
1. encryptStr (encrypt msg)
2. privateKey

let decryptRes = didClient.utils.decryptByPriKey(encryptStr, privateKey);

@return
{code: 0, content: origin msg, msg: "success"}
```

#### 5. encrypt（SM4）

```
@param 
1. msg (origin msg)
2. key

let encRes = didClient.utils.sm4Encrypt(key,msg)

@return
{code: 0, content: encrypt msg, msg: "success"}
```

#### 5. decrypt（SM4）

```
@param 
1. msg (key)
2. key

let decRes = didClient.utils.sm4Encrypt(key,msg)

@return
{code: 0, content: origin msg, msg: "success"}
```
