# ethereumjs-tx-schnorr備忘録
### １、git clone
### ２、npm install
### ３、npm run build
### ４、cd ..
### ５、npm install -g ethereumjs-tx-schnorr
### ６、cd ethereumjs-tx-schnorr
### ７、cd dist
### ８、transaction.js内 L328付近 Vの検証を弄る
#### Vが44でもエラーが出ないようにしてるだけ

```go

Transaction.prototype._validateV = function (v) {
        if (v === undefined || v.length === 0) {
            return;
        }
        if (!this._common.gteHardfork('spuriousDragon')) {
            return;
        }
        var vInt = ethereumjs_util_1.bufferToInt(v);
        if (vInt === 27 || vInt === 28) {
            return;
        } else if (vInt === 44) {
            return;
        }
        var isValidEIP155V = vInt === this.getChainId() * 2 + 35 || vInt === this.getChainId() * 2 + 36;
        if (!isValidEIP155V) {
            throw new Error("Incompatible EIP155-based V " + vInt + " and chain id " + this.getChainId() + ". See the second parameter of the Transaction constructor to set the chain id.");
        } 
    };

```
