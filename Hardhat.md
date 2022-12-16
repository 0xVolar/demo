这是关于hardhat的输出
hardhat是一款以太坊软件的开发工具

# 配置
可以在**hardhat.config.js**中可以进行自定义的配置，如果不想进行配置的话可以不用在意。在其中可以进行**defaultNetwork**, **networks**，**solidity**，**paths**，**mocha**的配置。

## 网络配置
在hardhat中存在两种网络，第一种是**JSON-RPC基础网络**，第二种是**内置的Hardhat网络**。在没有自定义网络的的时候，默认使用的是第二种网络。
- Hardhat网络
  - Hardhat有一个内置网络，当使用这个网络时会默认创建实例。Hardhat网络支持solidity语言，能够清楚的获取合约的状态，精确的执行合约操作，并且会对失败原因进行返回
- JSON-RPC基础网络
  - 这是hardhat提供连接其他外部网络节点的设置，使得节点能够运行在你的本地电脑上，可以使用下语句进行想要配置（需要什么写什么，没有说明的会用默认值替代除了必填的）
    - `url`：想要连接得外部节点得url，这是必须的
    - `chainId`：链Id，可以用来验证连接得网络是否正确，如果没有设置得话会进行一个省略验证
    - `from`：设置合约默认得使用者得地址，如果没有进行设置的话，将会使用节点的第一个账户进行替代。
    - `gas`：进行gaslimit的设置，可以设定想要的值，但是**默认值为“auto”**
    - `gasPrice`：设置gas的价格，和gas类似，**默认值为“auto”**
    - `gasMultiplier`：由于gas消耗量存在误差，设置一个值用来乘以预估之后扩大gas的消耗量保证合约能够运行。**默认值为1**.
    - `accounts`：Hardhat能够使用的账户，可以输入一个私钥数组指定可以使用的账户，或者使用一个HD Wallet。**默认值为“remote”**，即为使用远程节点中的账户
    - `httpHeaders`：通过此项设置在发送JSON-RPC请求时使用额外的Http报头，**默认值为“undifined”**。
    - `timeout`：设置JSON-RPC请求的超时时间，**默认值为本地主机40000，其他为20000**
- 默认的网络配置
    ```
    module.exports = {
        defaultNetwork: "localhost",
        netwrks
        {
            localhost(可以更改为想要的节点名称): {
                url: "http://127.0.0.1:8545"
            },
             hardhat: {
                // See its defaults
            }
        }
    }
    ```

## solidity配置
- 针对一个编译器的配置
  - `version`：编译器的版本
  - `settings`：跟solidity中编译器的**input Json**一样，具体可看`https://docs.soliditylang.org/en/v0.7.4/using-the-compiler.html#input-description`
- 针对多个编译器的配置
  - `compilers`：一个编译器配置的列表
  - `overrides`：可选择的编译器设置到object的映射。

## path配置
在这个配置中你可以自定义文件编译后的保存位置，或者其他文件的保存位置。

# 创建一个项目
- Hardhat基本结构目录  
在创建一个新的hardhat工程的时候，会提供三种选项创建**TS新项目**、**创建JS项目**、**创建基本的项目**。前面两种是基于不同语言的，而第三种是一个基本的结构目录，而一个基本的结构目录有以下的部分组成。
    `/contracts`：用来存放合约的目录
    `/test`：用来进行安全测试
    `/scripts`：自动执行的脚本，在其中可以定义想要的操作
    `hardhat.config.js`：配置文件，可以在其中配置solidity的版本

- 网络  
在hardhat中进行相关的网络配置，定义自己想要使用的节点。可以直接使用不同的链提供的不同的RPC url，同时也可以使用其他的节点配置进行访问。

# 运行一个项目
## 编写项目
- 使用命令`npx hardhat comple`，会自动对`/contracts`目录下的合约进行编译，编译后的文件会默认保存在`/aftifacts/`目录中，如果是首次编译的话会创建相应的目录。如果想改变存储目录，可以在config文件中进行自定义。
- 使用`--force`命令参数可以进行强制编译。`npx hardhat clean`命令回去清理缓存文件并且删除编译文件。
- 如果想要修改编译器的话，可以在config文件中进行修改。**通常的话在config中会指定solidity的版本**。也可以进行其他的设置

## 测试合约
- 在test目录下可以编写自己的测试脚本，测试脚本包含几个点
  - 有一个基础的部署好的合约，后面可以使用`loadFixture`获得合约刚部署的状态
  - 对合约的每一个方法都要进行测试，对判断条件也要进行测试，并且对方法中的每一个行为也要进行测试（事件触发）
  - 详情可看`https://hardhat.org/hardhat-runner/docs/guides/test-contracts`

- `npx hardhat coverage`：使用这个命令可以查看整个test命令覆盖的范围
- `npx hardhat test --parallel`：使用命令可以进行并行测试，正常情况下并行测试会产生相同的结果

## 部署合约
部署合约时推荐使用脚本进行合约的部署，在`scripts`目录下可以自定义自己的脚本。写好部署脚本后，可以在命令台输入命令运行执行部署合约。
- `npx hardhat run scripts/deploy.js --network <your-network> `，使用命令可以连接自定义网络，如果不使用命令参数的话就会默认使用本地的网络。

完成以上的操作就可以进行一个基本的合约的编写测试以及部署，并且可以进行自定义自己想要与合约的交互动作。

