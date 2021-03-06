adblock#.uc.js
==============

## Firefox 13以降をお使いの方へ

   Firefox 13でGlobal Storageが削除された影響で, 10/12/30 18:30 から 11/01/30 07:30 のバージョンを使っている場合<br />
   Firefox 13以降でadblock#.uc.jsが動作しなくなります(フィルタデータにアクセスできなくなるため).
   
   その際, Firefox 13以降をお使いのままadblock#.uc.jsを最新版へと更新しても, フィルタデータの引き継ぎはできませんので,<br />
   もしFirefox 13にアップデートしてしまった場合には, 一度[こちら](https://ftp.mozilla.org/pub/mozilla.org/firefox/releases/)より
   Firefox 12をダウンロードし, Firefox 12を起動して下さい.<br />
   一度Firefox 12を起動すれば自動でデータがGlobal Storageより引き継がれますので, adblock#.jsonがプロファイルフォルダのchromeフォルダ内にできていることを確認した上で
   Firefox 12を終了して下さい.
   
   そのあとはFirefox 13をお使いいただいても大丈夫です.
   

## 概要

   Adblock Plus（以下ABP）の劣化版.<br />
   広告を非表示にするのではなく, 読み込み自体をブロックするのでページ読み込み速度の向上, トラフィックの軽減が見込める.<br />
   ブロックしたURLはエラーコンソールに出力される.<br />
   コードの汚さ/起動などにかかる時間を犠牲にしてでもマッチ処理を速くすることを基本方針としている.

   本スクリプトはFirefox 3.6 - Firefox 13対応としているが, 基本的にFirefox 3.6.28, Firefox 10 ESR, 最新安定版(Releaseチャンネル)のFirefox,
   最新のFirefox Beta(Betaチャンネル), 最新のFirefox Aurora(Auroraチャンネル)で動作を確認しているので, それ以外のバージョン(Firefox 7など)では動かない可能性がある.<br />
   また, Nightly, Auroraでのみ発生するバグについては対応が遅れる可能性があるのでご了承下さい.


## フィルタについて

   ABPのフィルタと基本的には同じだが, 一部非互換/未実装部分がある.<br />
   同梱の adblock#.uc.js用リスト変換ツール.html で変換できる(不完全).<br />
   フィルタは 単純文字列 > アスタリスク > 正規表現の順で遅くなる.<br />
   速度遅延を気にする人はできるだけ単純文字列で済ませられるようにフィルタを作るとよい（正規表現でまとめないでバラの単純文字列にする 等）.<br />

   * 単純文字列で指定した場合, 部分一致/大文字小文字区別有りのフィルターとなる.

   * 正規表現/アスタリスク対応.
      * 正規表現は「/」で括ること.
      * アスタリスクを使用するフィルターは「+」で括ること（独自仕様のため注意）.
         * 設定により「+」無しでもアスタリスクが含まれているフィルタを自動的にアスタリスクフィルタと認識するようにできる(デフォルトでは無効).
            * この場合, 単純文字列フィルタ内に「*」が使用出来なくなる.
         * アスタリスクの数分だけ単純文字列より遅くなる(1個->2倍, 2個->3倍, ...).

   * 前方一致/後方一致フィルタ対応
      * 前方一致は先頭に, 後方一致は最後に「|」をつける.
      * 前方一致/後方一致フィルタには単純文字列とアスタリスクしか使用できない.
      * 前方一致と後方一致を同時に使用して完全一致ということはできないので, 完全一致させたい時は正規表現を使うこと.

   * ホワイトリスト(マッチしたURLをブロックしない)対応
      * ホワイトリストは先頭に「@@」をつける.
      * ホワイトリストにはすべての種類のフィルタが使用できる.

   * フィルタオプション(最後尾に$つけてファイルの種類などを指定するオプション)未対応
   * 「||」や「^」には未対応
   * CSSによる要素非表示フィルタ未対応(対応予定無) -> リスト変換ツールで変換可能.
   * フィルタの外部URL読み込み/自動更新未対応


## フィルタデータとその保存場所について

   フィルタやマッチ履歴などのデータは プロファイルフォルダ/chrome/adblock#.json に保存される.<br />
   (chromeフォルダは, [rebuild_userChrome.uc.xul](https://github.com/alice0775/userChrome.js/blob/master/rebuild_userChrome.uc.xul)
   を使っていれば 「ツール」->「userChrome.jsの設定」->「chromeフォルダを開く」より開くことができる)<br />
   スクリプトファイル内のfilter: 部分にフィルタを書く方式は廃止されたので,<br />
   移行する場合にはリスト変換ツールを使って変換後, Filter Manager より追加すること.
   
   10/12/30 18:30 以降のバージョンを使用していた場合には, フィルタのデータは Global Storageに保存されており,<br />
   Firefox 12 かそれよりも前のバージョンで実行した場合に限りデータは自動で引き継がれる.


## adblock#.uc.js Filter Managerについて

   「ツール」メニュー内にある「Organize adblock#.uc.js Filter...」をクリックすると, Filter Managerが新規タブで開く.<br />
   Filter Managerでは, フィルタとマッチ履歴一覧が表示され, 追加/編集/削除を行うことができる.<br />
   また各種設定もここで行うことができる.<br />
   UnDoはできないので, 大きな変更をするときには事前にadblock#.jsonのバックアップをとることを推奨.

   基本操作としては, フィルタを選択して, コンテキストメニューもしくは左下のボタンで<br />
   選択したフィルタに対して操作を行うという形になる.<br />
   ただし, Firefox7かそれよりも前のバージョンではコンテキストメニューが動作しないので,<br />
   すべて左下のボタンで操作することになる(のちのち改善予定).

   Black Listにはブラックリストが, White Listにはホワイトリストが表示される.<br />
   表示の都合上, ホワイトリストの先頭に@@がついていないのでブラックリストと混同しないよう注意.

   以下各機能について解説 (括弧内は左下のボタンを表す)
   
   * フィルターの追加 (「+」)
   
      入力欄に記入されたフィルターを, 1行ごとにフィルターとして追加する.<br />
      自動的にフィルターの種類が判別されて追加される.<br />
      「@@がなくてもホワイトリストとして処理する」にチェックを入れると,<br />
      入力したフィルタをすべてホワイトリストに追加する.

   * フィルターの削除 (「-」)
   
      選択されたフィルタを削除する.
      
   * フィルターの編集 (「✐」)
   
      項目をダブルクリックでも編集状態に入ることができる.<br />
      編集状態になるとフィルタ名のところがテキストボックスになるので, そこでフィルタを編集すればOK.<br />
      テキストボックス内でエンターを押すか, フィルターリスト以外のところをクリックすれば編集状態を終了する.<br />
      複数項目を同時に編集することはできない.
      
   * フィルターの最適化 (「⚙」)
   
      ヒット回数, 最終ヒット日時に基づいてヒットしやすいフィルターがより前にあるようにフィルターを並び替える.<br />
      フィルターのマッチ処理は上から順番に試行されていくため, マッチ処理の短縮化が望める.


## 設定項目について

   Filter ManagerのPreferenceパネルより設定が行える.<br />
   共にマッチ処理のパフォーマンスには影響しない.

   * 正規表現のフラグ
   
      正規表現フィルタのフラグを設定する. 変更は新しいウィンドウから有効.
      
   * +なしでもアスタリスクフィルタとして認識する
   
      フィルタを追加するときに, +で囲まなくてもアスタリスクフィルタとして認識するようになる.<br />
      そのかわり単純文字列フィルタ内に「*」を使えなくなる.


## adblock#.uc.js用リスト変換ツール.htmlについて

   他の広告ブロック拡張などのフィルターをadblock#.uc.js用フィルターへと変換するスクリプト.

### 使い方
   
   1. 変換元のリストを「変換元」にコピペする. 変換元リストの入手方法については下記を参照のこと.
   2. 「変換」を押すと「変換後」の所に変換されたものが出てくる.<br />
      「フィルター」の所に出力されたものは,Filter Managerで追加すれば良い.<br />
      また, 「CSS」の所に出力されたものは, 要素非表示フィルターをCSSへと変換したものなので,<br />
      そのまま Stylish または userContent.css へと追加すれば良い.

   * 大方のフィルタに対応済みだがたまに誤爆することも.
   * フィルタオプションは削除される.
   * 「||」や「^」はそれっぽいフィルタへと変換される.
      * オプションで正規表現を使って変換することができる. この場合ABPの「||」「^」とほとんど意味を変えることなく変換することが出来るが, 当然重くなる.
   * 要素非表示フィルタはCSSへと変換される.
   * 要素非表示フィルタの簡略記法 (http://adblockplus.org/en/filters#elemhide_simplified) には未対応.
   * フィルタの重複は確認しない.
   * 一応 豆腐氏の Adblock Plus 日本向けフィルタ(http://blog-imgs-31-origin.fc2.com/t/o/f/tofukko/Adblock_Plus_list.txt) に載っているリストが大体変換されることを確認済.

### 変換元リストの入手方法
		
#### Adblock plus

 1. Adblock Plusの設定画面を開く
 2. 「フィルタ」->「フィルタのエクスポート」を選択する
 3. フィルタがテキスト形式で保存されるのでそのファイルの内容をコピペすれば良い

#### Adblock++
		  
 1. about:config を開く
 2. extensions.adblock++.filter_array をコピペすれば良い


## 注意

   * 「Enable adblock#.uc.js」にチェックが入っているのにブロックされない場合, observerの登録にミスっている場合があります.<br />
     その場合は, about:config より adblock#.observerStarted の値を false にしてから新しいウィンドウを開くと直ります.
   * 要素非表示フィルタへの対応予定は今のところなし(userContent.css/Stylishでお願いします).
   * このスクリプトは総じてあまりテストされていないので, 不具合がある可能性があります. α版レベルと考えて下さい.<br />
     このスクリプトを使用したことによって生じたいかなる損害も作者はその責任を負いません.<br />
     すべて自己責任にてご使用下さい.
	 
## FAQ

Q. どうやって使うのかわからない

A. インストールから使い始めるまでの手順

 1. userChrome.jsを未インストールの場合は, まず[userChrome.js](http://userchromejs.mozdev.org/)をインストールし, 適当なサブスクリプトローダーを入れる.<br />
    この辺のuserChrome.jsの導入方法については他のサイトで解説されているので詳しくは割愛.
 2. adblock#.uc.jsをダウンロードして, 所定のフォルダ(サブスクリプトローダーで指定されているフォルダ)に入れる.
 3. Firefox を起動すると, 「ツール」メニューにadblock#.uc.jsのメニューが追加されているので, Organize adblock#.uc.js Filters...をクリックしてFilter Managerを立ち上げる.
 4. 左下の「+」ボタン, もしくはコンテキストメニューより「追加」をクリックして, ブロックしたいURLを追加していく.<br />
    Adblock Plusから乗り換えるには, 上の「adblock#.uc.js用リスト変換ツール.htmlについて」を参照のこと.

Q. 「このサイトでブロックされたURL一覧」みたいなのは無いのですか？

A. 今はありませんが, 実装予定です.

Q. $domain, $third-party が欲しい

A. 今はありませんが, 実装予定です.

Q. $subdocument, $xmlhttprequest みたいにURLの種類を指定したい

A. 申し訳ありませんが, ブロックしている方法がAdblock Plusと異なるため, 技術的に不可能です.

Q. 要素非表示フィルタを実装して欲しい

A. Adblock Plusも要素非表示フィルタについては単にCSSを動的に追加しているだけなので, 本質的にはuserContent.css/Stylishと機能が被っています.<br />
   adblock#.uc.jsは, できるだけ高速に動作することを目標としており, またuserChrome.jsで動く単なるスクリプトであるということも考えると,<br />
   そのような「URLのブロック」とは異なる機能を実装して重くなることはできるだけ避けたいと考えています.<br />
   そのため, 勝手ではありますが今後とも要素非表示フィルタに対応する予定はありません.<br />
   なお, Adblock Plusの要素非表示フィルタはリスト変換ツールでCSSに変換することができます.
   
Q. ホワイトリストがAdblock Plusと違う動作をする気がする

A. Adblock Plusには @@|http://example.com のように http:// または https:// から始まるホワイトリストを書くと, そのフィルターにマッチしたURLをブロックしないのではなく,<br />
   そのフィルターにマッチするURLの *ページ全体* でブロック機能を一時的にOFFするという機能がありますが, adblock#.uc.jsは現在この機能には未対応です.<br />
   混乱を招く可能性があるので, 今後オプションとして実装予定です.

Q. マッチ履歴とかいらないからもっと速くしろ

A. Old Version より 09/09/15 19:30 をお使い下さい. なお, 現在マッチ処理を高速化したあとの 10/05/21 21:30 相当のバージョンを準備中です.

Q. バグを見つけた/要望がある

A. 本ページ右上のIssuesより追加するか, [【CSS】Mozilla広告ブロック【Adblock】](http://find.2ch.net/?STR=Mozilla%B9%AD%B9%F0%A5%D6%A5%ED%A5%C3%A5%AF)に書き込んでいただければ対応します. もちろんTwitterやメールでも構いません.


## 更新履歴

   * 12/05/29 22:00 Global Storageの履歴データが壊れていると正しくデータが引き継がれなくなるバグを修正
   * 12/05/29 21:30 Firefox 13でFilter Managerが正常に動作しないバグを修正
   * 12/05/28 21:00 Firefox 13対応
   * 11/01/30 07:30 Bug 623435 - Rip out deprecated |RegExp.compile| builtin method
   * 11/01/28 22:00 結果が正しく保存されないことがあるバグを修正
   * 11/01/25 19:00 マッチ時の処理を少し高速化
   * 11/01/07 19:30 フィルターの最適化方法に誤りがあったのを修正
   * 11/01/06 20:50 空白行を追加するとFMが正しく表示されなくなるバグを修正
   * 11/01/06 17:40 フィルターの最適化ができるように
   * 11/01/05 17:10 ON/OFFが正しく切り替えられないことがあるバグを修正
   * 11/01/04 18:00 Firefox4でFMが起動できないバグを修正
   * 11/01/04 16:00 最後のwindowが閉じられたときにobserverを解除するようにした
   * 11/01/04 09:00 observerをwindow毎ではなく1回だけ登録するようにした
   * 11/01/03 17:00 FMで結果が正しく表示されないことがあるバグを修正
   * 11/01/01 00:30 正規表現フィルタが正しく動作していなかったのを修正
   * 10/12/31 22:00 結果が正しく保存されないバグを修正
   * 10/12/30 18:30 adblock#.uc.js Filter Manager を実装
   * 10/05/21 21:30 正規表現フィルタのマッチ処理を高速化
   * 10/05/21 20:40 アスタリスクフィルタを+無しでも機能するようにした
   * 10/05/21 20:30 マッチング処理を15%程度高速化
   * 09/09/15 19:30 typo
   * 09/09/14 18:00 ちょこっと高速化
   * 09/09/09 20:00 前方一致/後方一致フィルタに対応
   * 09/09/09 17:30 ホワイトリスト対応
   * 09/09/09 17:10 ON/OFFができるように
   * 09/08/31 10:30 高速化+メモリリーク対処
   * 09/08/31 09:30 アスタリスクフィルターが正しく動作していなかったのを修正
   * 09/08/30 22:00 /ads/のようなフィルターが正規表現と解釈されていたのを修正
   * 09/08/30 19:00 正規表現/アスタリスク/()を使用したフィルターに対応
   * 09/08/29 06:00 単純文字列/アスタリスク使用フィルターに対応(正規表現/()を使ったものには未対応)


## ToDo

   * adblock#.uc.js用リスト変換ツール の変換精度向上
   * $domain, $third-partyの実装
   * || の実装
   * ABP方式のホワイトリストの実装
   * 正規表現のショートカットの実装
   
   
   
   