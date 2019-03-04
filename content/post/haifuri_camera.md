---
title: "脱法はいふりカメラ"
date: 2019-03-02T23:12:06+09:00
categories: ["アニメ"]
tags: ["ハイスクール・フリート"]
description: "" 
banner: "banners/haifuri_OGimage_20181213.png"
images: ["https://sagamino.netlify.com/banners/haifuri_OGimage_20181213.png"]
menu: ""
draft:
---
はいふりカメラのフレームをサーバから一括してダウンロードするプログラムを書きました。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr"> Driveに保存完了した</p>&mdash; さがみの (@sagami_no) <a href="https://twitter.com/sagami_no/status/987207023589642240?ref_src=twsrc%5Etfw">2018年4月20日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

という作業を行ったのが1年ほど前で、この間に新規で追加されたフレーム、そして今後も追加されるであろうフレーム(なんとはいふりは今後ソシャゲと劇場版が控えています！)に対してもhtmlのソースを確認して手作業で保存するのが面倒になったからです。

<!--more-->

コードを公開するにあたって、公式に配布した画像ではないという懸念がありますが、はいふりアプリ自体がWebViewなので特に問題は無いような気がします。
<br/>
<br/>

**はいふりカメラについて**  
http://app.aniplex.co.jp/hai-furi/camera/  
これは、はいふりカメラのフレーム選択画面。ソースで各フレームのファイル名が確認できます。アプリ上で長押しすると低画質非透過のフレームが端末に保存される仕組みも何となく分かりますが、これは意図して実装した機能では無いような気がします。


実際のはいふりカメラのフレームは  
http://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/sp012w.png  
のように"http\://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/"に保存されています。
<br/>
<br/>

**数あるフレームの構成**  
はいふりカメラのフレームこの記事を書いた2019/3/3時点で252枚。同類のアプリの中では群を抜いています。  
これは以下のような構成になっています。 

a.001-037  
b.040-105  
c.sp001-sp030  
d.frame_h,frame_w,harekaze_w,sp_akenoh,sp_mashiroh,sp_minahなど特殊な  

a-cに関しては"008w"のように番号のあとに"h"か"w"が付いてそれぞれフレームの縦横に対応しています。cについては縦フレームがあったり無かったりします。

aとdはリリース時からあったノーマルフレームとスペシャルフレームで、bは後から追加されたスペシャルフレームです。ファイル名で区別するのが面倒になったのでしょう。
dのsp_akenohとかsp_mashirohはラーメンの上によくいるやつ

**038,039について**  
上の解説でこの2つが無いことに気づくことでしょう。  
結論から言うと[039](http://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/039h.png)は本当に欠番になっています。ファイルが存在しません。  
そして[038](http://app.aniplex.co.jp/hai-furi/assets/img/camera/frame/038h.png)はミーナさんのフレームになっていますが…アプリ上では表示されません。
```
        <li class="normal" data-file="034h.png"><img src="../assets/img/camera/thumb/034h.png" height="383" width="215"></li>
        <li class="normal" data-file="035h.png"><img src="../assets/img/camera/thumb/035h.png" height="383" width="215"></li>
        <li class="normal" data-file="036h.png"><img src="../assets/img/camera/thumb/036h.png" height="383" width="215"></li>
        <li class="normal" data-file="037h.png"><img src="../assets/img/camera/thumb/037h.png" height="383" width="215"></li>
        <li class="special" data-file="sp028h.png"><img src="../assets/img/camera/thumb/sp028h.png" height="383" width="215"></li>
        <li class="special" data-file="sp024h.png"><img src="../assets/img/camera/thumb/sp024h.png" height="383" width="215"></li>
        <li class="special" data-file="sp025h.png"><img src="../assets/img/camera/thumb/sp025h.png" height="383" width="215"></li>
```
ソースを見る限り書き忘れらしいんですよね…アニプレックスさん修正お願いします。

<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">はいふりカメラの未公開と思しきフレームを見つけたんですけど…<a href="https://t.co/rVr4fXs9DM">https://t.co/rVr4fXs9DM</a> <a href="https://t.co/uC676jg0zS">pic.twitter.com/uC676jg0zS</a></p>&mdash; さがみの (@sagami_no) <a href="https://twitter.com/sagami_no/status/974632713385689091?ref_src=twsrc%5Etfw">2018年3月16日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
正直これがsagaminoのTwitter歴で一番の実績のような気がしている
<br/>
<br/>

**最後に**  
かつて『天体のメソッド』というアニメにも「そらメソカメラ」(Androidのみ)というアプリが存在しました。これは物語の舞台のモデルである洞爺湖の聖地巡礼用につくられたものですが、今もうGoogle Playストアからダウンロードすることはできません。はいふりカメラもいつまでもあるとは限らないので、今のうちにできることはやっておきましょう。
<br/>
<br/>
<br/>
**余談**  
一応[exe化したもの](/file/haifuri_frame_dl.exe)も用意しましたが～、くれぐれも先方への攻撃とならないようお願いします。あとめちゃくちゃ遅いです

コード(言語はpython3)
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

		if os.path.exists(".\\haifuri_frame/" + frame_name) == 0:
			r = requests.get(url + frame_name, stream=True)
			if r.status_code == 200:
				with open(".\\haifuri_frame/" + frame_name , 'wb') as f:
					r.raw.decode_content = True
					shutil.copyfileobj(r.raw, f)
					print (frame_name+" をダウンロードしました。")


for i in range(1, 50):

	i_zero = str(i).zfill(3)
	frame_name_list = ["sp"+i_zero+"h.png","sp"+i_zero+"w.png",]

	for frame_name in frame_name_list:

		if os.path.exists(".\\haifuri_frame/" + frame_name) == 0:
			r = requests.get(url + frame_name, stream=True)
			if r.status_code == 200:
				with open(".\\haifuri_frame/" + frame_name , 'wb') as f:
					r.raw.decode_content = True
					shutil.copyfileobj(r.raw, f)
					print (frame_name+" をダウンロードしました。")


# 特別な名前のファイルをダウンロード

frame_name_list = ["frame_","sp_akeno","sp_mashiro","sp_mina","harekaze_"]

for frame_name in frame_name_list:
	if os.path.exists(".\\haifuri_frame/" + frame_name + "h.png") == 0:
		r = requests.get(url+frame_name+"h.png", stream=True)
		if r.status_code == 200:
			with open(".\\haifuri_frame/" + frame_name + "h.png", 'wb') as f:
				r.raw.decode_content = True
				shutil.copyfileobj(r.raw, f)
				print (frame_name+"h をダウンロードしました。")

	if os.path.exists(".\\haifuri_frame/" + frame_name + "w.png") == 0:
		r = requests.get(url+frame_name+"w.png", stream=True)
		if r.status_code == 200:
			with open(".\\haifuri_frame/" + frame_name + "w.png", 'wb') as f:
				r.raw.decode_content = True
				shutil.copyfileobj(r.raw, f)
				print (frame_name+"w をダウンロードしました。")
```

