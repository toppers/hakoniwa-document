# Windowsの場合の箱庭構成例

Windowsの場合の箱庭構成例は下図のとおりです。

![スクリーンショット 2024-01-13 0 41 58](https://github.com/toppers/hakoniwa-document/assets/164193/cb6654a8-7b0d-41be-84bf-7b3151af716b)


## Docker Container

箱庭コンダクタ(Hakoniwa Conductor)および箱庭アセット（Robot Control Program）は、Docker Container 内に存在しています。

Windows版の箱庭は、Docker Desktop for Windows は利用していません。

WSL2/Ubuntu 22.04.1 LTS の [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) をインストールして利用します。

Docker Container のネットワークは、ホストネットワーキングです。
これにより、コンテナ内のアプリケーションはホストマシンの IP アドレスとポートを直接使用することができ、ネットワークの設定を単純化することができます。

箱庭では、このモードを指定するために `--network="host"` というオプションを docker run コマンドに与えています。

例；

```bash
docker run --network="host" [その他のオプション] [イメージ名]
```

## IP アドレス

Windows上で動作するUnityとDocker Container内で動作する箱庭コンダクタの通信を成立させるために、それぞれのIPアドレスを取得方法を説明します。


### 箱庭コンダクタのIPアドレス(addr1)

箱庭コンダクタのIPアドレスは、WSL2/UbuntuのデフォルトのネットワークインタフェースのIPアドレスを使用します。

取得例：

```
NETWORK_INTERFACE=$(route | grep '^default' | grep -o '[^ ]*$' | tr -d '\n')
ifconfig "${NETWORK_INTERFACE}" | grep netmask | awk '{print $2}'
```

### UnityのIPアドレス(addr2)

UnityのIPアドレスは、WSL2/Ubuntuの `/etc/resolv.conf` の `nameserver` の IPアドレスを使用します。

取得例：

```
cat /etc/resolv.conf  | grep nameserver | awk '{print $NF}'
```

## ポート番号

ポート番号の構成は下表のとおりです。

|モジュール名|プロトコル|公開ポート番号|用途|
|---|---|----|---|
|箱庭コンダクタ|gRPC/TCP|50051|シミュレーション制御向け通信|
|箱庭コンダクタ|UDP|54001|PDU通信(箱庭コンダクタ受信側)|
|箱庭Unity|UDP|54003|PDU通信(Unity受信)|

