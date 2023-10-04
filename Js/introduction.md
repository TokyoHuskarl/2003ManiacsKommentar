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
  
### 何に使えるのか？
  
