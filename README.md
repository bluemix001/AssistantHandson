# WatsonChatBotHandson
IBM Watson チャットボット開発体験ハンズオン・セミナー2018の補助資料です

***

## 1. チャットボットに挨拶  
  
ワトソン宅配便  
宅配便受付チャットボット  
  
- 挨拶Intentの作成 

**挨拶**  
こんにちは    
おはよう  
こんばんは  
どうも  
まいど  
ハロー  
  
[質問例]  
>こんにちは  
>おはようございます  
>はじめまして  
>元気ですか  
>よろしくね  
  
- Dialogで挨拶ノードの作成  
**#挨拶**
\<? Input_text ?>。 いらっしゃいませ！  
  
## 2. IntentとEntityにより質問を理解する  
  
- サービス内容質問Intentの作成  
**サービス内容質問**  
何が出来るか聞きたい
サービスについて教えて
どうしたらいいか分かりません
ボットと話すの初めてです
何時までやってるの
いつまで受け付けてるの
料金はいくらですか

- 料金Entityの作成
**料金**
料金
送料
配送料
配達料
価格
値段
何円
いくら

- 営業時間Entityの作成  
**営業時間**  
  - 営業時間valueと同義語の作成  
  営業時間  
  受付時間  
  集荷時間  
  対応時間  
  
  - 休日valueと同義語の作成  
  休日  
  お休み  
  休憩  
  休業  
  
  - 何時valueと同義語の作成  
  何時  
  いつ  
  いつまで  
  何時まで  
  
- 受付ボットEntityの作成  
**受付ボット**  
ボット  
bot  
ワトソン  
Watson  
人工知能  
AI  
受付  
  
- Dialogでサービス内容質問ノードの作成
**#サービス内容質問**
  - Entityに応じた返答をする
  @料金  
  料金は荷物一つにつき¥1,500です。  
  
  @営業時間  
  お荷物の集荷は8時から22時まで。チャットでの受付は24時間いつでも対応いたします。  
  
  @受付ボット  
  私はコグニティブ技術を活用した受付botです。お荷物の集荷受付とご予約内容の確認ができます。  
  
  anything_else  
  ご質問でしょうか。私はWatson宅配便botです。  
  
[質問例]  
>荷物送るのっていくらするの  
>料金表ありますか  
>送料の情報が欲しい  
>  
>受付時間について知りたい  
>荷物はいつ出せるの？  
>休業日とかあるの？  
>  
>人工知能って何が出来るの？  
>ワトソンさんですか？  
>ボットと話すの初めてなんだけど  
>  
>名前はなんていうの？  
>質問していいかな  
>あなたは誰ですか  
  
- 確信度の表示
確信度=\<? intents[0].confidence ?>  
  
  
## 3. 注文を受け付ける
  
- 注文Intentの作成
**注文**
集荷お願い  
注文する  
注文したい  
荷物出したい  
出荷したい  
荷物取りに来て  
送りたい荷物があります  
荷物送りたい  
出荷予約したい  
  
  
- Dialogで注文ノードの作成

**#注文**
  - スロットでEntityに応じた対応をする  
  @sys-date  
  $date  
  集荷のご希望日は？  
  
  @sys-time  
  $time  
  集荷のお時間は？  
  
  @sys-number  
  $number  
  お荷物の数は？  
  
  - Contextがすべて揃ったら
  \<? $date.reformatDateTime('M月d日') ?> の <? $time.reformatDateTime('k時')  ?> にお荷物 <? $number ?> 点を引き取りに伺います。  
  
  
[質問例]  
>荷物を運んでほしい  
>12月27日  
>10:00でお願いします  
>4つです  
>  
>12/27の10:00に荷物を4つ運んでほしい  
>12月12日10:00に一袋の出荷予約する  
>今週土曜日の午前六時に荷物4個送りたい  
>今日の午後八時に荷物2つ引き取りに来て  
>今日二時間後に荷物2つで予約したい  
>次の月曜日の14時にお米3袋送りたい  
  
  
- Slots応答内容改善  
\<? $date.reformatDateTime('M月d日') ?>の集荷ですね  
集荷のご希望日を「何月何日」と指定してください。  
  
ご希望の時間は<? $time.reformatDateTime('k時') ?>ですね。  
集荷のお時間を「何時」と指定してください。  
  
荷物は <? $number ?>点ですね。  
荷物の数を数字で指定してください。  
  
[質問例]  
>注文する  
>クリスマス  
>十二月二十五日  
>午後イチでお願い  
>午後一時  
>たくさんあります  
>全部で十個口です  
>明日の午後8時に発送したい  
  
- Contextの評価  
$time.before('08:00:00')  
$time.after('22:00:00')  
申し訳ありません。集荷時刻は8時から22時の間でご指定ください。  
true  
  
[質問例]  
>明後日荷物の引き取りお願いします  
>朝7:00でお願いします  
>では朝8:00に来てください  
>3  
  
  
## 4. アプリケーションからアクセス
  
[質問例]  
>こんにちは  
>あなたは誰ですか？  
>宅配便botって何してくれるの  
>荷物送るのにいくらかかるの  
>何時まで受け付けてくれるの  
>注文したい  
>次の水曜日午後8時に来てください  
>4箱です  
>456@ibm.comです  
  
  
## 5. バックエンドシステムとの連携
  
- Dialogの注文ノードに注文確定処理を追加  
  
注文  
こちらでよろしければメールアドレスを入力してください。  
  
  
- @mail Entityの作成  
**mail**    
\\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b  
  
  
- Dialogで注文確定ノードの作成  
 
**注文確定**  
true  
  
  - スロットでEntityに応じた対応をする  
  @mail  
  $mail  
  メールアドレスを半角で入力してください。  
    
  - メールアドレスを受け取ったら  
  <? $mail ?> のご注文内容を登録します。  
  
  -json editorで追記  
  .literal  
  
  
[質問例]  
>明日の12時にダンボール3箱送りたい  
>日本橋箱崎町のIBMです  
>明日の12時にダンボール3箱送りたい  
>hakozaki@ibm.comです  
  
  
- 予約内容の照会  
  
[質問例]  
>予約した内容を確認したいです  
>test@ibm.comです  
>test@ibm.comの予約照会  
>注文した内容を忘れてしまいました  
>私のメールアドレスはabc@ibm.comです  
>注文照会する  
>xxx@ibm.com  
>出荷注文の内容を確認したい  
>ｘｙｚ＠ｉｂｍ．ｃｏｍ  
>予約確認したい  
>XYZ@IBM.COM  

