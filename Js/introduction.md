MP Version: v.230809  
# 導入 / Introduction
## 仕様 / Spec
まず、導入されているのは[Quickjs](https://github.com/bellard/quickjs)というjsエンジンである。  
内部的な詳細はそちらのリポジトリの中身を覗いたりドキュメントを読んだりすること。  
  
The engine embedded in 2003MP is [Quickjs](https://github.com/bellard/quickjs).  
If you want to know about details of the java script engine runs on your RPG_RT.exe, you should check its repository and documents.  
  

### 要点箇条書き / Bullet Points of facts  
- ES2020仕様のJava Scriptが実行可能。  
  - **ファイル読み込み・インポートや通信は(v.230809現在)できない。**  
  - QuickJSの仕様上、最初から組み込まれたモジュール以外は基本的にimport等の対象外であるため、  
    ライブラリ等を使うには手動で調整したものを組み合わせる必要がある。  
  - async/await等の機能も一切使えない（あくまで1イベントコマンドとして呼び出される以上、非同期処理を回すことができないため）  
- js拡張子のファイルを直で読み込んで実行することもできない。直接イベントコマンドとして書き込むか、TPC側から文字列変数を媒介して読ませる必要がある。  
  - ファイル入出力を.txtと.csvしか読み込めないTPCコマンドに依存しているため、  
    jsファイルとして書き上げた後も拡張子を.txtにする必要がある(著者はそのまま.js.txtという形で記名することを勧める)
- v.230809現在正式に実装されているのは、変数・スイッチ・文字列変数へのアクセスのみ。  
  - ピクチャやデータベース等へのアクセスについては実装予定らしい(著者はMP作者様じゃないので保証できません)  
- jsエンジン側で保持しているデータはツクール側にはセーブされない。  
- @jsコマンドを使って宣言したオブジェクト等は、F12/ロード等でQuickjs側のメモリが初期化されるまで保持される。
- **読み込まれたスクリプトに瑕疵があってjsエンジンが命令実行を中断しても、警告文が一切出ない**
- ツクールデータへのアクセスはけっこう重い(どれくらい重いのか気になる人向けにOptimizationの方にメモを置く**予定**)。
  
--------------------------------------
  (english version here)
  
### 何に使えるのか？（提案）
- 文字列処理
- 小数点を扱う計算
- 画像データ等のキャッシュ

### これだけは冒頭においておくべき魔法の呪文  
```js
var setv = setv || function(){};
var getv = getv || function(n){return n};
var sett = sett || function(){};
var gett = gett || function(t){return `string t[${t}]`};
var sets = sets || function(){};
var gets = gets || function(){return true};
```
  
2003MP以外の環境(コンソール上でのqjsを想定)でテストすることを考えてこのような処理を噛ませておくとよい。  
また、糖衣構文としてv[],t[],s[]等の記法が導入されているが、これらの利用は現状ではあまりおすすめできない。  
無論コンソール上でのデバッグを考えてのことである。  
  
モジュールインポート等を捨象しても、Node等とは微妙に動作が異なるところがあるため、  
スクリプトのデバッグにあたってはQuickJS本体をリポジトリからビルドしたものを使うと良い。  
