---
layout: post
title: "ctestwinのログファイル変換"
date: 2022-02-01 11:30:00 +0900
categories: [Contest]
description: ctestwinのコンテストログファイルをcsvファイルに変換するpythonのスクリプトです
author: JJ1BBY
---

## TL;DR
* ctestwinのログファイルをcsvファイルに変換するpythonのスクリプトです。
* 見当たらなかったので、作ってみました。
* 雑な実装ですのでお手柔らかに

探してみましたが、落ちてなかったので作ってみたシリーズ
![ソースコード](https://github.com/JJ1BBY/lg82csv/blob/main/lg82csv.py)
バイナリファイルを操作するのは初めてなので、色々と至らないところがあるかと思いますが。
とりあえず、動いているみたいなので...

##使い方
<code>
python lg82csv logfile.lg8
出力は logfile.csv になります。
</code>

---


<script src="https://utteranc.es/client.js"
        repo="JJ1BBY/JJ1BBY.github.io"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
</script>
