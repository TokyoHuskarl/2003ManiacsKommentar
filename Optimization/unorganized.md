## 似たような処理の比較
ほぼ自分用備忘録（このリポジトリ自体がそうだけど）  
  
### v[var1 + var2] = var3
A: 
```
var4 = var1 + var2
var3.copy v[var4],1
```

B:`v[var1 + var2] = var3`
- A勝ち。 **A -> 621t**, B -> 769t
  
### var1 += var1 + var2
A:
```
var1.add var2,1
var1.add var3,1
```

B:`var1 += var2 + var3`
- A勝ち。 **A -> 422t**, B -> 647t
 
### var1 = var1 * var2
A:
```
var2.mul var3,1
var2.copy var1,1
```

B:`var1 = var2 * var3`
- B勝ち。 A -> 417t, B -> 385t
  
### var1 += var1 * var2
A:
```
var2.mul var3,1
var1.add var2,1
```

B:`var1 += var2 * var3`
- A勝ち。 A -> 559t, B -> 654t
 

### modを使った単純な分岐式
A:
```
var1 = var2 % 16
@if var1 == 0 {}
```
B:`@if (var2 % 16) == 0 {}`
-  **A -> 664t** B -> 768t  
筆者の環境では\`を使うとマークダウンプレビューが崩れてしまうため省略しているが、本件Bの@if文では\`で式呼び出しを行っている。  

