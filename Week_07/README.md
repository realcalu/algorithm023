##### 70. 爬楼梯
爬楼梯已经做了很多遍了,有使用dp做的，有使用递归，也有使用非递归。看课程老师说使用递归做一下

```
class Solution {
    int[] cache;
    public int climbStairs(int n){
        if(n<=1)return 1;
        if(cache==null)
            cache = new int[n+1];
        if(cache[n]!=0)
            return cache[n];
        cache[n] = climbStairs(n-1)+climbStairs(n-2);
        return cache[n];
    }
}
```


##### 208. 实现 Trie (前缀树)
根据老师给的模板写了一下，发现太慢，然后找了个快点的版本。如下。
```
class Trie {
    Trie[] child;
    boolean isEnd = false;

    /**
     * Initialize your data structure here.
     */
    public Trie() {
        child = new Trie[26];
    }

    /**
     * Inserts a word into the trie.
     */
    public void insert(String word) {
        Trie t = find(word, true);
        t.isEnd = true;
    }

    /**
     * Returns if the word is in the trie.
     */
    public boolean search(String word) {
        Trie t = find(word, false);
        return t != null && t.isEnd;
    }

    /**
     * Returns if there is any word in the trie that starts with the given prefix.
     */
    public boolean startsWith(String prefix) {
        Trie t = find(prefix, false);
        return t != null;
    }

    private Trie find(String word, boolean insertMode) {
        Trie t = this;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (t.child[index] == null) {
                if (insertMode) {
                    t.child[index] = new Trie();
                } else {
                    return null;
                }
            }
            t = t.child[index];
        }
        return t;
    }
}

```

##### 547. 省份数量
这个一看就是使用并查集，直接套并查集模板。

```
class Solution {
    public int findCircleNum(int[][] isConnected) {
        if(isConnected==null||isConnected.length==0)return 0;
        UnionFind unionFind = new UnionFind(isConnected.length);
        for (int i = 0; i < isConnected.length; i++) {
            for (int i1 = 0; i1 < isConnected[i].length; i1++) {
                if(isConnected[i][i1]==1){
                    unionFind.union(i,i1);
                }
            }
        }
        return unionFind.count;
    }
}
class UnionFind {
    public int count = 0;
    private int[] parent;

    public UnionFind(int n) {
        this.count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }

     public int find(int p) {
        if(parent[p] == p)
            return p;
        return parent[p] = find(parent[p]);
    }

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;
        parent[rootP] = rootQ;
        count--;
    }
}
```
##### 200. 岛屿数量
##### 这题可以使用dfs来做，然后看了课程发现也可以使用并查集来做。这里没用到秩去优化，但是有看到别人使用了，还不是很清楚为什么要用秩。

```
class Solution {
    private static int[][] fanxiang = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int nr = grid.length;
        int nc = grid[0].length;

        UnionFind uf = new UnionFind(grid);
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    grid[r][c] = '0';
                    for (int k = 0; k < 4; k++) {
                        int newx = r + fanxiang[k][0];
                        int newy = c + fanxiang[k][1];
                        if(inArea(newx,newy,grid)&&grid[newx][newy]=='1'){
                            uf.union(r,c,newx,newy,nc);
                        }
                    }
                }
            }
        }

        return uf.getCount();
    }
    private boolean inArea(int x, int y,char[][] grid) {
        if (x > -1 && x < grid.length && y > -1 && y < grid[0].length)
            return true;
        return false;
    }
}
class UnionFind {
    public int getCount() {
        return count;
    }

    private int count = 0;
    private int[] parent;
    public UnionFind(char[][] grid) {
        count = 0;
        int m = grid.length;
        int n = grid[0].length;
        parent = new int[m * n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    parent[i * n + j] = i * n + j;
                    ++count;
                }
            }
        }
    }

    public int find(int p) {
        if(parent[p] == p)
            return p;
        return parent[p] = find(parent[p]);
    }
    public void union(int p, int q,int i,int j,int n) {
        int tmp1 = p*n+q;
        int tmp2 = i*n+j;
        int rootP = find(tmp1);
        int rootQ = find(tmp2);
        if (rootP == rootQ) return;
        parent[rootP] = rootQ;
        count--;
    }
}
```

