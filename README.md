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

# 確認版本

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

# 建立存放區塊鏈資料的資料夾

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

