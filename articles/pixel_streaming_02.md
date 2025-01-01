---
title: "【UE5】Pixel Streaming を使ってみる"
emoji: "🎏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["UnrealEngine5", "PixelStreaming"]
published: true
---

Unreal Engine 5 の Pixel Streamingの記事を書きます。

# Pixel Stremaing とは

前回の記事を参考にしてください。

[【UE5】Pixel Streaming とは？](https://zenn.dev/tkmkik/articles/pixel_streaming_01)

## 環境

以下の環境で動かしていきます。

- windows 11
- UE 5.3.2
- Pixel Streaming Infrastructure
    - [EpicGamesExt/PixelStreamingInfrastructure](https://github.com/EpicGamesExt/PixelStreamingInfrastructure)
- node@22

## 環境構築

### UEでThird Personを作る

UEを起動しThird Personのテンプレートを起動します。

![](/images/pixelstreaming2-1.png)

### Pixel Streamingプラグインを導入する

左上のメニュバーから　「編集」＞「プラグイン」を開き、検索窓に「PixelStreaming」と入力して

- Pixel Streaming
- Pixel Streaming Playerを導入する

![](/images/pixelstreaming2-2.png)

### UE EditorからPixelStreamingのサーバーを起動する方法

再生ボタンなどがあるバーに、Pixel Streaming Playerのメニューが追加されました。

「シグナリングサーバーを起動」を押下すると、「Pixel Streaming Infrastructure」の中のシグナリングサーバーと同等のものが起動します。

![](/images/pixelstreaming2-3.png)

「レベルエディタをストリーミング」を押下するとレベルエディタが配信されます。「Pixel Streaming Infrastructure」の中のシグナリングサーバーと似た環境を用意できます。

![](/images/pixelstreaming2-4.png)

フルエディタを配信すると開発環境毎配信できます。

今回は

- 「シグナリングサーバーを起動」
- 「レベルエディタをストリーミング」

をクリックして起動してください。

### ブラウザからUEを使う

任意のブラウザから、事前に設定したアドレスへアクセスしてください。特に変更がなければ「http://localhost:80」

![](/images/pixelstreaming2-5.png)

上記のような画面がでていれば「シグナリングサーバー」は起動成功です。

### UEからアプリを起動

UE側でゲームを再生してください。するとブラウザの画面はUnrealのレベルエディタと同じく再生画面になっており、ブラウザからゲームを体験できます。

![](/images/pixelstreaming2-6.png)

ひとまずは完成です。リリースを考えるとこの方法は使えないのでPixel Streaming Infrastructureを使う方法を書きます。

### Pixel Streaming InfrastructureでPixelStreamingをする方法

1. GitHubから自分のUEバージョンに合わせたブランチを選択する
    - UE5.3:　https://github.com/EpicGamesExt/PixelStreamingInfrastructure/tree/UE5.3 

2. これをDL、zip解答をする
3. `PixelStreamingInfrastructure-UE5.3\PixelStreamingInfrastructure-UE5.3\SignallingWebServer\platform_scripts\cmd`このパスを開く
4. Powershellで以下コマンドを実行する

    ```sh
    setup.bat
    ```

5. `Start_SignallingServer.ps1`を実行する

このとき次のようなエラーが出る場合はPS1ファイルの実行権限がない。

```
.\Start_SignallingServer.ps1 : ファイル C:\Users\username\Documents\UE\PixelStreamingInfrastructure-UE5.3\PixelStreami
ngInfrastructure-UE5.3\SignallingWebServer\platform_scripts\cmd\Start_SignallingServer.ps1 を読み込めません。ファイル C
:\Users\username\Documents\UE\PixelStreamingInfrastructure-UE5.3\PixelStreamingInfrastructure-UE5.3\SignallingWebServe
r\platform_scripts\cmd\Start_SignallingServer.ps1 はデジタル署名されていません。このスクリプトは現在のシステムでは実行
できません。スクリプトの実行および実行ポリシーの設定の詳細については、「about_Execution_Policies」(https://go.microsoft
.com/fwlink/?LinkID=135170) を参照してください。
発生場所 行:1 文字:1
+ .\Start_SignallingServer.ps1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : セキュリティ エラー: (: ) []、PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

その場合は以下コマンドで権限ポリシーを変更すること

```
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass 
```

再度実行して

```
09:42:20.429 peerConnectionOptions = {"iceServers":[{"urls":["stun:stun.l.google.com:19302"]}]}
09:42:20.484 Running Cirrus - The Pixel Streaming reference implementation signalling server for Unreal Engine 5.3.
09:42:20.879 WebSocket listening for Streamer connections on :8888
09:42:20.880 WebSocket listening for SFU connections on :8889
09:42:20.881 WebSocket listening for Players connections on :80
09:42:20.882 Http listening on *: 80
```

このようならログが出れば成功です。

### ビルド・パッケージ化したexeとシグナリングサーバーを紐づける

最後の手順です。パッケージ化したUEとシグナリングサーバーを紐づけて起動します。

「プラットフォーム」＞「Windows」＞「プロジェクトをパッケージ化」を選択し任意の場所を選択し実行します。

exe化が完了したら、exeのショートカットを作成します。

次にプロパティを開きます。リンクに次のコマンドを含めます。`-AudioMixer -PixelStreamingIP=localhost -PixelStreamingPort=8888`

![](/images/pixelstreaming2-7.png)

ここまで完了したらショートカット経由でUEアプリを実行します。

あらためてブラウザから http://localhost を開きます。

![](/images/pixelstreaming2-8.png)

上のように表示されれば、exeとシグナリングサーバーの紐づけを完了できました。


## 最後に

PixelStreamingの環境構築はさほど難しくありません。同じような手順を行えばEC2のWindowsインスタンスや、Linuxインスタンスでも環境構築が可能です。

## 参考文献

https://dev.epicgames.com/documentation/ja-jp/unreal-engine/getting-started-with-pixel-streaming-in-unreal-engine