直接通过程序调用链上交易合约

```
from web3 import Web3


bsc = "https://bsc-dataseed.binance.org/"
web3 = Web3(Web3.HTTPProvider(bsc))

# 钱包地址
address = "0x756081584f4b349ae7f4586d2ae68390dd66df82"
# 钱包私钥
private_key = "xxxxxxxxxxxxxx"

# input data
data =
"0x25d377f9dda59123d8a34e9bb139132ae33e961fa6f30e31b113b985511a2388
ee425120000000000000000000000000000000000000000000000000000000bc3a4806cfefa2c
0000"

# 对交易进行签名
signed_txn = web3.eth.account.sign_transaction(
    dict(
        nonce=web3.eth.get_transaction_count(address),
        gasPrice=web3.toWei('20','gwei'),
        gas=500000,
        to=Web3.toChecksumAddress('0x7b4452dd6c38597fa9364ac8905c27ea44425832'),
        # 调用交易合约地址
        value=0,
        data=data,
    ), private,)

# 提交交易信息
tx_hash = web3.eth.send_raw_transaction(signed_txn.rawTransaction)

# 此笔交易的哈啤
print("https://bscscan.com/tx/%s" % web3.toHex(tx_hash))
```