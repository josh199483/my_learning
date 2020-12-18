[binance chain sdk doc](https://github.com/binance-chain/javascript-sdk/wiki/API-Documentation#Transaction)

[binance doc](https://docs.binance.org/encoding.html)

[binance api doc](https://docs.binance.org/api-reference/dex-api/paths.html#apiv1transactions)

[binance 3rd party github](https://github.com/binance-chain/javascript-sdk)

[explorer](https://explorer.binance.org/)



## technology
transaction format
```js
const tx = `
c201
f0625dee // stdTx type
0a48
2a2c87fa // msgSend type
0a
200a14
1e0d2c47ba675ff6b8591994ba1d1180b50868de // input from address decode
12080a
03424e42 // input asset BNB
10 904e // input amount 10000
12
200a14
f4871994dd36caa4868d14f9d103eac28da205ca // output address decode
12080a
03424e42 // output asset BNB
10 904e // output amount 10000
12
700a26
eb5ae9872103f32030673bd26cf64e50e0cff7cd3d277ec485babdce7ced98c81c58839b3174 // publicKey 38 bytes with prefix + 0x2103
1240
7e641ddfa7f8416991fbc37047ad460b0efef9a96963751ec86d5cbf79a1bfdd // r
56ffea7c21703fb80cf60ffceeffaabf520fca52dc715224ed373b760af1bf02 // s
18 f39503 // account number
20 37 // sequence
20 01 // source
`;
```