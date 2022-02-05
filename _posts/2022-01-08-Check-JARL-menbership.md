---
layout: post
title: "JARL会員情報検索を自動化"
date: 2022-01-08 13:05
description: JARL会員情報検索をPythonで自動化し、QSLカード差出コールサインが転送されるか確認できるようにしてみた。HamlogからExcelに出力し、ExcelからCSVにしたデータを使っています。
categories: [Tips]
author: JJ1BBY
---
## TL;DR
* ython, selenium, pandasを利用。
* HamlogからExcel->CSVと出力したファイルを読み込み、コールサインを取得して検索し、QSL欄に書き戻す
* 存在しうるコールサインか確認した列など、いくつか追加しているので、不要なら削除してください。

## 新ソースコード
'[qslcheck2.py](https://jj1bby.com/qslcheck2.py)'
重複するコールサインを除いてユニークなコールサインだけをJARLで検索し、その結果を展開するように変更。

```python
import pathlib

#https://chromedriver.chromium.org/getting-started をダウンロードし、chromedriver.exe をソースコードのディレクトリに配置
from selenium import webdriver
from selenium.webdriver.chrome.service import Service 
from selenium.webdriver.common.by import By

import pandas as pd


#JARL 会員サイトURL
JARL = "https://www.jarl.com/Page/Search/MemberSearch.aspx?Language=Jp"
#読み込むHamlogのCSVファイル名 実際にはHamlogの検索-複合検索条件と印刷メニューでExcel形式で書き出し、ExcelからUTF-8のCSVで保存する必要がある
qsl = "LogList.csv"
#最終的に出力するCSVファイル名
csvout ="qslchecked.csv"

#CSVファイル読み込み
df_code = pd.read_csv(
    qsl,
    #debug用に読み込む列を少なく設定
    #nrows=40,
    index_col=0
  )

# 移動局を/で分割し、格納
df_cs =  df_code['Call'].str.split('/', expand=True)
#rename
df_cs.columns = ['CS_only', 'Portable']
# CSをテストして結果をfCSへ格納。存在しないCSならばFalse
df_cs['fCS'] = df_cs['CS_only'].str.match('^J[AE-S][0-9][A-Z]{3}|J[AR][0-9][A-Z]{2}|JD1[A-Z]{3}|7[J-N][0-9][A-Z]{3}|8[J-N][0-9][0-9A-Z]+$')
# df_listに元のdf_codeとdf_csを結合
df_list = pd.concat([df_code, df_cs], axis='columns') 

#重複したCSを除くユニークなコールサインのDFをつくり、それだけをJARLのWebで問い合わせる
CallSign = list(df_list['CS_only'][~df_list.duplicated(subset='CS_only')])

#JARL会員検索
s = Service ('chromedriver')
browser = webdriver.Chrome(service=s)
browser.get(JARL)
browser.maximize_window()

jarl_res = []
for m in CallSign:
    txtCallSign = browser.find_element(by=By.ID, value = "txtCallSign")
    txtCallSign.send_keys(m)
    btnSearch = browser.find_element(by=By.ID, value = "btnSearch")
    btnSearch.click()
    td = browser.find_element(by=By.ID, value = "ListView1_lblResult_0")
    jarl_res.append(td.text)
#問い合わせ結果のDF
df_isJARL = pd.DataFrame(jarl_res, columns = ['isJARL'])
#対応するコールサインのDFを結合
df_isJARL = pd.concat([df_isJARL, pd.DataFrame(CallSign, columns = ['CallSign'])], axis=1)
browser.quit()

#マスターDFにJARL会員情報を格納してdfを結合
df_list = df_list.reset_index()
#ユニークなコールサインDFをマスターDFでマッチさせて展開
df_list.insert(len(df_list.columns), 'isJARL', df_list['CS_only'].map(df_isJARL.set_index('CallSign')['isJARL']))

#QSL列の値をJARL会員情報の検索結果から書き換える
#QSL J yes, N no qsl, S SASE - non jarl 自分の使っている記号に合わせて下さい。
#○ Yes via等の対応は入れていません。
df_list['QSL'] = df_list['QSL'].where(df_list['isJARL'] != "×", "S")
df_list['QSL'] = df_list['QSL'].where(df_list['isJARL'] != "○ NO", "N")

#Debug          
#print(df_list)
#print(df_list.columns.values)

#Debug Data check
#print('dataframeの行数・列数の確認==>\n', df_code.shape)
#print('indexの確認==>\n', df_code.index)
#print('columnの確認==>\n', df_code.columns)
#print('dataframeの各列のデータ型を確認==>\n', df_code.dtypes)
#print(df_list)
#print (len(df_list['CS_only']), "includes ", df_list['CS_only'].nunique(), "unique CSs")


#csvファイルに書き込み
df_list.to_csv(
        pathlib.Path("", csvout),
        #index=False,
        #quoting=csv.QUOTE_NONNUMERIC,
        encoding="utf_8_sig"
)

```
## 旧ソースコード
'[qslcheck.py](https://jj1bby.com/qslcheck.py)'

