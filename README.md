# PukiWiki用プラグイン<br>	ページ数表示 totalpages.inc.php

ウィキ内のページ数を数えて出力する[PukiWiki](https://pukiwiki.osdn.jp/)用プラグイン。

|対象PukiWikiバージョン|対象PHPバージョン|
|:---:|:---:|
|PukiWiki 1.5.3 ~ 1.5.4RC (UTF-8)|PHP 7.4 ~ 8.1|

## インストール

下記GitHubページからダウンロードした totalpages.inc.php を PukiWiki の plugin ディレクトリに配置してください。

[https://github.com/ikamonster/pukiwiki-totalpages](https://github.com/ikamonster/pukiwiki-totalpages)

## 使い方

```
&totalpages([pagePrefix]);
```
pagePrefix … この文字列から始まる名前のページだけを数える。省略すると全ページ

## 使用例

```
このウィキには&totalpages();本の記事があります。
PukiWikiに関する記事は&totalpages(PukiWiki/);本です。
```

## 設定

ソース内の下記の定数で動作を制御することができます。

|定数名|値|既定値|意味|
|:---|:---:|:---|:---|
|PLUGIN_TOTALPAGES_PAGE_ALLOW| 文字列| |カウント対象ページ名を表す正規表現|
|PLUGIN_TOTALPAGES_PAGE_DISALLOW| 文字列| '``^(Pukiwiki\/.*)$``'|カウント除外ページ名を表す正規表現|
|PLUGIN_TOTALPAGES_NUMBERFORMAT| 0 or 1| 1|ページ数を3桁カンマ区切りで表示する|

## 処理の詳細

数えるのは次のすべての条件で絞り込まれたページです。  
特殊なページを省くなど、ご自分のウィキを厳密に数えたいかたは条件を調整してください。  
正規表現の知識が必要です。

1. RecentChanges ページを除外する
2. pukiwiki.ini.php 内 $non_list で指定されたページを除外する
3. PLUGIN_TOTALPAGES_PAGE_ALLOW 定数が空でなければ、それが表すページのみを対象とする
4. PLUGIN_TOTALPAGES_PAGE_DISALLOW 定数が空でなければ、それが表すページを除外する（デフォルトでは「Pukiwiki/」から始まるページを除外）
5. 引数が空でなければ、その文字列から始まるページのみを対象とする
6. 閲覧不可のページを除外する

数えたページ数は専用のキャッシュファイル ``cache/totalpages[.*].dat`` に保存し、次回から処理を省略します。  
``cache/recent.dat`` のタイムスタンプを参照し、ページの編集・増減を検知したら数え直します。  
もしプラグイン内の定数やコードを書き換えて条件を変更したら、キャッシュファイル ``cache/totalpages[.*].dat`` を削除するか適当なページを編集して強制的に数え直させてください。
