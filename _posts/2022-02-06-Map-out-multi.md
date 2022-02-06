---
layout: post
title: "マルチ獲得地図"
date: 2022-02-05 13:30
categories: [Contest]
description: csvにしたコンテストログファイルを白地図に塗りつぶした画像ファイルを作成します。
author: JJ1BBY
---
## TL;DR
* csvにしたコンテストログファイルを白地図に塗りつぶした画像ファイルを作成します。
* 関東UHFとACAGに対応
* ロカコンの特殊なマルチ番号やJCGより細かい番号、管外は都道府県レベルのマルチ番号などにまだ対応していません。
* いつものように雑な実装ですのでお手柔らかに...

探してみましたが、落ちてなかったので作ってみたシリーズ。  
[ソースコード](https://github.com/JJ1BBY/multimap)

読み込むファイルは ctestwinでファイル出力 - その他ファイル出力 - TXT形式ファイル出力 で作成したものを使ってください。log.txtかlogfileを設定してください。  
レポジトリにあるのはサンプルコードですので、必要に応じて変更してください。  

白地図のデータを https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-N03-v3_0.html#prefecture00 からあらかじめダウンロードしてローカルに展開し、指定してください。  
例
"D:\Downloads\N03-20210101_GML\N03-20210101_GML\N03-21_210101.shp" 

JCG等の番号一覧のデータを
https://github.com/w-ockham/JCC-JCG-List/blob/master/munitable.csv （w-ockham/JCC-JCG-List is licensed under the Apache License 2.0）から利用しました。ありがとうございます。

その他コメントを見てください。緯度経度指定して描画エリアを変更できます。塗りつぶしの色なども変更できます。

##使い方  
<code>
python mapout.py  
   
出力は multimap.pngおよびmultimap.csv になります。
</code>
---

   
<script src="https://utteranc.es/client.js"
        repo="JJ1BBY/JJ1BBY.github.io"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>

