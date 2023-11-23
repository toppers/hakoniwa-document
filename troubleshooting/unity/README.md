# Unity起動時にNewtonsoft.Jsonがないというエラーが出る

Unity起動時に `Newtonsoft.Json` がないというエラーが出る場合があります。

例：

![image](https://github.com/toppers/hakoniwa-document/assets/164193/4ef5c6ed-d5a7-4455-9e1f-c91cd38974f1)


原因は、Newtonsoft.Json が不足しているためです。 

Unityのパッケージマネージャから Newtonsoft.Jsonをインストールすることで解消できます。

`Window` -> `Package Manger`を開き、下図のように、`+` をクリックし、`Add pacakge from git URL...` をクリックします。
`com.unity.nuget.newtonsoft-json` を入力して、`Add` をクリックすると、インストールが始まり、エラーが解消されます。

![image](https://github.com/toppers/hakoniwa-document/assets/164193/2abb0ddc-c5ce-4e63-9288-98615ff92ed6)
