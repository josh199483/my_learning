# xrp info
## trx format before sign
```
version: 53545800 (4bytes)
12000022 (4 bytes)
flags: 80000000 (4 bytes)
source tag: 23 00000001 (5 bytes) (4 bytes for number BE)
24 (1 byte)
sequence: 00000062
destination tag: 2E 00000064 (5 bytes) (4 bytes for number BE)
201B
last ledger sequence: 02B9FA9F
amount: 61 4000000000989680(maximum is 61 416345785D8A0000 = 10^17)
fee: 68 400000000000000F
7321
public key: 031D68BC1A142E6766B2BDFB006CCFE135EF2E0E2E94ABB5CF5C9AB6104776FBAE (33 bytes)
8114AFF3C2E33458B30714CA16FFEE19952DD35C17C88314
address hash: 9EC07B09FA28E35A38541A6B779BE70DD0796D5C (20 bytes)
```

## trx format after signed
```
12000022
80000000
source tag: 23 00000001 (5 bytes) (4 bytes for number BE)
sequence: 24 0000000A (5 bytes) (4 bytes for number BE)
destination tag: 2E 00000064 (5 bytes) (4 bytes for number BE)
TransactionType??: 201B (payment)
last ledger sequence: 011D378B (4 bytes)
amount: 61 4000000005F5E100 (9 bytes) (8 bytes for amount BE)
fee: 68 400000000000000C (9 bytes) (8 bytes for amount BE)
public key: 732102AD2257E992754A6B8A32C184405529D48D215EB4673ECB1D5C217CA27E2015AE (33 bytes
signature: 74463044
02202230791F5E28E35CF5D0502B218ABA7152A93E8E037CCDE7F8E05D96C6771A8A
02207E7B896EBA1BD5CEAC6E87A864E877313C953086CD63CA148320AC7E185E0319

???: 8114BE50644A2B91C1E371DF554939B2A018865424E58314B

address hash: 27930AC6803C7856087B0ED83A7FE1F31A7B339 (20 bytes)
```

## maximum amount
10^17 drops

[send xrp example info](https://developers.ripple.com/send-xrp.html)

## offline prepare trx
preparePayment 是用在offline
prepareTransaction 是用在connect websocket server
[trx detail](https://developers.ripple.com/rippleapi-reference.html#payment)
