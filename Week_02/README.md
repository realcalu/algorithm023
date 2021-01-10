有效的字母异位词，可以用hashmap也可以使用这种数组，数组开销会小一点但不够通用。
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

两数之和
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

N 叉树的前序遍历

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