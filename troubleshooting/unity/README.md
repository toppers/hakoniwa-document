# Unity起動時にNewtonsoft.Jsonがないというエラーが出る

Unity起動時に `Newtonsoft.Json` がないというエラーが出る場合があります。

例：

![image](https://github.com/toppers/hakoniwa-document/assets/164193/4ef5c6ed-d5a7-4455-9e1f-c91cd38974f1)


原因は、Newtonsoft.Json が不足しているためです。 

Unityのパッケージマネージャから Newtonsoft.Jsonをインストールすることで解消できます。

`Window` -> `Package Manger`を開き、下図のように、`+` をクリックし、`Add pacakge from git URL...` をクリックします。
`com.unity.nuget.newtonsoft-json` を入力して、`Add` をクリックすると、インストールが始まり、エラーが解消されます。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/2abb0ddc-c5ce-4e63-9288-98615ff92ed6)

# gRPC のライブラリ利用箇所がエラー出力している


Unityエディタのメニューの `Edit` -> `Project Settings` -> `Player` -> `Other Settings` -> `Script Compilation` を開きます。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/6c657358-a798-41e8-8ea4-460d5c5fb73e)


上図のように、`Scripting Define Symbols` に `NO_USE_GRPC` を追加して `Apply` します。

また、Unityアプリを使用後にそのままUnityエディタを利用すると同じように gRPC のライブラリ利用箇所でエラー出力される場合があります。

その場合は、 [Unityエディタを利用する場合のインストール方法](https://github.com/toppers/hakoniwa-unity-drone-model/blob/main/README-ja.md#unity%E3%82%A8%E3%83%87%E3%82%A3%E3%82%BF%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95) の
手順でインストールすることでエラー出力が消えます。
