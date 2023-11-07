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

TODO

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
