## 似たような処理の比較
ほぼ自分用備忘録（このリポジトリ自体がそうだけど）  
  
### v[var1 + var2] = var3
A:
```
var1 = var2 + var3
var3.copy v[var1]
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
var1.add var2,1
```

B:`var1 = var2 * var3`
- B勝ち。 A -> 417t, B -> 385t
 
