---
title: "脱法はいふりカメラ"
date: 2019-03-02T23:12:06+09:00
categories: ["はいふり"]
tags: ["ハイスクール・フリート","はいふり"]
description: ""
banner: ""
images: []
menu: ""
draft:
---

<!--
はいふりアプリが起動する都度サーバーと通信を行っていることははいふり博士諸氏の間では周知の事実である。中でも厄介なのは、本アプリのメイン、一部ではコンテンツ全体の中核とも目されるはいふりカメラがフレームを読みこむ動作だ。電波の通じにくい高原で撮影しようものなら目も当てられないことになるのは想像に難くない。
-->

はいふりカメラのフレームをサーバから一括してダウンロードするプログラムを書きました。htmlのソースを確認して手作業で保存するのが面倒になったからです.

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">はいふりカメラ全フレームをOne Driveに保存完了した</p>&mdash; さがみの (@sagami_no) <a href="https://twitter.com/sagami_no/status/987207023589642240?ref_src=twsrc%5Etfw">2018年4月20日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

という作業を行ったのが1年ほど前で、この間に新規で追加されたフレーム、そして今後も追加されるであろうフレーム(なんとはいふりは今後ソシャゲと劇場版が控えています！)に対しても同様の作業をやりたくありませんでした。

公式に配布した画像ではありませんが、はいふりアプリ自体がWebViewなので特に問題は無いような気がします。

はいふりカメラのフレーム選択画面。ソースを見ればフレームのファイル名が確認できます。アプリ上で長押しすると低画質非透過のフレームが端末に保存される理由も分かります。
http://app.aniplex.co.jp/hai-furi/camera/


はいふりカメラの各フレームは以下のように http://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/ に保存されています。  
http://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/sp012w.png

ソースは以下の通り。

```python:haifuri_frame_dl.py
import sys
import os
import requests
import shutil

url = "http://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/"

if not os.path.exists("haifuri_frame"):
	os.makedirs("haifuri_frame")
	print("ディレクトリ " + os.path.dirname(os.path.abspath(__file__)) + "/haifuri_frameを作成しました。")

# ノーマルフレームとスペシャルフレームの名前が番号のファイルをダウンロード

for i in range(1,150):

	i_zero = str(i).zfill(3)
	frame_name_list = [i_zero+"h.png",i_zero+"w.png"]

	for frame_name in frame_name_list:

# verify=Falseで自己証明書でもスキップする
		if os.path.exists(r".\haifuri_frame/" + frame_name) == 0:
			r = requests.get(url + frame_name, stream=True, verify=False)
			if r.status_code == 200:
				with open(r".\haifuri_frame/" + frame_name , 'wb') as f:
					r.raw.decode_content = True
					shutil.copyfileobj(r.raw, f)
					print (frame_name+" をダウンロードしました。")


for i in range(1, 50):

	i_zero = str(i).zfill(3)
	frame_name_list = ["sp"+i_zero+"h.png","sp"+i_zero+"w.png",]

	for frame_name in frame_name_list:

		if os.path.exists(r".\haifuri_frame/" + frame_name) == 0:
			r = requests.get(url + frame_name, stream=True, verify=False)
			if r.status_code == 200:
				with open(r".\haifuri_frame/" + frame_name , 'wb') as f:
					r.raw.decode_content = True
					shutil.copyfileobj(r.raw, f)
					print (frame_name+" をダウンロードしました。")


# 特別な名前のファイルをダウンロード

frame_name_list = ["frame_","sp_akeno","sp_mashiro","sp_mina","harekaze_"]

for frame_name in frame_name_list:
	if os.path.exists(".\\haifuri_frame/" + frame_name + "h.png") == 0:
		r = requests.get(url+frame_name+"h.png", stream=True, verify=False)
		if r.status_code == 200:
			with open(".\\haifuri_frame/" + frame_name + "h.png", 'wb') as f:
				r.raw.decode_content = True
				shutil.copyfileobj(r.raw, f)
				print (frame_name+"h をダウンロードしました。")

	if os.path.exists(".\\haifuri_frame/" + frame_name + "w.png") == 0:
		r = requests.get(url+frame_name+"w.png", stream=True, verify=False)
		if r.status_code == 200:
			with open(".\\haifuri_frame/" + frame_name + "w.png", 'wb') as f:
				r.raw.decode_content = True
				shutil.copyfileobj(r.raw, f)
				print (frame_name+"w をダウンロードしました。")
```