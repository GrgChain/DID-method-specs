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

create wallet mnemonic by HD wallet，here is the example：

```
eg: device leg summer source assault drill pilot include excess sausage immense year m/1 m/2
```

the last two words m/1 m/2 is used to tag the path of two wallet:

1. mainwallet 
2. recoverywallet

for example there is a mnemonic：device leg summer source assault drill pilot include excess sausage immense year

the path of main wallet is : m/44'/60'/0'/0/1

the path of recovery wallet is ：m/44'/60'/0'/0/2

## Method-specific DID Syntax

The namestring that identifies this DID method is: grg
The DID comprises of multiple elements separated by colons (:):
A DID that uses this method MUST begin with the following prefix: did:grg. Per this DID specification, this string MUST be in lowercase.

````
grg-did = "did:grg" grg-specific-idstring
grg-specific-idstring = ethereum-address
````

When create a did, will generate a mnemonic, and drive three wallets by a different path.
1. path is: m/44'/60'/0'/0/0, the ethereum-address of this wallet is used in grg-specific-idstring as grg did id.
2. path is: m/44'/60'/0'/0/1, the path of the main wallet.
3. path is: m/44'/60'/0'/0/2, the path of recovery wallet.

### Example

A valid grg DID might be :

```
did:grg:0x802B718AF528214931E2642C2b113436C847a16b
```

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

Create a DID by such function in JS SDK. This will return a did document and private info of this DID.
At the start of this document, provide an example of "didInfo" and "didDocument".
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
To update your did info, you need to get Login Token from service by this function.
And then use the token as a param in the following function.
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

Here is an example of how to update document service. Use JSON to describe your service and then use the update function like the following example.

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
Reset your document publickey by this function, please note that, in this case, all the claims issued by this did will be invalid.
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
Recover your did info and did document by your did mnemonic, here is an example.
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

#### 7. delete(revoke) DID
Deletion means invalidation of the existing DID
```
@param
1. didInfo
2. token

let res = await didClient.removeDid(didInfo, token);

@return
{code: 0, content: {}, msg: "success"}
```

#### 8. to be issuer
In grg did system, you can apply to be an issuer, and issue a claim to other did user.
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

#### 1. verify DID document

```
@param: didDocument（Json）


let res = await didClient.didClaim.verifyDid(didDocument)

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
When the issuer issue a claim to the user, he must create a rawClaim first, then use the issue claim function in JS SDK.
Here is an example.

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

#### 4. verify claim
```
@param
1. Claim

let res = await didClient.didClaim.verifyClaim(Claim);

@return
{code: 0, content: true, msg: "success"}
```

#### 5. encrypt by sm2

```
@param 
1. nonce (origin msg)
2. publicKey

let encryptStr = didClient.utils.encryptByPubkey(nonce, publicKey);

@return string:  encrypt msg

```
#### 6. decrypt by sm2

```
@param 
1. encryptStr (encrypt msg)
2. privateKey

let decryptRes = didClient.utils.decryptByPriKey(encryptStr, privateKey);

@return
{code: 0, content: origin msg, msg: "success"}
```

#### 7. encrypt（SM4）

```
@param 
1. msg (origin msg)
2. key

let encRes = didClient.utils.sm4Encrypt(key,msg)

@return
{code: 0, content: encrypt msg, msg: "success"}
```

#### 8. decrypt（SM4）

```
@param 
1. msg (key)
2. key

let decRes = didClient.utils.sm4Encrypt(key,msg)

@return
{code: 0, content: origin msg, msg: "success"}
```

## Security Considerations

### Key Management
The Key of grg did is derived from the mnemonic based on the HD wallet. Users can recover it's did identity by mnemonic. Mnemonics and did document are created locally on the client-side to avoid the disclosure of the private key.

### Identity authentication
GRG did document security authentication is based on the cryptography algorithm. A signature is used to verify that the claim is from a trusted did user. What should be noted is the authenticity verification of the issuer. An alliance blockchain maintained by an official organization is designed and used. In the alliance chain, the did document of the certification authority should be stored, and its did ID number should be displayed on the official website of the relevant organization. Therefore, the verifier can verify claims based on this information.

## Privacy Considerations
The privacy protection of did includes two parts:
1. Claim file privacy security；
2. Claim content privacy security;

For the first point, in grg did system, users are encouraged to store verifiable declaration files locally, so the claim file privacy is protected through security mechanism on the client-side. For the second point, it is suggested that the issuer should only include the minimum elements when issuing a claim.