##### 130. 被围绕的区域
之前使用的是dfs，听完老师的课使用了并查集来做。先把外围的O链接到一个虚拟的点，然后再把所有和O点相连的都的找到其并查集。最后把非外围的连通量都改为X即可。
```
public class Solution {
    private int m, n;
    public void solve(char[][] board) {
        int rows = board.length;
        if (rows == 0) {
            return;
        }
        int cols = board[0].length;
        if (cols == 0) {
            return;
        }
        m = rows;
        n = cols;

        UnionFind unionFind = new UnionFind(rows * cols + 1);
        int dummyNode = rows * cols;

        // 填写第 1 行和最后一行
        for (int j = 0; j < cols; j++) {
            if (board[0][j] == 'O') {
                unionFind.union(getIndex(0, j, cols), dummyNode);
            }
            if (board[rows - 1][j] == 'O') {
                unionFind.union(getIndex(rows - 1, j, cols), dummyNode);
            }
        }

        // 填写第 1 列和最后一列
        for (int i = 1; i < rows - 1; i++) {
            if (board[i][0] == 'O') {
                unionFind.union(getIndex(i, 0, cols), dummyNode);
            }
            if (board[i][cols - 1] == 'O') {
                unionFind.union(getIndex(i, cols - 1, cols), dummyNode);
            }
        }


        int[][] directions = new int[][]{ {0, 1}, {1, 0}};
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (board[i][j] == 'O') {
                    for (int[] direction : directions) {
                        int newX = i + direction[0];
                        int newY = j + direction[1];
                        if (inArea(newX,newY) && board[newX][newY] == 'O') {
                            unionFind.union(getIndex(i, j, cols), getIndex(newX, newY, cols));
                        }
                    }
                }
            }
        }


        for (int i = 1; i < rows - 1; i++) {
            for (int j = 0; j < cols - 1; j++) {
                if (board[i][j] == 'O') {
                    if (!unionFind.isConnected(getIndex(i, j, cols), dummyNode)) {
                        board[i][j] = 'X';
                    }
                }
            }
        }
    }

    private int getIndex(int x, int y, int cols) {
        return x * cols + y;
    }
    private boolean inArea(int x, int y) {
        if (x > -1 && x < m && y > -1 && y < n)
            return true;
        return false;
    }

    class UnionFind {

        private int[] parent;

        public UnionFind(int n) {
            this.parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public boolean isConnected(int x, int y) {
            return find(x) == find(y);
        }

        public int find(int x) {
            while (x != parent[x]) {
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        public void union(int x, int y) {
            int xRoot = find(x);
            int yRoot = find(y);
            if (xRoot == yRoot) {
                return;
            }
            parent[xRoot] = yRoot;
        }
    }
}

```

##### 36. 有效的数独

```
class Solution {
    public boolean isValidSudoku(char[][] board) {
        HashSet<Integer>[] row = new HashSet[9];
        HashSet<Integer>[] col = new HashSet[9];
        HashSet<Integer>[] box = new HashSet[9];
        for (int i = 0; i < 9; i++) {
            row[i] = new HashSet<Integer>();
            col[i] = new HashSet<Integer>();
            box[i] = new HashSet<Integer>();
        }
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char num = board[i][j];
                if (num != '.') {
                    int n = (int)num;
                    int box_index = (i / 3 ) * 3 + j / 3;
                    if(row[i].contains(n)||col[j].contains(n)||box[box_index].contains(n))
                        return false;
                    else{
                        row[i].add(n);
                        col[j].add(n);
                        box[box_index].add(n);
                    }
                }
            }
        }
        return true;
    }
}
```

##### 22. 括号生成
```
class Solution {
    private List<String> res = new LinkedList<>();
    public List<String> generateParenthesis(int n) {
        generate(0,0,n,"");
        return res;
    }
    private void generate(int left,int right,int n,String s){
        if(left==n && right==n){
            res.add(s);
            return;
        }
        if(left<n)
            generate(left+1,right,n,s+"(");
        if(left>right)
            generate(left,right+1,n,s+")");
    }
}
```
##### 127. 单词接龙
这题可以用双向bfs解决

##### 433. 最小基因变化
这题可以用双向bfs解决

##### 212. 单词搜索 II

##### 51. N 皇后
```
class Solution {
    Set<Integer> pie;
    Set<Integer> na;
    Set<Integer> su;
    int[][] map;
    List<List<String>> res ;
    public List<List<String>> solveNQueens(int n) {
        pie = new HashSet();
        na = new HashSet();
        su = new HashSet();
        map = new int[n][n];
        res= new LinkedList<List<String>>();
        setbool(0,n);
        return res;
    }
    private void setbool(int level,int n){
        //terminal
        if(level==n) {
            LinkedList<String> strings = new LinkedList<>();
            for(int i=0;i<n;i++){
                String s="";
                for(int j=0;j<n;j++){
                    if(map[i][j]==1)
                        s+="Q";
                    else
                        s+=".";
                }
                strings.add(s);
            }
            res.add(strings);
            return;
        }
        //current
        for(int i=0;i<n;i++) {
            if(!beattack(level,i)){
                map[level][i] = 1;
                setattack(level,i);
                setbool(level + 1, n);
                map[level][i] = 0;
                robackattack(level,i);
            }
        }
    }
    private boolean beattack(int x,int y){
        if(na.contains(x+y)){
            return true;
        }else if(su.contains(y)){
            return true;
        }else if(pie.contains(x-y)){
            return true;
        }
        return false;
    }
    private void robackattack(int x,int y){
        pie.remove(x-y);
        na.remove(x+y);
        su.remove(y);
    }
    private void setattack(int x,int y){
        pie.add(x-y);
        na.add(x+y);
        su.add(y);
    }
    
}
```

##### 37. 解数独