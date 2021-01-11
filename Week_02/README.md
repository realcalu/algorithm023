## 有效的字母异位词
可以用hashmap也可以使用这种数组，数组开销会小一点但不够通用。
```
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] chsMap = new int[26];
        
        for(char c: s.toCharArray()) {
            chsMap[c - 'a']++;
        }
        
        for(char c: t.toCharArray()) {
            chsMap[c- 'a']--;
        }
        
        for(int cnt: chsMap) {
            if(cnt != 0) {
                return false;
            }
        }
        
        return true;
    }
}
```

## 两数之和
使用hashmap可以很快解答
```
class Solution {
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i< nums.length; i++) {
            if(map.containsKey(target - nums[i])) {
                return new int[] {map.get(target-nums[i]),i};
            }
            map.put(nums[i], i);
        }
    }
}
```

N 叉树的前序遍历，使用递归吧，使用非递归也会写。

```
class Solution {
    private List<Integer> res = new ArrayList<Integer>();
    public List<Integer> preorder(Node root) {
        if(root==null)
            return res;
        res.add(root.val);
        for (int i = 0; i < root.children.size(); i++) {
            preorder(root.children.get(i));
        }
        return res;
    }
}
```
## 字母异位词

三年前的解法

```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
      List<List<String>> list = new ArrayList<List<String>>();
		HashMap<String, Integer> hashMap = new HashMap<String, Integer>();
		int num = 0;
		for (int i = 0; i < strs.length; i++) {
			char[] ca = strs[i].toCharArray();
			Arrays.sort(ca);
			Integer def = hashMap.get(String.valueOf(ca));
			// 如果map里面有该字符串
			if (def != null) {
				List<String> arr = list.get(def);
				arr.add(strs[i]);
			} else {
				ArrayList<String> arr = new ArrayList<String>();
				arr.add(strs[i]);
				list.add(num, arr);
				hashMap.put(String.valueOf(ca), num++);
			}
		}
		return list;
    }
}
```
现在的解法，使用排序，我写的代码太原始了，看了别人的题解发现很简单,可以使用groupingby方法

```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        return new ArrayList<>(Arrays.stream(strs).collect(Collectors.groupingBy(str -> {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            return new String(array);
        })).values());
    }
}
```


## 二叉树的中序遍历
```
class Solution {
    private List<Integer> res = new LinkedList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root==null)
            return res;
        inorderTraversal(root.left);
        res.add(root.val);
        inorderTraversal(root.right);
        return res;
    }
}
```

## 二叉树的前序遍历
```
class Solution {
    private List<Integer> res = new LinkedList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root==null)
            return res;
        res.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return res;
    }
}
```

## N 叉树的层序遍历
```
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                level.add(node.val);
                queue.addAll(node.children);
            }
            result.add(level);
        }
        return result;
    }
}
```
## 丑数 
这个超时了，所以肯定不可以用暴力解法。

```
public int nthUglyNumber(int n) {
        int[] k = new int[n];
        int f=0;
        int i = 1;
        while (f<n){
            if(determine(i)){
                k[f] = i;
                f++;
            }
            i++;
        }
        return k[n-1];
    }
    boolean determine(int num) {
        if (num > 0 && num <= 6) {return true;}
        else if (num <= 0) {return false;}

        while (num % 3 == 0) {num /= 3;}
        while (num % 5 == 0) {num /= 5;}
        while (num % 2 == 0) {num /= 2;}

        return (num == 1);
    }
```
使用动态规划
```
int nthUglyNumber(int n) {
        int two = 0, three = 0, five = 0;
        int[] dp = new int[n];
        dp[0] = 1; // dp 初始化

        for (int i = 1; i < n; ++i) {
            int t1 = dp[two] * 2, t2 = dp[three] * 3, t3 = dp[five] * 5;
            dp[i] = Math.min(Math.min(t1, t2), t3);

            if (dp[i] == t1) {++ two;}
            if (dp[i] == t2) {++ three;}
            if (dp[i] == t3) {++ five;}
        }

        return dp[n - 1];
    }
```

看了一个人的代码，使用了一个静态变量存所有的元素（一开始就计算出来），这样就不用每次进行计算了，可以方便的取出，改了一下变成
```
class Solution {
    public static Ugly ug = new Ugly();
    int nthUglyNumber(int n) {
        return ug.res[n-1];
    }
}
class Ugly{
    public int[] res = new int[1690];
    Ugly(){
        int a=0,b=0,c=0;
        res[0] = 1;
        for(int i=1;i<1690;i++){
            res[i] = getMin(res[a]*2,res[b]*3,res[c]*5);
            if(res[i] == res[a]*2) a++;
            if(res[i] == res[b]*3) b++;
            if(res[i] == res[c]*5) c++;
        }
    }
    private static int getMin(int a,int b,int c){
        return Math.min(Math.min(a,b),c);
    }
}

```

## 前 K 个高频元素

先使用map排序，然后使用大顶堆，最后输出。
这里用了一个结构体，整体脉络会清晰一些

```
class Solution {
    class node implements Comparable<node>{
        int key;
        int num;
        public node(int key,int num){
            this.key = key;
            this.num = num;
        }

        @Override
        public int compareTo(node o) {
            return o.num-this.num;
        }
    }

    public int[] topKFrequent(int[] nums, int k) {
         Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        PriorityQueue<node> list = new PriorityQueue<>(k);
        for (Integer key : map.keySet()) {
            Integer val = map.get(key);
            list.add(new node(key,val));
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = list.poll().key;
        }
        return res;
    }
}
```