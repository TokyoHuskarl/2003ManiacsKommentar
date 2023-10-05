# ピクチャ関連処理  
前提として、変数操作の章と比較して**計測試行回数を/10している**。  
そのため、変数操作を基準として比較したい場合はざっくり\*10Tickして考えること。  
## ピクチャ表示のバニラからの仕様変更  
## ピクチャキャッシュシステムの有無による負荷の差  
### 結論
キャッシュとしてどこか空いたピクチャ番号に画像をロードしておくと表示処理の負荷がメチャクチャマシになる。
が、キャッシュされていたとして、ピクチャ移動と同じくらい軽いというわけではない。  
### 計測
```
@pic[2].show {
    "ui\minimapback"
    .pos v[1282], v[1283] .topLeft
    .chromakey 1
    .scale 100
    .trans 0
    .rgbs 100, 100, 100, 100
    .mapLayer 9
    .eraseWhenTransfer
}
@pic[2].erase
```
上記処理を繰り返す。ui\minimapback.bmpはこのディレクトリに同梱されているものと同じ透明色一色148*92の　ビットマップファイルである。    
**重くて終わらないので計測試行回数を変数操作比で/100している。(つまり変数操作と比較する場合、\*100して考えるべし)**  
    
### 結果
A:どこにも同じ画像がキャッシュされていない状態での試行 -> 2877t  
B:事前に"ui\minimapback"をpic[1]にロードした状態での試行 -> **100t**  
比較すれば激烈に軽い。沢山消したり出したりする画像はどこかにキャッシュしておこう。  
  
## ピクチャ編集  
おもいょ？
### ミニマップ用途について
```
@pic[1].show {
    "ui\minimapback"
    .pos v[1282], v[1283] .topLeft
    .chromakey 1
    .scale 100
    .trans 0
    .rgbs 100, 100, 100, 100
    .mapLayer 9
    .eraseWhenTransfer
}
v[748949].enum 222, 13616
```
上記をループ外に前置して試行

A:
```
@pic[2].show {
    "ui\minimapback"
    .pos v[1282], v[1283] .topLeft
    .chromakey 1
    .scale 100
    .trans 0
    .rgbs 100, 100, 100, 100
    .mapLayer 9
    .eraseWhenTransfer
}
@pic[2].setPixel .xywh 0, 0, 148, 92 .src v[748949]
```
 -> 3830t  
B:
```
@loop 16 {
    @pic[2].setPixel .xywh 0, 0, 4, 4 .src v[748949]
    
}


```
-> 763t 
`@loop 64(略)` -> 2894t
可能であれば更新する場所のIDを配列として整理してからその配列に対して@foreachをかけて処理した方が早いかもしれない。 

```
@pic[2].show {
    "ui\minimapback"
    .pos v[1282], v[1283] .topLeft
    .chromakey 1
    .scale 100
    .trans 0
    .rgbs 100, 100, 100, 100
    .mapLayer 9
    .eraseWhenTransfer
}
@pic[2].setPixel .xywh 0, 0, 1, 1 .src v[748949]
```
キャッシュから画像を再表示して初期化->2614t  
```
@pic[2].setPixel .xywh 0, 0, 148, 92 .src v[748949]
@pic[2].setPixel .xywh 0, 0, 1, 1 .src v[748949]
```
画像編集による初期化->1239t  
