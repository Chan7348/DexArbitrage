# DexArbitrage
### abi
合约abi，一个函数的inputs参数中，internalType和普通type的区别：
一般情况下都显示基本类型比如uint256
但是也有例外情况
```solidity
contract B {
    struct View {
        uint x;
    }
    function foo(View memory bad) public view {
        this;
    }
}
```
函数 foo的abi中，参数bad的 internalType = "struct B.View"
                        type = "uint256"
这样设置主要还是为了方便调试，但合约不需要对其encode/decode


### 套利
WETH/USDC交易对
监控：

    cheap one(WETH price): USDC -> WETH, 监控 (算上手续费)3000USDC in 能拿到多少WETH -> 拿到WETH数量 amountWETH0
    expensive one(WETH price): WETH -> USDC, 监控 (算上手续费)需要多少个WETH in 才能拿到3000个USDC -> 需要的WETH数量 amountWETH1
        
profit = amountWETH0 - amountWETH1 - gas 利润，赚WETH
在便宜的地方用3000USDC买到稍多的ETH，在贵的地方卖出稍少的ETH得到3000USDC本金，盈利为WETH
