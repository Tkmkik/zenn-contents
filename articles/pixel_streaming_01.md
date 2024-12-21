---
title: "【UE5】Pixel Streaming とは？"
emoji: "🎏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["UnrealEngine5", "PixelStreaming"]
published: true
---

Unreal Engine 5 の Pixel Streamingの記事を書きます。できる限り自分の言葉で書いていきたいです。図も自分で起こしていきたいと考えています。

# Pixel Stremaing とは

Pixel Streamingとはクラウドのサーバーで Unreal Engine アプリケーションを実行し、レンダリングしたフレームと音声を WebRTC経由でブラウザやモバイルデバイスにストリーミングできる機構です。

![](/images/pixelstreaming.jpg)

## メリット

### 利用者にGPUコストがかからない

クラウド側でレンダリングを実行するのでクライアントは通信するだけでUnrealEngineアプリを体験できます。

### 利用者がブラウザのみですぐにコンテンツを体験できる

ブラウザからコンテンツにアクセスするだけで、すぐに体験が開始できます。ユーザにアプリのダウンロードやインストールをしていただく必要がありません。

## デメリット

### 通信料がかかる

映像等々を配信するため、それなりの通信負荷がかかります。有線やWifiへの接続が求められます。

### 従量課金制クラウドを利用する場合はさらにコストがかかる

EC2などの利用料＋クラウドから外部に投げる映像の通信料がそれなりに負荷があるのでそれなりの通信コストがかかります。

## できること

※調査中です。わかり次第追記していきたいと思っています。

- 一人用アプリをPixel Streamingで配信して、一人で体験する。
    - PixelStreaming Serverのみで動く
- マルチプレイアプリをPixel Streamingで配信して、複数人で体験する
    - MatchMakerを併用して利用する

## できないこと

※調査中です。わかり次第追記していきたいと思っています。

- ボイスチャットは現状できなさそうです。
    - 別途ブラウザ組み込みのボイスチャット機能が必要そうです。
    - [PixelStreamingAudioComponent](https://dev.epicgames.com/documentation/en-us/unreal-engine/python-api/class/PixelStreamingAudioComponent?application_version=5.0)というコンポーネントを使うことでブラウザの音声を取得しゲーム内で再生できます。クオリティが低いのか設定が悪いのか、音がバリバリになってしまいます。

## 終わりに

次の記事でサンプルを動かしていこうと思います。

とりあえず記事を書いていきたいという気持ちだけで書いています。間違いがあればご指摘をお願いします。修正してもっといい記事として残していきたいです。


# 参考

- [公式Doc](https://dev.epicgames.com/documentation/ja-jp/unreal-engine/pixel-streaming-in-unreal-engine)はこちらです。
- [PixelStreamingAudioComponent](https://dev.epicgames.com/documentation/en-us/unreal-engine/python-api/class/PixelStreamingAudioComponent?application_version=5.0)
- [Use Microphone/Pixel Streaming Audio Component - YouTube](https://www.youtube.com/watch?v=YC5ZiZ-PSVA)