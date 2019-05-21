
![Build Status](https://steemitimages.com/300x200/https://en.bitcoin.it/w/images/en/c/cb/BC_Logotype.png)
# Bitcoin (BTC)

### Bitcoin Core란?
우리가 알고있는 비트코인은 Bitcoin core 클라이언트를 구동하는 블록체인을 지칭한다.
오픈소스로 공개되어 있어 Github를 통해 누구나 이용할 수 있다.


### Bitcoin Core Package 구성
| 패키지 | 설명 |
| ------ | ------ |
| bitcoin-qt | 비트코인의 GUI클라이언트, 지갑(Wallet)에 해당한다.|
| bitcoin-daemon(bitcoind) | Back-end에서 동작하는 클라이언트로 JSON-RPC를 통해 개발자를 위한 API를 제공한다. |
| bitcoin-cli | Command Line Interface로 bitcoind를 동작시키기 위한 명령어를 입력한다. |


### Bitcoin Core Install Guide
#### Install 사전준비
```sh
# OS X command line tools
$ xcode-select --install
```
```sh
# 의존성 라이브러리 설치
$ brew install automake berkeley-db4 libtool boost miniupnpc openssl pkg-config protobuf python qt libevent
```

```sh
# make deploy를 사용해 디스크 이미지를 만드려면 RSVG가 필요하므로 brew명령어로 라이브러리 설치
$ brew install librsvg
```

#### Source Download
원하는 위치로 이동하여 github에서 소스를 내려받는다.
```sh
$ git clone https://github.com/bitcoin/bitcoin
```
#### Core Build
아래와 같이 빌드한다.
만약 GUI를 비활성화하여 빌드하고자 할 경우, configure에 --without-gui 옵션을 같이 명령한다.
```sh
$ ./autogen.sh
$ ./configure
$ make
```

#### Core Install
```sh
$ sudo make install
```



#### Testnet 기동
bitcoin-daemon(이하 bitcoind)를 이용하여 Bitcoin core를 실제 기동한다.
```sh
$ bitcoind -testnet -daemon
Bitcoin server starting
```
> 테스트모드 종류
> Testnet: 인터넷상에서 동작하는 테스트 네트워크. 테스트용 BTC를 사용하지만 처음 시작할 때 Testnet의 모든 블럭을 동기화해야 한다.
> Regtest: 로컬PC 내에서 동작하는 테스트 네트워크. 개인PC 내에서만 계정을 만들거나 채굴할 수 있고 블럭체인 초기화도 쉽게 때문에 테스트로 사용하기에 적합하다.

#### 지갑생성
생성을 하지 않아도 "walletname" : "" 이라는 지갑이 있다.
하지만 나중에 있을 혼란을 방지하기 위해 만드는 것을 추천한다.
```sh
$ bitcoin-cli -testnet createwallet bswallet
```

#### 지갑주소 생성
생성을 하면 지갑에 대한 지갑주소를 출력해준다.
```sh
# Default에 주소생성
$ bitcoin-cli -testnet -rpcwallet=bswallet getnewaddress
2NA1pnKS6otinYbaHcgeaNoGgZ7as3psMKf
# 특정 라벨에 주소생성
$ bitcoin-cli -testnet -rpcwallet=bswallet getnewaddress bswalletlabel
2NFpYbjTLSZp7N2Y4mPP5TNXsPcBZcmSChX
```

#### 테스트 비트코인받기 (채굴대신)
아래 웹사이트로 접속하여 비트코인을 받을 수 있다. [비트코인받기] (https://bitcoinfaucet.uo1.net/send.php)
지갑에 들어오기까지 몇분정도 소요될 수 있다.

#### 비트코인 송금
형식은 다음과 같다.
-rpcwallet={지갑이름} sendtoaddress {지갑주소} {코인 개수}
```sh
$ bitcoin-cli -testnet -rpcwallet=testuser1 sendtoaddress 2NFpYbjTLSZp7N2Y4mPP5TNXsPcBZcmSChX 0.0001
fe72391f75eceadd41df1ac80fb6ad7f19ec41533bcdad408060c896e2e244f4
```
전송명령을 하면 txid를 출력해준다.
비트코인이 전송완료까지는 어느정도 시간이 걸린다.

#### 지갑정보 확인
```sh
$ bitcoin-cli -testnet -rpcwallet=testuser2 getwalletinfo
{
  "walletname": "napawallet",
  "walletversion": 169900,
  "balance": 0.00000000,
  "unconfirmed_balance": 0.00010000,
  "immature_balance": 0.00000000,
  "txcount": 1,
  "keypoololdest": 1557476910,
  "keypoolsize": 999,
  "keypoolsize_hd_internal": 1000,
  "paytxfee": 0.00000000,
  "hdseedid": "0ff907546d3055729e78d66c70b3c222323d579a",
  "private_keys_enabled": true,
  "scanning": false
}
```
지갑정보를 출력하면 balance는 아직 0이지만, unconfirmed_balance는 0.00010000 인 것을 확인할 수 있다.
전송완료까지 시간이 조금 소요된다.

#### 그외의 명령어
```sh
[확정된 트랜잭션 확인]
$ bitcoin-cli -testnet -rpcwallet=testuser1  listunspent
[
]

[미확정 트랜잭션 확인]
$ bitcoin-cli -testnet -rpcwallet=testuser1 listunspent 0
[
  {
    "txid": "fe72391f75eceadd41df1ac80fb6ad7f19ec41533bcdad408060c896e2e244f4",
    "vout": 0,
    "address": "2Mz2Zz5qLiH5Etx1eqXGQYLZUjrihXrumoK",
    "redeemScript": "00148fb036c3563e713084338e7b7203a9be422eec31",
    "scriptPubKey": "a9144a642a4eed1a28ce7ba387c4622f7bc2ab98fedf87",
    "amount": 0.00009744,
    "confirmations": 0,
    "spendable": true,
    "solvable": true,
    "desc": "sh(wpkh([a0892090/0'/1'/0']02d9a937e57c909f6611761b0ae827f9d6eaf2894675d17aceb0fecd8e3eca8a98))#4x9kz08n",
    "safe": true
  }
]
```



