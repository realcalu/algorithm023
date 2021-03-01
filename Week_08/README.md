##### 191. 位1的个数

使用n&(n-1)可以减去最低位的1。
```
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while(n!=0){
            count++;
            n = n&(n-1);
        }
        return count;
    }
}
```

##### 231. 2的幂

如果最低位不是0，那么肯定不是2的幂。
```
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(-2147483648==n)return false;
        return (n!=0&&(n&(n-1))==0);
    }
}
```

##### 190. 颠倒二进制位
```
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int m=0;
        for (int i = 0; i < 32; i++) {
            int last = n&1;//最后一位
            m=m<<1;
            m=m|last;
            n=n>>1;
        }
        return m;
    }
}
```
##### 1122. 数组的相对排序

分为前后两段来排序
```
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        boolean[] alive = new boolean[1001];
        int[] flag = new int[1001];//计数出现的个数

        for (int i : arr2) {
            alive[i] = true;
        }
        for(int i:arr1){
            if(alive[i])
                flag[i]++;
        }
        int[] res = new int[arr1.length];
        int tmp = 0;
        //前半段结束
        for(int i: arr2){
            int i1 = flag[i];
            for (int j=0;j<i1;j++){
                res[tmp++] = i;
            }
        }
        int[] last = new int[arr1.length-tmp];
        int tmp2 = 0;
        for (int i : arr1) {
            if(!alive[i]){
                last[tmp2++] = i;
            }
        }
        Arrays.sort(last);
        for (int i : last) {
            res[tmp++] = i;
        }
        return res;
    }
}
```

##### 242. 有效的字母异位词

使用计数排序的思想。
```
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length()!=t.length())return false;
        int[] flag1 = new int[26];
        int[] flag2 = new int[26];
        char[] array = s.toCharArray();
        char[] array1 = t.toCharArray();
        for(int i=0;i<array.length;i++){
            int k = array[i] - 'a';
            int k1 = array1[i] - 'a';
            flag1[k]++;
            flag2[k1]++;
        }
        for(int i=0;i<26;i++){
            if(flag1[i]!=flag2[i])return false;
        }
        return true;
    }
}
```

##### 146. LRU 缓存机制

继承LinkedHashMap即可。
```
class LRUCache extends LinkedHashMap<Integer,Integer> {
    private int capacity;
    public LRUCache(int capacity) {
        super(capacity,0.75F,true);
        this.capacity = capacity;

    }

    public int get(int key) {
        return getOrDefault(key,-1);
    }

    public void put(int key, int value) {
        super.put(key,value);
    }
    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}

```

##### 1244. 力扣排行榜

用基数排序的原理就可以解决，其实有优化空间。
```
class Leaderboard {
    int[] res ;

    public Leaderboard() {
        res = new int[10001];
    }

    public void addScore(int playerId, int score) {
        res[playerId] += score;
    }

    public int top(int K) {
        int[] tt = res.clone();
        Arrays.sort(tt);
        int sum = 0;
        int k=0;
        for (int i = tt.length-1; k<K ; i--) {
            sum+=tt[i];
            k++;
        }
        return sum;
    }

    public void reset(int playerId) {
        res[playerId] = 0;
    }
}
```


##### 51. N 皇后
```
class Solution {
    private HashSet<Integer> pie = new HashSet<>();
    private HashSet<Integer> na = new HashSet<>();
    private HashSet<Integer> col = new HashSet<>();
    private int[][] map ;

    List<List<String>> res;
    public List<List<String>> solveNQueens(int n) {
        map = new int[n][n];
        res = new LinkedList<>();
        dfs(0,n);
        return res;

    }
    private void dfs(int rank,int n){
        if(rank==n){
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
        for(int i=0;i<n;i++){
            if(!isAttack(rank,i)){
                setAttack(rank,i);
                map[rank][i] = 1;
                dfs(rank+1,n);
                rollback(rank,i);
                map[rank][i] = 0;
            }
        }
    }


    private void rollback(int x,int y){
        pie.remove(y-x);
        na.remove(x+y);
        col.remove(y);
    }

    private boolean isAttack(int x,int y){
        if(pie.contains(y-x))
            return true;
        if(na.contains(x+y))
            return true;
        if(col.contains(y))
            return true;
        return false;
    }
    private void setAttack(int x,int y){
        pie.add(y-x);
        na.add(x+y);
        col.add(y);
    }
}
```


##### 52. N皇后 II

N皇后的套路修改一下就可以了。
```
class Solution {
    
    private HashSet<Integer> pie = new HashSet<>();
    private HashSet<Integer> na = new HashSet<>();
    private HashSet<Integer> col = new HashSet<>();
    private int[][] map ;
    int res;


    public int totalNQueens(int n) {
        map = new int[n][n];
        dfs(0,n);
        return res;
    }
    private void dfs(int rank,int n){
        if(rank==n){
            res++;
            return;
        }
        for(int i=0;i<n;i++){
            if(!isAttack(rank,i)){
                setAttack(rank,i);
                map[rank][i] = 1;
                dfs(rank+1,n);
                rollback(rank,i);
                map[rank][i] = 0;
            }
        }
    }


    private void rollback(int x,int y){
        pie.remove(y-x);
        na.remove(x+y);
        col.remove(y);
    }

    private boolean isAttack(int x,int y){
        if(pie.contains(y-x))
            return true;
        if(na.contains(x+y))
            return true;
        if(col.contains(y))
            return true;
        return false;
    }
    private void setAttack(int x,int y){
        pie.add(y-x);
        na.add(x+y);
        col.add(y);
    }
}
```