```python
import pathlib

#https://chromedriver.chromium.org/getting-started をダウンロードし、chromedriver.exe をソースコードのディレクトリに配置
from selenium import webdriver
from selenium.webdriver.chrome.service import Service 
from selenium.webdriver.common.by import By

import pandas as pd


#JARL 会員サイトURL
JARL = "https://www.jarl.com/Page/Search/MemberSearch.aspx?Language=Jp"
#読み込むHamlogのCSVファイル名 実際にはHamlogの検索-複合検索条件と印刷メニューでExcel形式で書き出し、ExcelからUTF-8のCSVで保存する必要がある
qsl = "LogList.csv"
#最終的に出力するCSVファイル名
csvout ="qslchecked.csv"

#CSVファイル読み込み
df_code = pd.read_csv(
    qsl,
    index_col=0
  )

# 移動局を/で分割し、格納
df_cs =  df_code['Call'].str.split('/', expand=True)
#rename
df_cs.columns = ['CS_only', 'Portable']
# CSをテストして結果をfCSへ格納。存在しないCSならばFalse
df_cs['fCS'] = df_cs['CS_only'].str.match('^J[AE-S][0-9][A-Z]{3}|J[AR][0-9][A-Z]{2}|JD1[A-Z]{3}|7[J-N][0-9][A-Z]{3}|8[J-N][0-9][0-9A-Z]+$')
# df_listに元のdf_codeとdf_csを結合
df_list = pd.concat([df_code, df_cs], axis='columns') 

#Debug Data check
#print('dataframeの行数・列数の確認==>\n', df_code.shape)
#print('indexの確認==>\n', df_code.index)
#print('columnの確認==>\n', df_code.columns)
#print('dataframeの各列のデータ型を確認==>\n', df_code.dtypes)
#print(df_list)

CallSign = list(df_list['CS_only'])

#JARL会員検索
s = Service ('chromedriver')
browser = webdriver.Chrome(service=s)
browser.get(JARL)
browser.maximize_window()

jarl_res = []
for m in CallSign:
    txtCallSign = browser.find_element(by=By.ID, value = "txtCallSign")
    txtCallSign.send_keys(m)
    btnSearch = browser.find_element(by=By.ID, value = "btnSearch")
    btnSearch.click()
    td = browser.find_element(by=By.ID, value = "ListView1_lblResult_0")
    jarl_res.append(td.text)
df_isJARL = pd.DataFrame(jarl_res, columns = ['isJARL'])
browser.quit()

#マスターリストにJARL会員情報を格納してdfを結合
df_list = df_list.reset_index()
df_list = pd.concat([df_list, df_isJARL], axis=1)

#QSLの値をJARL会員情報の検索結果から書き換える
#QSL J yes, N no qsl, S SASE - non jarl 自分の使っている記号に合わせて下さい。
#○ Yes via等の対応は入れていません。
df_list['QSL'] = df_list['QSL'].where(df_list['isJARL'] != "×", "S")
df_list['QSL'] = df_list['QSL'].where(df_list['isJARL'] != "○ NO", "N")

#Debug          
#print(df_list)
#print(df_list.columns.values)

#csvファイルに書き込み
df_list.to_csv(
        pathlib.Path("", csvout),
        #index=False,
        #quoting=csv.QUOTE_NONNUMERIC,
        encoding="utf_8_sig"
)
```
<script src="https://utteranc.es/client.js"
        repo="JJ1BBY/JJ1BBY.github.io"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
