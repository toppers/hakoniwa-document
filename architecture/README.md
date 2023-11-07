# アーキテクチャ概要

箱庭のシステムアーキテクチャの高レベルなビューは下図のとおりです。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/9f6206c8-fbaa-41f2-8938-f9fe544c0958)

箱庭の中核をなす機能（箱庭コア機能）は、非常にシンプルで、以下の４つに集約されます。

* スケジューリング(Scheduling)
* 同期・通信(Synchronization & Communication)
* 時間管理(Time Management)
* アセット管理(Asset Management)

箱庭コア機能の利用者は「箱庭アセット」であり、箱庭のAPIを呼び出して箱庭の機能を呼び出しする構成です。

## 箱庭アセット

箱庭アセットの定義は、『箱庭のAPIを利用して作成された再利用可能なアプリケーション』です。

箱庭アセットの分類としては、以下のものが挙げられます。

* 被制御対象
  * ロボット、ドローン、信号機、踏切、電車、車などなど
* 制御プログラム
  * 被制御対象を制御するためのプログラムであり、様々な言語で開発されたプログラム（Python, C/C++, Rust, Ruby, Elixir,..）
* シミュレータ
  * 被制御対象向けのシミュレータ（Unity, Unreal Engine, Gazebo, PyBullet など）
  * 制御プログラム向けのシミュレータ（マイコンシミュレータAthrillなど）
* シミュレーション自動化機能
  * 箱庭のAPIには、シミュレーションを実行、停止、リセットする機能があります。
  * これらの機能を利用する、以下のような自動化機能が実現できます。
  * ロボットの強化学習（アクション→評価→再実行を繰り返す）
  * 物理シミュレーションを含めた全体結合テスト自動実行（CI/CD環境として）
* ビジュアライズ機能
  * 箱庭アセットとして、Unity等を利用すれば被制御対象であるロボット等を容易に可視化できます。
  * また、制御プログラムとの間の通信データは、箱庭の通信データ（PDU）を参照することで、データの流れを可視化できます。
  * さらに、そのデータをROS2のデータとしても可視化できるようになります。
* さらなる応用機能
  * 箱庭の上では、様々な制御プログラムを利用できますので、箱庭アセットの可能性はとても広いと考えています。
  * 特に、Pythonはその応用範囲は広く、AIとの連携が期待できます。
  * 例えば、Python上でAIエージェントを作成して、箱庭APIを使ってシミュレーション連携させるようなことも容易に実現できます。

# アーキテクチャ詳細

箱庭アーキテクチャの詳細レベルのビューは下図のとおりです。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/1729d782-791d-4b37-9b63-67bf304a4141)

1. [箱庭コア機能(Hakoniwa Core functions)](https://github.com/toppers/hakoniwa-document/blob/main/architecture/README-core.md) - 箱庭コア機能の概要を示します。
2. [箱庭コア機能のAPI(Hakoniwa API)](https://github.com/toppers/hakoniwa-document/blob/main/architecture/README-api.md) - 箱庭コア機能のAPIの概要を示します。
3. [PDU](https://github.com/toppers/hakoniwa-document/blob/main/architecture/README-pdu.md) - 箱庭アセット間のデータ交換の共通インタフェースの概要を示します。
4. [箱庭コンダクタ(Hakoniwa Conductor)](https://github.com/toppers/hakoniwa-document/blob/main/architecture/README-conductor.md) - 箱庭アセット間のシミュレーションを調停するコンポーネントの概要を示します。
5. [箱庭アセット](https://github.com/toppers/hakoniwa-document/blob/main/architecture/assets/README.md) - 代表的な箱庭アセットの概要を示します。

# ネットワーク構成とインフラストラクチャ

## ネットワークの基本構成

箱庭のネットワークの基本構成は下図のとおりです。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/94eacd63-816b-4413-abfa-a50139c5f3a0)

## インフラストラクチャの基本構成

ネットワークの基本構成図にある Computer は、以下の３パターンがあります。

* Personal Computer
* Docker Container
* Embedded Device（Raspberry PI や [mROS 2 サポート対応機器](https://github.com/mROS-base/mros2#supported-platform)など）

また、OSのパターンとしては、以下があります。

* Windows/WSL2
* MacOS(Intel)
* MacOS(AppleSilicon)
* Ubuntu

## 構成パターン

箱庭では、さまざまな構成でのシミュレーションを可能にします。

ただ、物理シミュレータとしてUnityを利用する場合は、Unityと箱庭との構成可能なパターンに制約があります。

以下、構成可能なパターンと、Unityと箱庭との間の通信方式（制御向け通信とPDU通信）をマトリクスで示します。

| OS | Computer | 制御向け通信 | PDU通信方式 |
|----------|----------|----------|----------|
| Windows/WSL2 | Docker Container | gRPC | UDP, MQTT |
| MacOS(Intel)  | Personal Computer | gRPC | UDP, MQTT |
| MacOS(Intel)  | Personal Computer | SHaredMemory | SharedMemory |
| MacOS(AppleSilicon)  | Personal Computer  | SharedMemory | SharedMemory |
| Ubuntu | Personal Computer/Docker Container | gRPC | UDP, MQTT |
| Ubuntu | Personal Computer/Docker Container | SharedMemory | SharedMemory |



## 構成適用例

箱庭の構成パターンは多種多様ですが、ここでは１つのマシンで箱庭を構成する場合の適用例を示します。

OS毎に最適な構成パターンが変わりますので、OS毎に示します。

* [Windowsの場合](https://github.com/toppers/hakoniwa-document/blob/main/architecture/examples/README-win.md)
* MacOSの場合
* Ubuntuの場合
