# RPG_RT.iniの編集で何ができる？ / What can we do with RPG_RT.ini?  
  
 バニラではプロジェクト名を記載できる程度であったが、2003MPにおいてRPG_RP.iniには機能が色々と追加されている。  
以下、DIS_Legacyのものを参考として簡素な機能解説をする。  
  
In native RM2003, what RPG_RT.ini can do was basically just to set GameTitle.  
But in Maniacs Patch, ini file got new functions.  
some can be very important if you want to do something complex with the patch.  
  
-------------------------------------------------
  
## DIS:Legacyからのサンプル / Example from DIS:Legacy
```
//************
//*RPG_RT.exe setting
//************

// ---------------------- DO NOT CHANGE HERE ----------------------
[RPG_RT]
GameTitle=Doradora Island Saga:Legacy v.1.162BETA3fix1Alpha
MapEditMode=2
MapEditZoom=0
FullPackageFlag=1
RuntimePackageKey=""
pf = 1
// sjis:932 utf-8:65001
Encoding=932
StepsPerFrame=30000000
// ---------------------- DO NOT CHANGE HERE ----------------------



//************
//*RESOLUTION*
//************

// Default resolution デフォルト
WinW = 800
WinH = 450
// Low-End PC option 低スぺ向け 細かい表示が崩れます
//WinW = 640
//WinH = 360
// Wide monitor fullscreen 1920*1080のモニタにピッタリフィット
//WinW = 960
//WinH = 540

```
  
-------------------------------------------------
  
## 元からある記載 / Native function
### [RPG_RT]
`[RPG_RT]`  
具体的な記載の前に置いておく必要がある。省略できない。  
  
You cannot omit this. Also you have to put this before anything but comment.  

### ゲームタイトル / GameTitle
`GameTitle=Doradora Island Saga:Legacy v.1.162BETA3fix1Alpha`  
ウィンドウに表示されるゲームタイトルを決める。  
  
Set game title.

### エディタの編集モード / Map Edit Mode on RPG Maker Editor
`MapEditMode=2`  
`MapEditZoom=0`  
Should I even eggsblain this?  

## ランタイム周辺 / Settings loading RTP
### RTPの要否 / Whether RTP is required or not
`FullPackageFlag = 1`  
0か1が入る。記載しなければ0。  
0であればRTPを読み込む（プレイに際してRTPが必要となる）。  
1であればRTPを読み込まず、ゲームディレクトリ内のアセットのみを読み込む。  
- このフラグが1でも、ツクールエディタ上においてはRTP素材は依然として選択可能となっていることに注意。  
  

  
### ランタイムパス指定 / Setting RTP Path
`RuntimePackageKey=""`  
フォルダを指定することで英語版RTPではなく日本語版RTPを読み込ませることができる。  
- サンプルではそもそもRTPを利用していない関係上未指定。  
このとき、仮にFullPackageFlagが1であったとすれば、英語版RTPが読み込まれる。  
  
- 

## MPの設定関連 / MP Configuration

### 処理モード変更
`pf = 1`  
処理のモードを変更する。1であればpf、0であればimとなる。  
デフォルトは0。  
- 現在Discord外で公開されているMPには、マップイベント更新設定について差異のあるpf版とim版の2つが存在している。  
しかし、最新64bit版では両者は統合されており、この記載によって初めてpf版の処理が行われることとなる。  
**Discord外で公開されているバージョンで作っていたプロジェクトを64bit版に移植する際は、どの版を使っていたかを確認した上でこの記載をしておかないと変なことになる**。  

### 文字エンコード / Encoding
`Encoding=932`  
文字エンコーディングの設定。**英語版だけかも？**  
右側には文字コードが入る。サンプルではsjisなので932。  
- 日本人ユーザーはよほどのことがない限り設定する必要がない。  
右辺をUTF8(65001)にすればウムラウト等の特殊な文字も記入可能になると思われる。  
が、２バイト文字の素材が読めなくなったり色々と面倒なことになるので既存のプロジェクトのエンコードをsjisからいきなり変えるのはおすすめできない。  

### ステップ数上限 / Upper limit of steps per frame
`StepsPerFrame=30000000`  
ステップ数上限を設定する。  
- デフォルトでは1fに50万？30万？程度のコマンドステップが嵩むと処理を強制的に中断して次fへ回すようになっている。  
並列処理で使用変数に被りがある際（一時変数等を想起せよ）、コマンド数の多い処理の繰り返しを何百回と使ったりしていると、この上限にひっかかって謎の不具合を起こしがち。  
なので、アクションゲーム等、処理数が増えがちなゲームを作る際は、可能な限り数値を大きめに設定すると吉。  
- ただし注意点として、組み方が悪いループがあったりすると、今度はそこでフリーズしてしまったりする。  
これは本来上限に触れることで数ｆに分割して行われていた処理が一気に１ｆに集中することで生じる。  

## 画面設定 / Window Config
### 解像度 / Resolution  
`WinW = 800`  
`WinH = 450`  
ゲームの画面解像度を設定する。WinWは横幅、WinHは縦幅。  
デフォルトではWinW=320,WinH=240である。  
- 理論上は超高解像度にすることもできる。  
が、現行MPの描画はあまり強くないため、デフォルトの2倍あたりから有意にパフォーマンスが低下してくる。  
- 画面に表示されるピクチャ数が多い重めのゲームで高解像度設定を行う場合、  
内部処理をちゃんと最適化していかないと要求スペックが跳ね上がってしまうかもしれないことに注意。  
- ちなみに、TPCコマンド`@sys.getInfo .winSize .dst v[n]`を使うことで、現在の解像度をゲーム内で[v[n],v[n+1]]へ出力することができる。  
当該コマンドを使えば、ユーザー側の解像度変更を検知してUI配置を調整したり、あるいは解像度変更自体がトリガーとなるイベントを作ったりすることができる。  

## その他  
### コメント / Comment out  
`//*RPG_RT.exe setting`  
`//`を行頭に置くことでコメントアウトができる。  
どれが何をするのか思い出せなくなりそうなとき等にメモしておくと吉。  
  
By putting `//` at BOL, you can comment out the line.  


