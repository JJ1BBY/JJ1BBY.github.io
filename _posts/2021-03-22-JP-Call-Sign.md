---
layout: post
title: "日本の呼出符号 - 正規表現"
date: 2021-03-13 09:30
description: 正規表現を用いて、日本の呼出符号を検索するには？
categories: [Tips]
author: JJ1BBY
---

## TL;DR
* 正規表現 `^J[A-S]|^[7-8][J-N]`  
* Microsoft OfficeはOffice Open XML ISO/IEC 29500という国際規格になってます。Excelでは標準でない関数だが、Google DogsではSpreadsheetで=REGEXMATCH()という関数を拡張子、正規表現に一致するか否かを返すことができる。  
* というわけで、コールサインのデータがあれば、日本の[コールサイン](https://www.itu.int/en/ITU-R/terrestrial/fmd/Pages/call_sign_series.aspx)は =REGEXMATCH(A1,"`^J[A-S]|^[7-8][J-N]`")などで、TRUE or FALSEで判定されます。  

とりあえず、LoTWのユニークな局から日本局を探してみる。  

[データ](http://www.hb9bza.net/lotw-users-list)はHB9BZA局が編纂されているものを用いた。最終コールサインとLoTWへの最終アップロード日付が記載されている。  
149507局中日本の局は6333局、4%ということになる。

正規表現はどっかに落ちてるかと思ったが、なかったので、作ってみた。というだけの、備忘録のポストです。

## Explicitな正規表現
MJH局より、以下条件を加えた正規表現をいただきましたので、追記します。ありがとうございました。
* JDは1のみ
* 2文字コールはJAとJRのみ
* 8J<数字>の後は任意の数の英数文字

`^J[AE-S][0-9][A-Z]{3}|J[AR][0-9][A-Z]{2}|JD1[A-Z]{3}|7[J-N][0-9][A-Z]{3}|8[J-N][0-9][0-9A-Z]+$`

---

<script src="https://utteranc.es/client.js"
        repo="JJ1BBY/JJ1BBY.github.io"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>


