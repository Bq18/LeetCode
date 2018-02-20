### 782.Transform-to-Chessboard

此题有比较大的难度。

我们来思考，交换矩阵的列、行会带来什么影响？那就是，无论怎么变化之后，任意两列相对的same/different关系是不会变的，同理任意两行之间的same/different关系也不会变。考虑最后的棋盘图案，行只有两种模式，列也只有两种模式。这就意味着在初始状态，所有的列必须也只有两种模式，同理，所有的行也是只有两种模式。不满足这两种条件的，都不能通过变换得到棋盘图案。

可行性的另外一个必要条件是：初始图案中，列的两种模式必须在数量上大体一致。也就是说，如果Ｎ是偶数，则两种数量一样；如果Ｎ是奇数，那么只可能一种比另一种多１个。同理，对于行方向也是如此。

以上的两个判定条件用代码实现起来比较麻烦。这里有一种比较巧妙的方法来实现第一个判定条件：
```cpp
        for (int i=0; i<N; i++)
            for (int j=0; j<N; j++)
            {
                if (board[0][0]^board[i][0]!=board[0][j]^board[i][j]) return -1;
                if (board[0][0]^board[0][j]!=board[i][0]^board[i][j]) return -1;
            }
```
我们可以这么理解，对于图案中任意的一个矩形ＲＯＩ：
```
AXXXXXB
XXXXXXX
CXXXXXD
```
因为所有的列只可能有两种模式，所以如果A=B，那么必须C=D（这两列是同一模式）；或者如果A!=B，那么必须C!=D（这两列是不同模式）。合并起来：```if (board[0][0]^board[i][0]!=board[0][j]^board[i][j])```。这样遍历所有的Ｄ的位置```(i,j)```之后，就能检验所有的列是否只有两种模式（相对于第０列）。

同理，```if (board[0][0]^board[0][j]!=board[i][0]^board[i][j])```可以判断所有的行是否只有两种模式。

接下来，因为所有的列都只有两种模式，所以我们只要考察每列的首位作为该列的代表，来判断所有的列的这两种模式在数量上是否大体一致。说明白了，就是抽取第一行，看０和１的数目是否大体相当。同理我们再抽取第一列，看０和１的数目是否大体相当。这两个条件也满足之后，说明本题是solvable.

此题的第二部分就是考虑交换多少次实现棋盘图案。首先，要明白一个特点，所有的行交换和列交换都不会互相冲突。我们可以先实现所有的行交换，再实现所有的列交换。注意，行交换（或者列交换）之间的顺序显然是不能随意变化的。

对于任意一个形如```0010100110```的数列，如果最少交换次数地变成一个０１交叉数列呢？

如果Ｎ是偶数，那么最终的目标是```01010101```或者```10101010```均可．只要将目标数列与原数列比较，假设有m位不一样，那么只要交换m/2次就可以了。比较两个目标数列中取得到结果最小的那个。

如果N是奇数，那么最终的目标必然是```101010101```（或者相反）。同理，将目标数列与元数列比较，假设有m位不一样，那么只要交换m/2次就可以了。注意，这里m必定会是偶数。

最终的答案就是将调整行的次数加上调整列的次数之和。