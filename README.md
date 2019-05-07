# Bitcoin (BTC)
![Build Status](https://en.bitcoin.it/w/images/en/2/29/BC_Logo_.png)
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
OS X command line tools : 
```sh
$ xcode-select --install
```
의존성 라이브러리 설치 :
```sh
$ brew install automake berkeley-db4 libtool boost miniupnpc openssl pkg-config protobuf python qt libevent
```
make deploy를 사용해 디스크 이미지를 만드려면 RSVG가 필요하므로 brew명령어로 라이브러리 설치 :
```sh
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



### Test net 기동
bitcoin-daemon(이하 bitcoind)를 이용하여 Bitcoin core를 실제 기동한다.
```sh
$ bitcoind -regtest -daemon
Bitcoin server starting
```
> 테스트모드 종류
> Testnet: 인터넷상에서 동작하는 테스트 네트워크. 테스트용 BTC를 사용하지만 처음 시작할 때 Testnet의 모든 블럭을 동기화해야 한다.
> Regtest: 로컬PC 내에서 동작하는 테스트 네트워크. 개인PC 내에서만 계정을 만들거나 채굴할 수 있고 블럭체인 초기화도 쉽게 때문에 테스트로 사용하기에 적합하다.

