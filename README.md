# HD-address-generator
HD-adddress-generator is used to generate different kinds of addresses,which allowed bip32 bip39 bip44 and bip84.If you want,you could create bip141 by using my bip32

## About my bip32 code
My bip32 code is from bip32utils,but i add P2WPKH address and rewrite bip32 extend key version by using sqlite3.So we can add new key version easily. Read more key version : https://github.com/satoshilabs/slips/blob/master/slip-0132.md

How to generate coin address 
----------------------------
```
from Derivation_Path import bips
from root_key import key
    entropy = key.to_mnemonic(data="a9495fe923ce601f4394c8a7adadabc3").seed()
    bip = bips.initialize("m/49'/0'/0'/0",seed=entropy)
    bip.exkey()   #return BIP32 Extended Private&Public Key
    bip.cokey()   #retrun privkey and pubkey
    bip.wif()     #return WalletImportFormat privkey
    bip.address() #return address
    
    bip.gen(5)    #return information {index:[subpath,address,wif,cokey]} 
    such as: 
    #{0: ["m/49'/0'/0'/0/0", '3PRoiesDDHaRAsWVDV2ekSMf3Y3KApkyEH', 'L2qHrMfSHbsLBbTkDpXb9dysffNhefJwk7GtA7WNbkmQX5Nh81zD', ('a77dad30e1a39d6dbd2dbd1bf5740777ac6c74855693d91f77fafd96e4867f8c', '0361ad6aa540cb92c0477b482f43cb52a8315dd6a0b0cc797243ed249d47089db3')]}
```
See how to get the accounts
```
from mnemonic import Mnemonic
import unittest
import binascii
from Derivation_Path import bips
from root_key import key

_store = [
["m/44'/0'/0'/0/0","1HnA8fYPskppWonizo8v1owqjBDMviz5Zh","02707f1e1a0e1ea7ba8ce83710604304fa85f995b6b0d15ff752cf70602cf4757e","KzR58Tj1WkLvMJmyZzt3P4KTLJpv2tn7xK32nfHGNq9Ffw8WLUEc"],
["m/44'/0'/0'/0/1","19UbynsayGFyzSttsyaWAScUoDJV44hukC","036409b451fa2d626376dc63c49f787716a8b823bc72aac0052c43c2a780e1133f","L48hJkym4z3WDu9gbeRgdF67FWYbeD7kmnxVWkqb1a6TSm44oEJY"],
["m/44'/0'/0'/0/2","1A5YmdnXm1BFTciqDgU7jNGMagRiydAKN2","033ee2fb84b95cec5e5d111e901f8487395d821993806414da7d8ee01cb1e7941a","L4qwB74LW3w935dEDzzFxoMfyhX3oToP1CKdkX9npAuDRWqSg2pj"],
["m/44'/0'/0'/0/3","1GoD7BUF8oPUkjuv4NoPx74aRaeb2BRKyH","03b444e59c92359d85e56a6de86143456be93f00b6d6b52dd2ef6d5cfc2d56ee83","L3Bm5RDRX5xt6oy5xy2b8yK7kf3F8wqykoTR1jsLW6mbyy42tHhn"],
["m/44'/0'/0'/0/4","1JDtA64hxnsCcrk6dibaFEXH5ytL2coy44","035851a2e3d42b3fa0eaee904c6f81c3fee2c71dacf1a1db7caab580edaa202417","KzVJ8nTJLLCC2TkxRtXtbA3WixPXz6wG15MkXsN9kwDKG3QPfhyn"],
["m/44'/0'/0'/0/5","12SDdnhhtqqXUpt9NCXAj7Biq7U8LmCTYG","03cb51f0e1f70b6dcf053d9d2c13cbf9cef747d8456861ba95e60a37b55e95b067","L4VkN6BVzhmzrLCo1AKV1BbuAD5aanftNc5LDi6uCd9cqhnkv8PS"],
["m/44'/0'/0'/0/6","17sdDw3rkKqt46Dn1RBay6Xs8cPspBNr34","03925a483d2aa23b78e71b37a8c1621f99bf81bd0b2fae6157b288e7eef23aba7d","KyGrxM8rdTcdQogdXULuoZoubzQmHy5sUVee9Z718Zy8XBvhtSiG"]
]

class test_(unittest.TestCase):

	def test_to_seed_by_words(self):
		words = "plate inject impose rigid plug tornado march art vast filter issue village"
		passphrase = ""
		seed = Mnemonic.to_seed(words, passphrase)
		seed_ = b"99b5ccefea83beb7dd42e62bb3bceb965b935c26c85290febc48f571060a1d8c8f4e847c45b67864977ae1d0c1e83e6ae0019a78c571c0fc3fdee6e020329664"
		self.assertEqual(binascii.hexlify(seed), seed_)

	def test_to_accounts_by_words_passphrase(self):
		words = "plate inject impose rigid plug tornado march art vast filter issue village"
		passphrase = ""
		seed = Mnemonic.to_seed(words, passphrase)
		bip44 = bips.initialize("m/44'/0'/0'/0",seed=seed)
		store = bip44.gen(7)
		self.assertEqual(
			{k:[ele if p !=3 else ele[1] for p,ele in enumerate(v)] for k,v in store.items()}, # remove private key
			{p:v[:-2] + v[-2:][::-1] for p,v in enumerate(_store)}) # store

	def test_to_accounts_by_entropy(self):
		entropy = "a62e81c7dcfa6dcae1f066f1aacddafa"
		seed = key.to_mnemonic(data=entropy).seed()
		bip44 = bips.initialize("m/44'/0'/0'/0",seed=seed)
		store = bip44.gen(7)
		self.assertEqual(
			{k:[ele if p !=3 else ele[1] for p,ele in enumerate(v)] for k,v in store.items()},
			{p:v[:-2] + v[-2:][::-1] for p,v in enumerate(_store)})
	
def __main__():
	unittest.main()

if __name__ == "__main__":
	__main__()
```