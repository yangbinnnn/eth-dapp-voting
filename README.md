# eth-voting

Ethereum dapp study

# 基本开发
1. 使用`ganache-cli` 快速搭建本地开发测试环境
2. 手动编译并部署合约

## 测试环境搭建
1. 安装nodejs
2. 安装开发工具，npm install ganache-cli web3@0.20.2 solc
3. 创建并运行开发测试节点，./node_modules/.bin/ganache-cli
4. 编译部署合约
```
Web3 = require('web3')
# 连接测试节点
var web3 = new Web3(new Web3.providers.HttpProvider("http://127.0.0.1:8545"))
# 编译合约
var code = fs.readFileSync('volting.sol').toString()
var solc = require('solc')
compliedCode = solc.compile(code)
abiDefifnition = JSON.parse(compliedCode.contracts[':Voting'].interface)
# 初始化部署合约
VotingContract = web3.eth.contract(abiDefifnition)
deployedContract = VotingContract.new(['Rama', 'Nick', 'Jose'], {data: byteCode, from: web3.eth.accounts[0], gas: 4700000})
# 获取合约地址, 部署完成
deployedContract.address
```

## Vote 流程
1. `eth_accounts` 获取当前账号列表，取第一个用户交易

req:
```
{
    "jsonrpc": "2.0",
    "id": 19,
    "method": "eth_accounts",
    "params": []
}
```
resp:
```
{
    "id": 19,
    "jsonrpc": "2.0",
    "result": [
        "0x1658d981b081537973b24ca57d12a1515f028403",
        "0x34358711ad76f21eb8a3297e693fa22f999c7df6",
        "0x80ea81d1a687b89c12a294b70a546cdd7f6cebd7",
        "0xaa830fab767b97ac885548c0f3a77b0b7b76d5bf",
        "0xb9b92cb9e6ca9b8effccce7bf9bd73bd4f3e4a19",
        "0xaff5ed507fa170bdcd5849d72369866b5a443dd9",
        "0xd58da7d53fa4e0e39898520019b786a7b3572e4a",
        "0x588f2aef3e91379e4106c507921bd63caa37645e",
        "0x453590be9a2303001aa17b6eaddb32965af57f0a",
        "0x87ff55e3a1422e0e4d0798fcad8843a11aed0768"
    ]
}
```
2. `eth_sendTransaction` vote投票，使用第一个账号往合约地址发送交易

req:
```
{
    "jsonrpc": "2.0",
    "id": 20,
    "method": "eth_sendTransaction",
    "params": [
        {
            "from": "0x1658d981b081537973b24ca57d12a1515f028403",
            "to": "0xcb2bb35e231fdeb18c6b3c3c1007d9d2ac526c35",
            "data": "0xcc9ab2674a6f736500000000000000000000000000000000000000000000000000000000"
        }
    ]
}
```

resp:
```
{
    "id": 20,
    "jsonrpc": "2.0",
    "result": "0x0343e898cf783d0b03f0eb0e0c1f5d218f9322c19d0e60dfb2942000b7c4df74"
}
```

3. `eth_call` 获取投票信息

req:
```
{
    "jsonrpc": "2.0",
    "id": 21,
    "method": "eth_call",
    "params": [
        {
            "to": "0xcb2bb35e231fdeb18c6b3c3c1007d9d2ac526c35",
            "data": "0x2f265cf74a6f736500000000000000000000000000000000000000000000000000000000"
        },
        "latest"
    ]
}
```

resp:
```
{
    "id": 21,
    "jsonrpc": "2.0",
    "result": "0x0000000000000000000000000000000000000000000000000000000000000005"
}
```

# 进阶开发
1. 使用ethereum `geth`或`mist` 搭建公共测试环境`rinkeby`
2. 使用`Truffle` 开发框架

## 测试环境搭建
1. 完成`基本开发`测试环境搭建
2. 安装`geth`或`mist`，当前使用[mist](https://github.com/ethereum/mist/releases)
3. 切换到`rinkeby test network`，访问https://www.rinkeby.io/#faucet 获取`ether`
4. 安装`Truffle` 开发框架，`npm install -g truffle webpack`
5. 安装`webpack` 打包依赖，`npm install --save web3@0.20.2 jquery@3.1.1`

## 总结
1. 往合约地址发送交易可以调用合约函数
2. 本地开发可以使用ganache-cli 在内存中快速创建ethereum 模拟环境
3. 参考https://medium.com/@mvmurthy/full-stack-hello-world-voting-ethereum-dapp-tutorial-part-1-40d2d0d807c2


# 问题
1. DAPP 如何执行?
2. DAPP 产生的数据如何存储？