# bitcoin
[simple concept of blockchain](https://medium.com/cobinhood-%E4%B8%AD%E6%96%87%E5%A0%B1/tagged/blockchain-technology)

[change address strategy](https://bitzuma.com/posts/five-ways-to-lose-money-with-bitcoin-change-addresses/)

[opcodes wiki](https://en.bitcoin.it/wiki/Script)

[build transaction(important)](https://klmoney.wordpress.com/bitcoin-dissecting-transactions-part-2-building-a-transaction-by-hand/)

[build transaction1 about witness](https://medium.com/coinmonks/how-to-create-a-raw-bitcoin-transaction-step-by-step-239b888e87f2)

[build transaction2](https://medium.com/@support_62391/exploring-bitcoin-signing-the-p2pkh-input-b8b4d5c4809c)

[bitcoin script opcodes](https://en.bitcoin.it/wiki/Script)

[bitcoin sighash type](https://raghavsood.com/blog/2018/06/10/bitcoin-signature-types-sighash)

[5 types of standard trx](https://medium.com/@wilsonhuang/mastering-bitcoin-%E7%AD%86%E8%A8%98-standard-transactions-undone-bfb9b4ed0ed8)

[p2sh transaction](https://www.soroushjp.com/2014/12/20/bitcoin-multisig-the-hard-way-understanding-raw-multisignature-bitcoin-transactions/)

[default address](https://coinomi.freshdesk.com/support/solutions/articles/29000009746-what-are-default-compatibility-and-legacy-addresses-all-about-segwit-)

[segwit](https://bitcoincore.org/en/segwit_wallet_dev/)

[bitpay core](https://github.com/bitpay/bitcore/blob/master/packages/bitcore-node/docs/api-documentation.md)

[bitcoin rest api](https://developer.bitcoin.com/rest/docs/address)

## technology!!!!
[bip143](https://github.com/bitcoin/bips/blob/master/bip-0143.mediawiki)

[segwit trx](https://bitcoin.stackexchange.com/questions/77180/what-is-witness-and-what-data-does-it-contain)

[bitcoin xpub prefix](https://github.com/arcbit/arcbit-ios/issues/14)

[xpublic key generate](https://github.com/cryptocoinjs/hdkey/blob/master/lib/hdkey.js)

[bitcore-lib error 1](https://stackoverflow.com/questions/16904658/node-version-manager-install-nvm-command-not-found)

[bitcore-lib error 2](https://github.com/creationix/nvm/issues/1648)

[bitcore-lib error 3](https://github.com/bitpay/bitcore/issues/1454)

[address prefix](https://en.bitcoin.it/wiki/List_of_address_prefixes)

[multisig example](https://gist.github.com/gavinandresen/3966071)

## tools
[coinb multisign tool](https://coinb.in)

## explorer
[blockchain pushtx](https://www.blockchain.com/zh-tw/btc/pushtx)

[bitcoin testnet smartbit pushtx/explorer](https://testnet.smartbit.com.au/txs/pushtx)

# bitcoincash
[mastering bitcoincash](https://developer.bitcoin.com/mastering-bitcoin-cash/4-transactions/)

[address format](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/cashaddr.md)

[explorer api???](https://developer.bitcoin.com/rest/docs/block)

[explorer api1](https://bch.btc.com/api-doc)

[explorer api complete](https://github.com/bitpay/bitcore/tree/master/packages/bitcore-node)

[cashAddr generation](https://github.com/bitcoincashjs/cashaddrjs/blob/master/src/cashaddr.js)

[bitcoincash github](https://github.com/bitpay/bitcore-lib-cash/blob/master/docs/examples.md#generate-a-random-address)

[sighash forkID and transaction](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/replay-protected-sighash.md)

[transaction format](https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/transaction.md)

[different decision between BTC and BCH(has no segwit)](https://www.reddit.com/r/btc/comments/8vw7qt/can_anyone_explain_to_me_why_bitcoin_cash_choose/)

[many push tx web](https://bitcoin.stackexchange.com/questions/60695/how-do-you-push-a-transaction-to-the-bitcoin-cash-network)

[bitcoincash.js](https://bitcoincashjs.github.io/)

[bch testnet explorer](https://test-bch-insight.bitpay.com/)

[bch push tx](https://tbch.blockdozer.com/tx/send)

[bch faucet](https://developer.bitcoin.com/faucets/bch/)

# litecoin
[many info](https://www.reddit.com/r/litecoin/comments/7iafc3/explain_the_main_differences_between_btc_and_ltc/)

[different from BTC](https://www.genesis-mining.com/litecoin-mining-guide)

[address difference](https://bitcoin.stackexchange.com/questions/62781/litecoin-constants-and-prefixes/80355#80355)

[explorer for m-address](https://blockchair.com/litecoin)

# Omni layer(USDT #31)
[omni explorer(TetherUS)](https://www.omniexplorer.info/asset/31)

[blockchair explorer(TetherUS)](https://blockchair.com/bitcoin/omni/property/31)

[omni layer spec](https://github.com/OmniLayer/spec)

[omni trx](https://www.jishuwen.com/d/2Lat/zh-tw)

[omni simple send trx](https://github.com/OmniLayer/omnicore/wiki/Use-the-raw-transaction-API-to-create-a-Simple-Send-transaction)


# other
[coinbase CPFP](https://blockcast.it/2020/03/03/how-coinbase-deal-with-cold-storage-challenge/)


# transaction format!!!
## multisig( p2sh(p2ms) )
hash function: double sha256
```sh
# rawData(before sign)
01000000 # version
01 # count of input
4d7cd8fb2c2285ef7c49be6483c2f2c82950a0efff111012578ee7652d0c09d7 # prev trx hash, LE
00000000 # prev out, LE

# redeem script
69 # total redeem length
52 # OP_ZERO + n
21 # public key 1 length
03d70e78115a73990a1ca8829694cc9b9804f668059bc01ec70fd7363a29c5b0ca # public key 1
21 # public key 2 length
02d188690fe8e806c2da8869dbbae6e22d68ca41f9587270965028d9f41444d922 # public key 2
21 # public key 3 length
0256a931037afbe56aa462185f4a6a2664b575b58ebdef3d81d88a5b8c228fbe27 # public key 3
53 # OP_ZERO + m
ae # OP_CHECKMULTISIG

feffffff # sequence
{
  假如有第二個input
  當要簽第一次時，第二個input的redeem script要寫0x00，代表0 length的redeem，
  當要簽第二次時，其餘的input的redeem script要寫0x00，代表0 length的redeem
}
01 # count of output
581b000000000000 # amount(satoshi), LE
17a914b14db996e5c602701e0a0ba75032ae6e0f164c5387 # redeem for output
00000000 # locktime
01000000 # sig hash type(SIGHASH_ALL 0x01), LE
```

```sh
# first sign raw data(sign above raw data!)
01000000
01
4d7cd8fb2c2285ef7c49be6483c2f2c82950a0efff111012578ee7652d0c09d7
00000000

b4 # total len for signature and redeem
00 # bug for signature
473044
# signature for first keypair
0220
697b0b5bc8e038af7e0c3c8c540708943fcbc699df500fad804e95604a89d341 # signature r
0220
3125bd8d4631e3cb2aa6f7414e7cd4bdee971526f01ae9472742bbe190e974bd # signature s
01 # sighash type(SIGHASH_ALL 0x01)
4c # ??
6952
21
03d70e78115a73990a1ca8829694cc9b9804f668059bc01ec70fd7363a29c5b0ca
21
02d188690fe8e806c2da8869dbbae6e22d68ca41f9587270965028d9f41444d922
21
0256a931037afbe56aa462185f4a6a2664b575b58ebdef3d81d88a5b8c228fbe27
53ae

feffffff
01
581b000000000000
17a914b14db996e5c602701e0a0ba75032ae6e0f164c5387
00000000
```

```sh
# second sign raw data(sign above raw data!)
# can collect all signature and allocate each position of signature
01000000
01
4d7cd8fb2c2285ef7c49be6483c2f2c82950a0efff111012578ee7652d0c09d7
00000000
fc # total len for signature and redeem
00473044
0220
697b0b5bc8e038af7e0c3c8c540708943fcbc699df500fad804e95604a89d341
0220
3125bd8d4631e3cb2aa6f7414e7cd4bdee971526f01ae9472742bbe190e974bd
01 # sighash type(SIGHASH_ALL 0x01)
473044
0220
53159892b8a5e7163eec01542ce644c84377771b33d60ecb76cfe9f7c1693280
0220
27f696baf4fc39febb11d8c02619c3ebff52320ddb6887e0e08acf714b99e46e
01 # sighash type(SIGHASH_ALL 0x01)
4c # ??, ignore
6952
21
03d70e78115a73990a1ca8829694cc9b9804f668059bc01ec70fd7363a29c5b0ca
21
02d188690fe8e806c2da8869dbbae6e22d68ca41f9587270965028d9f41444d922
21
0256a931037afbe56aa462185f4a6a2664b575b58ebdef3d81d88a5b8c228fbe27
53ae
feffffff
01
581b000000000000
17a914b14db996e5c602701e0a0ba75032ae6e0f164c5387
00000000
```

## multisig( p2sh-p2wsh-p2ms(segwit) )
```sh
# raw data before sign
01000000 # version
74afdc312af5183c4198a40ca3c1a275b485496dd3929bca388c4b5e31f7aaa0 # hash prevOut
# comprised of
doubleSha256(utxo1(trxId.reverse()+outputIndex.reverse())+utxo2...)
3bb13029ce7b1f559ef5e747fcac439f1455a2ec7c5f09b72290795e70665044 # hash sequence
# comprised of
doubleSha256(utxo1(sequence)+utxo2...)
36641869ca081e70f394c6948e8af409e18b619df2ed74aa106c1ca29787b96e01000000 # outpoint
# comprised of
# 現在如果是簽第一個utxo的資料，outpoint就是第一個utxo的(trxId.reverse()+outputIndex.reverse())
# 簽第幾個，outpoint就是填對應的utxo的(trxId.reverse()+outputIndex.reverse())
```

# script type
## p2sh(p2wsh(p2ms))
```
p2ms redeem:
OP_2
public key 1 length(0x33)
public key 1
public key 2 length
public key 2
public key 3 length
public key 3
OP_3
OP_CHECKMULTISIG>

p2wsh redeem:
OP_0
p2ms redeem sha256 hash length(0x32)
p2ms redeem sha256 hash

p2sh redeem:
OP_HASH160
hash160 length(0x14)
hash(不一定是public key hash,是看你要用什麼redeem script的has160)
OP_EQUAL

p2sh address:
redeem script(p2wsh redeem) hash160
base58check
get p2sh address
```
## p2pkh
```
p2pkh redeem:
OP_DUP,
OP_HASH160,
0x14,
pubKeyHash(hash160),
OP_EQUALVERIFY,
OP_CHECKSIG

p2wpkh redeem:
OP_0,
0x14,
pubKeyHash(hash160)

```