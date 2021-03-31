---
layout: post
title: "アマチュア無線でのPPS対応PD電源の活用"
date: 2021-03-28 09:30
description: Power Delivery PPS対応電源を使うことで任意の電圧をモバイルバッテリーから生成して運用に利用する
categories: [Tips]
author: JJ1BBY
---
![発発](https://user-images.githubusercontent.com/79028771/112736878-660f9e80-8f99-11eb-9a71-7b9f2cb3a586.jpg)

## TL;DR
* 発発の給油口の蓋にパイプを通して、携行缶内の燃料を自動供給するというシステムもヘビーデューティー仕様では必須
* 1200MHz 1W移動運用だと、13.8V 2Aもあれば十分IC-9700が稼働します。モバイルバッテリーにPDで十分!
* 9/12/15/20Vを生成するトリガーケーブルを作って運用。
* 無線機は15V(16V)くらい定格上限だったり明記されているのでまぁ安心？
* でも、12Vではちょっと低い、15Vではちょっと高い。やっぱり13.8Vがほしい。DCDCかませなくても、ロスなく取り出せるとは!!!
* トランスバーター用電源にPDを使いたいが、15V通すのは気が引ける。
* [USB Power Delivery](https://www.usb.org/document-library/usb-power-delivery) Programmable power supply (PPS)で任意電圧が取り出せるらしい。
* でもまだPPS対応機器は限定。やや人柱な結果に...   

発発で24時間、HF 50Wでブリブリコンテストに参加するシステムを組んでいましたが、1200MHzに上がってからはめっきりダウンサイジング。  

PD対応のモバイルバッテリーあたりで、IC-9700も駆動できちゃいます。  

6時間級のコンテストでハンディ機ならこのシステム（バッテリ2台）でなんとか行けます。  

今回は、ここにPPSという任意電圧を取り出す拡張機能を使って13.8Vを取り出してみるという記事です。  


### USB Power Delivery PPS
![PD](https://user-images.githubusercontent.com/79028771/112736872-598b4600-8f99-11eb-92c8-6e1fb4d866c7.png)
PPS = Programmable power supply ということで、PDで任意の電圧を設定、取り出すことができる規格です。  

固定の9, 12, 15, 20Vを取り出すトリガーケーブルが最近ようやく知名度を獲得してきて、利用を始めたOMも多いようです。  

山行ではコンパクトでマルチソースなモバイルバッテリーが大変便利ですね。しばらく前からトリガー用小型基板を数百円でAliexpressから購入して愛用しています。  

ノイズ等気になったことはありません。  


### USB PDシステムで必要なもの
* PD対応モバイルバッテリー。12/15V 3A, 20V 5Aまであるといいですが、20V3Aとかでも十分ですね。  
* PD対応ケーブル。 20V5A対応を謳うものがあれば万全。なくても、まぁいけるでしょう。  
* PDトリガー。ケーブル内蔵品もあります。PDでは電源側(Source)と負荷側(Sink)で通信を常に行い、電圧のネゴシエーション等を処理しています。Sink側の通信を担う回路が必要です。  
* DCプラグ。PDトリガー内蔵で、負荷側がDCプラグ5525付きのケーブルも売っている。  
* USBテスター。 神経質な人はそろえておくと、抵抗値とか、対応状況が可視化できます。  

Aliexpressなどで、自作用[PDトリガー](https://ja.aliexpress.com/item/4000460866867.html?spm=a2g0o.productlist.0.0.676754f50iC13k&algo_pvid=e1391d28-af44-468a-8f44-e2e7bd828006&algo_expid=e1391d28-af44-468a-8f44-e2e7bd828006-5&btsid=0b0a556b16168862676057846ec7c0&ws_ab_test=searchweb0_0,searchweb201602_,searchweb201603_)基板を入手しましょう。簡単な工作です。
![PD Trigger](https://user-images.githubusercontent.com/79028771/112738165-b1c74580-8fa3-11eb-995c-d3492dd9b51a.JPG)  

電圧をジャンパで設定するので、+-と電圧は事前に確認しましょう。  
![自作ケーブル](https://user-images.githubusercontent.com/79028771/112738162-af64eb80-8fa3-11eb-8228-ffb139d21372.jpg)


面倒な人はお高い市販品、ケーブル一体型などがお手軽です。


### DCプラグのシステム統一を考えておく
DCプラグ5.5mm外径2.1mm内径センタープラスがデファクト(いわゆる5521))ですが、5525や5521/25両用、DG-G7/TH-D74/ID-31, 51など3.4x1.4、VX-3 2.35×0.7、FT-817/818N/VX-8 4.0×1.7、IC-705 5.5x2.5などのDCプラグを5521と変換するプラグは持っておいたほうがいいです。  

![APP](https://user-images.githubusercontent.com/79028771/112737542-a0c80580-8f9e-11eb-9d3e-f67bd0fe0396.png)
大電流を流すには少し心配なDCプラグなので、[Anderson Power Pole](https://www.andersonpower.com/us/en/resources/PowerPoleResourcesPage.html)でシステムを組む方が多いですね。私もAPPへ移行しました。レガシーというが、機器側がDCなので、DCプラグも併用しています。  


### PPS用ケーブル
![PPS Cable](https://user-images.githubusercontent.com/79028771/112736913-c1da2780-8f99-11eb-8da3-95d13ce50cdd.jpg)
USB A - USB Cのドングル（電圧設定時にPCに接続して利用）とType Cケーブルのセットを入手。  
私の買った[ケーブル](https://ja.aliexpress.com/item/4000382210098.html?spm=a2g0s.9042311.0.0.3c544c4dCqFbdf)は機器側の7つの設定を確認して設定する仕様のせいか、機器側が７つ以上設定があると肝心の任意電圧を設定する設定がUIにも現れず、結局設定できませんでした。手持ちのPPS対応電源では13.8Vが設定できました。  
![USB Power PD](https://user-images.githubusercontent.com/79028771/112736914-c3a3eb00-8f99-11eb-8aba-744ba6132e99.jpg)
![13.8V](https://user-images.githubusercontent.com/79028771/112736917-c56dae80-8f99-11eb-8075-8c87927b41c0.jpg)


ちょっと面倒に思える、手順です。
1. USBドングルとケーブルを接続しておいて、PCにUSB Aを差し込む
2. 設定アプリ(witrn upgrade 3.3)で設定ファイルを読み込む  
![witrn upgrade 3.3](https://user-images.githubusercontent.com/79028771/112737768-4760d600-8fa0-11eb-99b7-cb58b1bfa9ac.png)  
3. 任意にするには"PDC002アダプタ任意電圧選択(LED詳細)_2.0.pd1s"を読み込み、Upgradeボタンでケーブルにファームをアップロード(ダウンロード?)する。  
![upgrade](https://user-images.githubusercontent.com/79028771/112737810-8858ea80-8fa0-11eb-8aa6-c1f0176383a0.png)
4. PCからケーブルを取り外し、Type CプラグをPPSサポートしている機器につなぐ
5. ケーブルが接続された機器と通信を行い、接続機器がサポートしている電圧パターンを理解
ここでは電圧は設定されません!
6. 再度ケーブルをUSBドングルに差し込み、PCに接続し、設定アプリで電圧の設定を行う。  
![電圧設定](https://user-images.githubusercontent.com/79028771/112737853-07e6b980-8fa1-11eb-842c-4012bd91ec78.png)
7. Config Unitのドロップダウンで固定電圧（ここでは15V 3A）か、任意電圧を選び（横のテキストボックスで任意電圧を0.02の倍数で指定）、Configボタンを押す
8. 再度機器に接続すると、設定電圧が出力されているはず。電圧計で確認！
![15V](https://user-images.githubusercontent.com/79028771/112736916-c43c8180-8f99-11eb-9fce-7ff9f31f67a3.jpg)  

別の機器につなぐと5Vしかでない安全設計。  

設定ソフトのダウンロードは怪しさ満載な以下サイトから自己責任で。
Firmwareダウンロード（本家）  
http://www.witrn.com/witrn/upd/PDC002_software.rar  
Firmwareダウンロード（コピー）  
https://www.amazon.co.jp/clouddrive/share/fbZQyaMTDO3hx8Ir1vFUtjWiucwZW7PzgpfM3IqHsTV  


### PD PPS対応モバイルバッテリー
PD PPS対応を謳うモバイルバッテリーはまだ数が少ないです。見つけられたのは一つだけ。
SMARTCOBY20000 60W対応。  
![Mobile Battery PD all](https://user-images.githubusercontent.com/79028771/112736919-c69edb80-8f99-11eb-9c67-2ace6bbb34ca.jpg)
ただし、前述したようにPPSケーブルとの組み合わせでは残念ながら任意電圧を設定できませんでした。  

これは、バッテリー側の問題ではなくケーブル側の問題だと思われます。  

ケーブルも他に見当たりませんが、検索してみたところPPS対応のトリガー基板はいくつか世にあるようです。  

### PD PPS対応トリガー
![PD Micro](https://user-images.githubusercontent.com/79028771/112738284-dff95500-8fa4-11eb-9f22-80e42a37b058.png)
何とか任意電圧を出せないものか。[PD Micro](https://www.crowdsupply.com/ryan-ma/pd-micro)という、Open Source & Hardwarreのプロジェクトがあり、お望みにかつArduino付きで妄想が広がりそうです。  

送料がやや高い$18なのと、送付が6月後半だけど、ポチっときました。着きましたら、また結果をアップデートいたします。  


### Aliexpressでお買い物
さて、今回もいくつか紹介したパーツAliexpressさん経由で購入しています。  

最近送料値上げもありますが、いつ到着するか気にしない、安めのお買い物にはAliexpressがワクワクします。  

100回に一回くらい、何事か起こります。これまで、428回購入していますが、さしたるトラブルはありません。  

たまーに故障、入り数が少ない、３か月たっても届かない（でも後から来ることが多い）等々も含めてのお買い物体験。返金保証なので心配なし。売主には買主が届いたと申告しないとお金が渡らないシステムです。  

日本の通販にもクレカ番号入れない！セキュアな方にはおすすめできませんが。  

まだAliexpressに登録してない方は、こちらの[紹介コード](https://a.aliexpress.com/_mtFaoNp)を使うと$24 USD相当のクーポンがもらえるそうです。私に紹介料$5 USDクーポンもいただけるそう。よろしかったら、どうぞ。


---

ご紹介した商品
[CIO SMARTCOBY 20000](https://www.amazon.co.jp/gp/product/B08CHK2YCC/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=B08CHK2YCC&linkCode=as2&tag=jj1bby-22&linkId=eb5a9f7f2074fe40e5e08747d0c334d3)  

---
<script src="https://utteranc.es/client.js"
        repo="JJ1BBY/JJ1BBY.github.io"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
