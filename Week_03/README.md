236. 二叉树的最近公共祖先

```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        contain(root,p,q);
        return res;
    }
    private boolean contain(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null)
            return false;
        boolean left = contain(root.left,p,q);
        boolean right = contain(root.right,p,q);
        if((left&&right)||((root==p||root==q)&&(left||right))){
            res = root;
        }
        return left||right||(root==p||root==q);
    }
    TreeNode res;
}
```

46题：全排列

```
class Solution {
    HashSet<Integer> set;
    List<List<Integer>> res = new LinkedList<List<Integer>>();
    public List<List<Integer>> permute(int[] nums) {
        LinkedList<Integer> integers = new LinkedList<>();
        set = new HashSet<>();
        generate(0,nums,integers);
        return res;
    }
    private void generate(int current,int[] nums, LinkedList<Integer> list){
        if(list.size()==nums.length) {
            res.add((List<Integer>) list.clone());
        }
        for(int i=0;i<nums.length&&current<nums.length;i++) {
            if(!set.contains(nums[i])) {
                list.add(nums[i]);
                set.add(nums[i]);
                generate(current + 1, nums, list);
                Integer integer = list.removeLast();
                set.remove(integer);
            }
        }
    }
}
```
这个速度太慢了！不过可以通过。


47. 全排列 II
```
class Solution {
    List<List<Integer>> res = new LinkedList<List<Integer>>();
    HashSet set = new HashSet<Integer>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        LinkedList<Integer> tmp = new LinkedList<Integer>();
        Arrays.sort(nums);
        dfs(nums,0,tmp);
        return res;
    }

    private void dfs(int[] nums ,int idx, LinkedList<Integer> perm) {
        if (idx == nums.length) {
            res.add((List<Integer>) perm.clone());
            return;
        }
        for (int i = 0; i < nums.length; ++i) {
            if (set.contains(i)||(i>0&&nums[i]==nums[i-1]&&!set.contains(i-1))) {
                continue;
            }
            perm.add(nums[i]);
            set.add(i);
            dfs(nums, idx + 1, perm);
            set.remove(i);
            perm.remove(idx);
        }
    }
}
```
这个速度也太慢了！不过可以通过。


102. 二叉树的层序遍历
```
class Solution {
    
    public List<List<Integer>> levelOrder(TreeNode root) {
        List res = new ArrayList<>();
        if(root==null)
            return res;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> collect = queue.stream().map(treeNode -> treeNode.val).collect(Collectors.toList());
            res.add(collect);
            LinkedList<TreeNode> tmp = new LinkedList<>();
            for (TreeNode treeNode : queue) {
                if (treeNode.left != null)
                    tmp.add(treeNode.left);
                if (treeNode.right != null)
                    tmp.add(treeNode.right);
            }
            queue = tmp;
        }
        return res;
    }
}
```

455. 分发饼干

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int l = 0;
        int f = 0;
        int res = 0;
        while (f<s.length&&l<g.length){
            if(g[l]<=s[f]){
                l++;
                f++;
                res++;
            }else{
                f++;
            }
        }
        return res;
    }
}
```


122. 买卖股票的最佳时机 II
```java
class Solution {
    public int maxProfit(int[] prices) {
        int sum = 0;
        for (int i = 0; i < prices.length; i++) {
            if(i+1>=prices.length)
                return sum;
            if (prices[i] < prices[i + 1]) {
                sum+=prices[i+1]-prices[i];
            }
        }
        return sum;
    }
}
```


55. 跳跃游戏
```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length==1)
            return true;
        int max=nums.length-1;
        boolean[] flag = new boolean[nums.length];
        for (int i = nums.length-2; i >=0 ; i--) {
            if(i+nums[i]>=max){
                max = i;
                flag[i] =true;
            }
        }
        return flag[0];
    }
}
```

69. x 的平方根
```java
class Solution {
        public int mySqrt(int n) {
            int ans = 0;
            int l = 0;int r = n;
            while (l<=r){
                int mid = l+(r-l)/2;
                if((long)mid*mid<=n){
                    ans = mid;
                    l = mid+1;
                }else{
                    r = mid-1;
                }
            }
            return ans;
        }
        
    }
```

367. 有效的完全平方数

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int k = mySqrt(num);
        if(k*k==num)
            return true;
        return false;
    }
    public int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }
        int ans = (int) Math.exp(0.5 * Math.log(x));
        return (long) (ans + 1) * (ans + 1) <= x ? ans + 1 : ans;
    }
}
```