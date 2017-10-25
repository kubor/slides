<!-- $theme: gaia -->
<!-- template: invert -->
<!-- page_number: true -->
<!-- $size: 4:3 -->


ソーシャル創薬美少女
活動日記
===

###### 第6回ケモインフォマティクス若手の会


###### 久保 竜一 [@kubor_](https://twitter.com/kubor_)
---

# WHO ARE YOU ??
![](https://pbs.twimg.com/profile_images/917266518160769025/DDQFzcye_200x200.jpg)
- DeNAライフサイエンスのバイオインフォマティシャン
- 趣味でソーシャルメディアを活用した研究活動

---

### RDKitユーザー会作ったりしてます

![](https://user-images.githubusercontent.com/7918702/27967309-84eba2f0-637d-11e7-94cd-3ac5d68505c1.png)
https://rdkit-users-jp.github.io/

---

# 今日話したいこと

- 創薬ちゃんのこと
- 創薬ちゃんの化合物詠唱機能のこと
    - Twitter bot
- Dockerで簡単にアプリをデプロイする
- Flaskを使った簡単なWEB API実装

---

# 創薬ちゃんって？
<img src="https://user-images.githubusercontent.com/7918702/27967611-a8f87c44-637e-11e7-83bc-ca11b2fd485a.png" align=right width=280>

- すごく可愛い
- ケミカルスペースを旅してる
- かわいい
- 創薬研究者
- とにかくかわいい
- Twitter: [@souyakuchan](https://twitter.com/souyakuchan)

---

# 創薬ちゃんTwitterモーメント
- 時事ネタを含めた創薬化学全般の話題を提供
	- [BRAF を例にキナーゼの活性化機構について
](https://twitter.com/i/moments/824262531564699648)
	- [エンドセリン type B 受容体の活性化機構](https://twitter.com/i/moments/789800984528551936)
	- [ボツリヌス毒素](https://twitter.com/i/moments/850626127181643776)
	- [抗マラリア薬](https://twitter.com/i/moments/839516326704009216)

---

#### なんでも知ってる創薬ちゃん
<img src="https://user-images.githubusercontent.com/7918702/27968022-5180f50c-6380-11e7-8f38-1c8f15726c64.png" width=500>

###### 難しいけどわからないわけじゃない

<img src="https://user-images.githubusercontent.com/7918702/27968046-6f474960-6380-11e7-8aa4-717eda61d7e8.png" width=500>

[重水素化医薬品 - 創薬ちゃんTwitterモーメントより](https://twitter.com/i/moments/852056343229997056)

---

# 構造式を描くBOT機能を実装

---

# 創薬ちゃんが構造式を描いてくれる

<img src="https://user-images.githubusercontent.com/7918702/27969212-f9882e92-6384-11e7-878b-e444ad6ebaa0.png" width=420>

<img src="https://user-images.githubusercontent.com/7918702/27969235-11c4f9a4-6385-11e7-8f1d-9733c5534378.png" width=420>

---

# ユーザーの反応

---

<img src="https://user-images.githubusercontent.com/7918702/27969576-5610ff4e-6386-11e7-8eb8-2f6a54cb4284.png" width=480 align=right>

## 確かにこういうのもSMILESで書けるけども・・・

---

# 大喜利が始まる

---

<img src="https://user-images.githubusercontent.com/7918702/27969467-ee2d5b48-6385-11e7-92d6-53ae83ae93e6.png" width=580 align=right>
<img src="https://user-images.githubusercontent.com/7918702/27969503-0e3bbe5c-6386-11e7-8df2-4ed29849582e.png" width=580 align=right>

### ChemDrawかなにかでお絵描きしてからSMILES貼り付け様子

---

<img src="https://user-images.githubusercontent.com/7918702/27969702-ddadb3de-6386-11e7-9a72-75e744a18821.png" width=600 align=right>

## サルフラワーもなんだか怪しいけど描ける

---

<img src="https://user-images.githubusercontent.com/7918702/27969762-2808970a-6387-11e7-90bf-29005787c1c1.png" width=580 align=right>

## 円形

---

<img src="https://user-images.githubusercontent.com/7918702/27972974-32c67bd8-6393-11e7-8d00-ca695659b228.png" width=540 align=right>

## テトリス

---

# bot機能はどうだったか
- フォロワーが増えた
![](https://user-images.githubusercontent.com/7918702/27970368-8714f3d6-6389-11e7-9c7b-726848d1028a.png)

---

# bot機能はどうだったか
- ユーザーの反応がリアルタイムに返ってきて開発側としてもすごく楽しい！
- 有機の課題も助かったみたいで嬉しい

![](https://user-images.githubusercontent.com/7918702/27970463-e60b755e-6389-11e7-81bc-ec211cbd42de.png)

---

# 技術の話

## どうやって作ったのか

---

# Python3 + Docker + RDKit
- [ddquest/chem_bot](https://github.com/ddquest/chem_bot)

![](https://user-images.githubusercontent.com/7918702/27972710-42debd92-6392-11e7-93f7-3b115006deee.png)

---

# 仕組み

- :bird: Twitter Userstreamingを監視（[Tweepy](https://github.com/tweepy/tweepy)）
- :bulb: メンションを受けると[RDKit](http://www.rdkit.org/)で構造式を描画
- :speaker: IUPAC名はSMILESに変換（[OPSIN](http://opsin.ch.cam.ac.uk/)）
- :bird: JPEGバイナリ付きのツイートをTwitter APIを叩いて送信

---

# RDKitで構造式を描くプロセスを簡略化

---
`rdkit.Chem.Draw.MolDraw2DCairo()` をラップして使用
```
from chem_bot import SmilesEncoder
encoder = SmilesEncoder('C(CN(CC(=O)O)CC(=O)O)N(CC(=O)O)CC(=O)O')
encoder.to_png()
encoder.to_file('edta.png')
```
![](https://github.com/ddquest/chem_bot/blob/master/docs/img/edta.png?raw=true)											

---

# IUPAC名の変換

---

# OPSIN

> Lowe, D. M., Corbett, P. T., Murray-Rust, P., & Glen, R. C. (2011). Chemical name to structure: OPSIN, an open source solution. http://pubs.acs.org/doi/abs/10.1021/ci100384d

- Java製のコマンドラインツール
<img width="585" alt="2017-10-17 19 24 03" src="https://user-images.githubusercontent.com/7918702/31659971-d672c8cc-b370-11e7-8e1c-310e9e7c2588.png">
- Cannonical SMILESではない
    - RDKitで変換

---

# Docker環境

---

デプロイを簡単にするためDockerで環境構築
```
FROM kubor/alpine-rdkit:latest

MAINTAINER kubor

COPY . /chem_bot

WORKDIR /chem_bot

ENV LC_ALL=C

RUN python setup.py install && \
    (cd java/ && sh get_opsin.sh)

RUN apk update && \
    apk --no-cache add openjdk8

CMD ["python", "-u", "bin/run_twitter_client.py"]
```

---

#### RDKitが動く軽量なDocker Imageも作った

[kubor/alpine-rdkit](https://github.com/kubor/alpine-rdkit)

- Alpine Linuxベースで軽量（圧縮済みで465 MB）
- miniconda
- rdkitの動作に必要なライブラリも内包
- TRY! 
	```
    docker -it run kubor/alpine-rdkit bash
    ```
    
---

# 構成図
- Azure上でDockerコンテナを稼働

![chem_bot](https://user-images.githubusercontent.com/7918702/31661574-33c340ce-b376-11e7-893e-0b16bbf4c7cd.png)

---

# Dockerを使うメリット
- 環境設定がテキストなのでGit管理しやすい
- 開発環境と同じ環境をクラウド上に再現できる
- アプリケーションを自動で再起動できる
	```
	docker run --restart=always
	```

---

chem_botの描画機能をWEB APIとして実装してみた

```
https://chemical.space/api/v1.0/smi2img?smi=BBBBBB&width=420&height=420
```

![](https://chemical.space/api/v1.0/smi2img?smi=BBBBBB&width=420&height=420)

---

# FlaskWEB API化
- 構造式を描画機能をWEB API化しておくと汎用性が増して応用範囲が広がりそう
- オンラインならPubChem API(PUG REST)もあるけど、ローカルで完結させたい
    - https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/smiles/CCCCC=O/PNG

---
30行程でWEB APIを作ることができる

<img width="400" alt="2017-10-17 20 17 05" src="https://user-images.githubusercontent.com/7918702/31662127-3118c45a-b378-11e7-9648-47a7f6f16442.png">

---

# まとめ
- RDKitの構造式を描画関数を簡易に扱うようPythonでラップした
- 一般の方でも体験してもらえるようTwitter bot化した
- 環境構築にDockerを利用し、Azureにデプロイした
- Flaskを使ってWEB API化すると更に汎用化できる

---

# :warning: 注意
##### [@kubor_](https://twitter.com/kubor_)は創薬ちゃんの中の人ではないし、創薬ちゃんに中の人などいません

---

# おしまい

#### 創薬ちゃん先生の今後の活躍にご期待下さい