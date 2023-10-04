# 最適化
## はじめに
まず、このディレクトリ内の文書に書かれている事柄の実践は、完全にTPC化して製作している方やある程度心得がある方以外にはおすすめできない。  
TPCで組み上げない限りは可読性が完全に終わりになる。エディタはもうバグ取りでコマンド差し込むの以外に使わないくらいの覚悟が必要である。  
~~しかしそうするとなんでツクールでやっているんだろうか~~
また、著者はCSのdegree持ってるとか競プロやってるとかではないため、そういった王道な最適化についての指針としては役に立たないと思われる。  
端的に言って強い人の参考にはあまりならないだろう。  
  
とはいえ2003MP特有の事情が色々とあるため、コーディングに自信ありの諸兄も一度目を通しておくとよいかもしれない。  
また、めぼしいコマンドの負荷についても逐次メモを取っておく（予定）。  
  
[変数操作関連コマンド](./VariableAndArray.md)  
  
------------------------------
  
Tick計測に用いているイベントコード / Tick measuring event code:  
<details>
  <div>

  ```tpc
  @sys.gameOpt .picLimit 10000
  @sys.gameOpt .fatal 60, 0, 0
  @bgm.play "2003Empire" .opt 0, 100, 100, 50
  @msg.show "Press any key to start"
  @loop .inf() {
      @wait 0
      v[10] = 5
      v[30] = 10000000
      v[7..8] = 0
      @loop v[10] {
          v[5] = rnd(1, 999999)
          v[6] = rnd(1, 999999)
          @wait 0
          v[5].copy v[1], 2
          v[3] = sys.tick
          @loop v[30] {
              // insert proc a

          }
          v[3] = sys.tick - v[3]
          @wait 0
          v[5].copy v[1], 2
          v[4] = sys.tick
          @loop v[30] {
              // insert proc b

          }

          v[4] = sys.tick - v[4]
          v[7] .add v[3], 2

      }

      v[7..8] /= v[10]
      @msg.show "\> Result:
  \>proc a * 10000000 -> \v[7] tick
  \>proc b * 10000000 -> \v[8] tick"

  }
  ```

  </div>
</details>


