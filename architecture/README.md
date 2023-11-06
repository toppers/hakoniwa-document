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
