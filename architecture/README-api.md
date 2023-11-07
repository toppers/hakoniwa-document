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

箱庭の基本的な状態遷移は下図の通りです。

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

シミュレーション開始し、箱庭の状態がRUNNINGになった場合でも、各箱庭アセットはまだPDUを書き込みしていない可能性があります。この場合、未書き込みのPDUデータを他の箱庭アセットが誤って読んでしまう可能性があるため、下図に示す状態遷移をさせています。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/75be4767-792f-470c-9bc1-dcd501af7a91)


|状態|説明|
|---|---|
|NOT_SIMULATION_MODE|箱庭アセットが自分が書き込むべきPDUデータを１度も書き込んでいない状態|
|SIMULATION_MODE|箱庭アセットが最低１回は自分のPDUデータを書き込んでいる状態|

|イベント|説明|
|---|---|
|All assets have written PDU data|全箱庭アセットがPDUデータを最低１回は書き込んだ|


# シミュレーション開始シーケンス

箱庭のイベントとしてシミュレーション開始するトリガ(HakoniwaSImEvent)が発生した場合に、箱庭アセット(HakoniwaAsset)と箱庭コア機能(HakoniwaCore)間で行われる通信シーケンスは下図の通りです。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/dfbf981f-8ebb-4011-9820-41b67c5635ff)
