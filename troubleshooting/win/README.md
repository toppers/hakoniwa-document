# docker 起動時にエラー発生する

docker 起動時に以下のようなエラーが出る場合があります。

```
$ bash docker/run.bash
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied
 * Starting Docker: docker                                                                                              waiting for docker service activation..
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create?name=hakoniwa-ros2sim": dial unix /var/run/docker.sock: connect: permission denied.
```

原因は、/var/run/docker.sock にアクセス権がないためですので、以下のコマンドでアクセス権を与えることで直ります。

例：ユーザ名が tmori の場合

```
sudo chown tmori   /var/run/docker.sock
```

# シミュレーション開始したけど、Unity画面表示されない

Unity側に以下のファイルが存在しないか、コンフィグファイルの設定値が不正である可能性があります。

* [core_config.json](https://github.com/toppers/hakoniwa-document/blob/main/architecture/assets/README-unity.md#%E7%AE%B1%E5%BA%AD%E3%82%B3%E3%83%B3%E3%83%95%E3%82%A3%E3%82%B0%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%85%A5%E5%8A%9Bcore_configjson)
* [hakoniwa_path.json](https://github.com/toppers/hakoniwa-document/blob/main/architecture/assets/README-unity.md#%E7%AE%B1%E5%BA%AD%E3%82%B3%E3%83%B3%E3%83%95%E3%82%A3%E3%82%B0%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%85%A5%E5%8A%9Bhakoniwa_pathjson)
* [箱庭PDU定義ファイル](https://github.com/toppers/hakoniwa-document/blob/main/architecture/assets/README-unity.md#%E7%AE%B1%E5%BA%ADpdu%E5%AE%9A%E7%BE%A9%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%85%A5%E5%8A%9B)

## core_config.jsonの設定値

以下の項目が適切に設定されているか確認してください。

* cpp_mode: asset_rpc
* cpp_asset_name: UnityAsset
* core_ipaddr: [箱庭コンダクタのIPアドレス](https://github.com/toppers/hakoniwa-document/blob/main/architecture/examples/README-win.md#%E7%AE%B1%E5%BA%AD%E3%82%B3%E3%83%B3%E3%83%80%E3%82%AF%E3%82%BF%E3%81%AEip%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9addr1)
* core_portno: 50051
* asset_ipaddr: [UnityのIPアドレス](https://github.com/toppers/hakoniwa-document/blob/main/architecture/examples/README-win.md#unity%E3%81%AEip%E3%82%A2%E3%83%89%E3%83%AC%E3%82%B9addr2)
* pdu_udp_portno_asset: 54003
* pdu_configs_parent_path: ./ros_types/json
* pdu_bin_offset_package_dir: ./ros_types/offset

## hakoniwa_path.jsonの設定値

以下の項目が適切に設定されているか確認してください。

* hakoniwa_base: 存在するディレクトリぱすであるかどうか
* settings: .
* pdu_path: ./ros_types/json
* offset_path: ./ros_types/offset


# シミュレーション開始したけど、ロボットが動かない

考えられる原因として、Unityの通信がWindowsのセキュリティでブロックされている可能性があります。

Windows Defender で、以下の２点をご確認ください。

* Windows Defenderの詳細設定
* Windows Defenderファイアウォールを介したアプリまたは機能を許可

![image](https://github.com/toppers/hakoniwa-document/assets/164193/a1391548-bb19-4bc3-bdca-234fe9cfccc2)


以下では、Unity エディタの例で説明していますが、Unityアプリケーション(.exe)の場合は、対象は exe になります。

## Windows Defenderの詳細設定

受信の規則を参照して、下図のように Unity Editor が赤になっている場合は、ブロックされています。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/c9508004-bf11-49f2-9720-9ff430bab95a)

プロパティを表示して許可してください。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/692dd3ad-3c7d-46d7-b24c-418e09f1d318)

## Windows Defenderファイアウォールを介したアプリまたは機能を許可

「許可されたアプリおよび機能(A):」を参照して、Unity Editor が下図のように、チェック入っていない場合は、ブロックされています。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/1a5d1e5a-015a-4948-bef4-50c6979d80e1)

設定変更して、チェックを入れてください。
