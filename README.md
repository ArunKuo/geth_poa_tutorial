## 施工中......

# Windows系統-Geth PoA 私有鏈架設筆記
# 環境

- Windows 10 專業版 x64
- Geth v1.8.23

# 下載、安裝
## 下載

下載很簡單，直接到 [Go Ethereum官網](https://geth.ethereum.org/downloads/)下載就行了。  
請選擇自己的作業系統來下載。我自己下載了Windows版 v1.8.23。

## 安裝

下載完後，執行下載下來的安裝檔。

Step1. 點選 I agree    
![step1](img/install1.png)  

Step2. 將 Geth 與 Development Tools打勾並按 Next  
![step2](img/install2.png)  

Step3. 選擇安裝路徑。我這邊直接選擇預設路徑。完成後按Install 開始安裝  
![step3](img/install2.png)  

## 確認版本

開啟命令提示字元，並輸入指令：`geth version` 來看GETH的版本資訊 

```
C:\>geth version      # 會顯示出GETH的版本資訊
Geth
Version: 1.8.23-stable
Git Commit: c942700427557e3ff6de3aaf6b916e2f056c1ec2
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.11.5
Operating System: windows
GOPATH=
GOROOT=C:\go
```

## 建立存放區塊鏈資料的資料夾

為了方便我們在C槽底下建立一個資料夾，我自己取名叫node。  

# 建立以太坊帳戶

現在要來建立以太坊的帳戶，先將命令提示字元切換到C槽底下並輸入指令：`geth --datadir node account new`來建立帳戶。輸入完指令後會要求輸入密碼，並要求再輸入一次密碼做確認。都輸入完之後就會產生一個以太坊的帳戶。    

```
c:\>geth --datadir node account new
INFO [03-25|16:57:09.744] Maximum peer count                       ETH=25 LES=0 total=25
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase:
Repeat passphrase:
Address: {4b5d920a41e2c6b07cc422f66d4f9e8177530ae1}
```

指令`--datadir`後面接的值為儲存區塊鏈資料與帳戶keystore的資料夾。  

# 使用puppeth來建立PoA私有鏈的Genesis檔案

接下來我們要來使用Geth裡面的開發工具**puppeth**來進行PoA私有鏈的創世區塊檔案。  
在命令提示字元輸入`puppeth`，它會要求輸入一個網路的名稱。想取什麼都可以，我自己取名為poa。  
```
c:\>puppeth
+-----------------------------------------------------------+
| Welcome to puppeth, your Ethereum private network manager |
|                                                           |
| This tool lets you create a new Ethereum network down to  |
| the genesis block, bootnodes, miners and ethstats servers |
| without the hassle that it would normally entail.         |
|                                                           |
| Puppeth uses SSH to dial in to remote servers, and builds |
| its network components out of Docker containers using the |
| docker-compose toolset.                                   |
+-----------------------------------------------------------+

Please specify a network name to administer (no spaces, hyphens or capital letters please)
> poa

Sweet, you can set this via --network=poa next time!

 [32mINFO  [0m[03-25|17:45:55.524] Administering Ethereum network            [32mname [0m=poa
 [33mWARN  [0m[03-25|17:45:55.553] No previous configurations found          [33mpath [0m=.puppeth\\poa
```
接著這邊要選擇2，來建立一個創世區塊檔  
```
What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
> 2
```
然後這邊選擇1，從0開始建立一個genesis檔  
```
What would you like to do? (default = create)
 1. Create new genesis from scratch
 2. Import already existing genesis
> 1
```
選擇要使用的共識演算法，這邊我們選擇2 **Clique-PoA**  
```
Which consensus engine to use? (default = clique)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
> 2
```
接下來要來設定區塊產生的時間，預設為15秒。我這邊也設成跟預設相同。
```
How many seconds should blocks take? (default = 15)
> 15
```
這裡要指定一個帳戶來做為授權打包的角色，把我們剛剛建立帳戶位置貼上去。  
可以指定多個帳戶，這邊我們只指定一個帳戶作為打包的角色。  
```
Which accounts are allowed to seal? (mandatory at least one)
> 0x4b5d920a41e2c6b07cc422f66d4f9e8177530ae1
> 0x
```
同樣的，這邊輸入我們建立的帳戶上去，使帳戶裡面事先擁有一些ether。   
```
Which accounts should be pre-funded? (advisable at least one)
> 0x4b5d920a41e2c6b07cc422f66d4f9e8177530ae1
> 0x
```
接著會問你是否要將precompile-addresses配置1 wei。  
推薦是使用yes，由於不是很懂這次就先不要。
關於**precompile-addresses**以及這個message要做什麼  
可以參考[此連結](https://ethereum.stackexchange.com/questions/68056/puppeth-precompile-addresses?rq=1)  
```
Should the precompile-addresses (0x1 .. 0xff) be pre-funded with 1 wei? (advisable yes)
> no
```
輸入你的網路ID，我這邊設置為15   
```
Specify your chain/network ID if you want an explicit one (default = random)
> 15
 [32mINFO  [0m[03-25|18:19:31.099] Configured new genesis block
```
### 網路ID:    
| ID | chain                         |
|----|-------------------------------|
| 0  | Olympic (disused)             |
| 1  | Frontier (now mainnet)        |
| 2  | Morden (disused)              |
| 3  | Ropsten (current PoW testnet) |
| 4  | Rinkeby (current Geth PoW testnet) |
| 5  | Goerli  (cross-client PoA testnet) |

資料來源:  
[Ethereum Wire Protocol (ETH)](https://github.com/ethereum/devp2p/blob/master/caps/eth.md#newblockhashes-0x01)、[How to select a network id or is there a list of network ids?](https://ethereum.stackexchange.com/questions/17051/how-to-select-a-network-id-or-is-there-a-list-of-network-ids)
