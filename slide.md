# AWS Casualその後
AWS Advanced Users Meetup vol.2 @Cookpad

id:myfinder

---

# who?
- id:myfinder
- M.T.Burn/FreakOut
- MySQL Casual

---

# 先にお知らせ

___

# MySQL Casual Talks vol.7
- <span style="color: red">12/12(金)</span>開催
- トーク/LTネタがある方はお気軽に^^

___

# MySQL Casual Advent Calendar
- まだ空きがあるのでカジュアルに登録かもんლ(╹◡╹ლ)

---

# 今日話すこと
- <span style="color: red">"どオンプレ環境"</span>で作ったサービスを最近 AWS に移設した話をAWS Casualでしました
- あれからもうちょっと手入れしたのでその辺の話と疑問点について

___

# 対象サービス
## http://mtburn.jp/

___

## メディア企業の皆様
# どしどし<span style="color: red">お申込み</span>
# ください

---

## 前回までのアプリサーバ
- NAT インスタンス<span style="color: red">不使用</span>
- すべての EC2 インスタンスに Public IP を付与
- アクセスコントロールは Security Group で設定
- 個別のインスタンスに名前は付けない
 - したがって<span style="color: red">内部 DNS 廃止</span> / IP 付与も DHCP 任せ
- セットアップ->運用はAnsibleの<span style="color: red">継続適用</span>
 - しょぼい適用ミスの障害が出たりしてた

___

## 今日までのアップデート
- <span style="color: red">Route53 Private DNS</span> 導入
- <span style="color: red">Immutable</span> な運用に変更
- <span style="color: red">Auto Scaling</span> 対応

___

## これでようやく
## Advancedか？

___

## Private DNS 導入
- やっぱり<span style="color: red">名前付けたい</span>サーバは(ごく一部だけど)ある
- 渡りに船でした :)

___

## Immutable な運用

- 一度設定したサーバに変更を加えず、変更があるときはアプリサーバ作り直し
 - blue-green :)
- 但し開発者が書くソースコードはこれまでどおり <span style="color: red">cap</span> で deploy
 - cap with mackerel

___

## Auto Scaling 対応
- ライフサイクルフック<span style="color: red">不使用</span>
 - 複雑なセットアップが要らない
 - 通知はSNS使わず、Mackerel/Slackにまとめた
- cloud-init と shutdown 時のスクリプトで処理
 - Ansible<span style="color: red">廃止</span>
 - 起動時に cloud-init で監視/セットアップ
 - 停止時に init.d スクリプトで通知/監視停止
 - AWS にロックインされにくい体にしたい向きもあった

---

# これで
# ひと安心

---

# 疑問/質問

- terminate 時に init スクリプトで sleep するなどして処理を<span style="color: red">ブロック</span>した場合の待ち時間はどの程度か
- <span style="color: red">"ライフサイクルフックの方が捗るよ！"</span>みたいな方と意見交換したい

---

# おわり
