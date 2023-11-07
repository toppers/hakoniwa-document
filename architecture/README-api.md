コントリビュート募集中！

以下の説明が必要と考えています。

* C/C++ 版の箱庭コア機能のAPI概要説明
  * [master API](https://github.com/toppers/hakoniwa-core-cpp/blob/main/src/include/hako_master.hpp)
  * [asset API](https://github.com/toppers/hakoniwa-core-cpp/blob/main/src/include/hako_asset.hpp)
  * [simevent API](https://github.com/toppers/hakoniwa-core-cpp/blob/main/src/include/hako_simevent.hpp)
* [gRPCのAPI](https://github.com/toppers/hakoniwa-core-spec/blob/main/hakoniwa_core.proto)

また、gRPCについては、通信シーケンスの説明が必要と考えています。

# 状態遷移仕様

## 全体像

![image](https://github.com/toppers/hakoniwa-document/assets/164193/df33f200-379c-4b3f-adc6-d2252ce3cd2e)

|状態|説明|
|---|---|
|STOPPED|初期状態であり、シミュレーションが停止ている状態|
|RUNNABLE|シミュレーション開始可能な状態|
|RUNNING|シミュレーション実行中状態|
|STOPPING|シミュレーション停止中状態|
|RESETTING|シミュレーションリセット中状態|
|ERROR|シミュレーションエラーが発生した状態|

|イベント|説明|
|---|---|
|start|シミュレーション開始イベント|
|stop|シミュレーション停止イベント|
|reset|シミュレーションリセットイベント|
|start_feedback|箱庭アセットへのシミュレーション開始通知に対するフィードバック（OK/NG）|
|stop_feedback|箱庭アセットへのシミュレーション停止通知に対するフィードバック（OK/NG）|
|reset_feedback|箱庭アセットへのシミュレーションリセット通知に対するフィードバック（OK/NG）|

|アクション|説明|
|---|---|
|allocate PDU memory|PDUメモリを共有メモリとして確保し、初期化する|
|free PDU memory simulation time = 0|PDUメモリを解放し、シミュレーション時間を０に戻す|


## RUNNING中の状態遷移

![image](https://github.com/toppers/hakoniwa-document/assets/164193/75be4767-792f-470c-9bc1-dcd501af7a91)


# シミュレーション開始シーケンス

![image](https://github.com/toppers/hakoniwa-document/assets/164193/dfbf981f-8ebb-4011-9820-41b67c5635ff)
