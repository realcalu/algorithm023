这周在面试就没空做那么多题目了。
##### 387. 字符串中的第一个唯一字符

这题很简单，通过生成一个26大小的数组记录所有的字母出现的次数，然后再遍历一遍字符串，判断字符出现数量为一的即为解。
```
class Solution {
    public int firstUniqChar(String s) {
        char[] array = s.toCharArray();
        int[] flag = new int[26];
        for (char c : array) {
            flag[c-'a'] +=1;
        }
        for (int i1 = 0; i1 < array.length; i1++) {
            if(flag[array[i1]-'a']==1)
                return i1;
        }
        return -1;
    }
}
```

##### 541. 反转字符串 II
这题很简单，这里我使用一个函数reverse（）用于反转前从i开始的前k个字符。使用reverseStr作为跳板判断从哪里开始要反转。这里要注意一个点是如果k>要反转的字符串长度的话，就要整个字符串反转。
```
class Solution {
    public String reverseStr(String s, int k) {
        char[] array = s.toCharArray();
        for (int i = 0; i < s.length(); ) {
            reverse(array,i,k);
            i+=2*k;
        }
        return new String(array);
    }
    private void reverse(char[] array,int i,int k){
        int tt = i+k-1;
        if(tt>=array.length)
            tt = array.length-1;
        for (int j = i,l=tt; j <l&&l<array.length; j++,l--) {
            char tmp = array[j];
            array[j] = array[l];
            array[l] = tmp;
        }
    }

}
```

##### 151. 翻转字符串里的单词
这题可以使用库函数直接反转。
```
class Solution {
    public String reverseWords(String s) {
        // 除去开头和末尾的空白字符
        s = s.trim();
        // 正则匹配连续的空白字符作为分隔符分割
        List<String> wordList = Arrays.asList(s.split("\\s+"));
        Collections.reverse(wordList);
        return String.join(" ", wordList);
    }
}
```
当然，也可以自己写。
```
class Solution {
    public String reverseWords(String s) {
        s = s.trim();
        String[] split = s.split(" +");
        for (int i = 0,j=split.length-1; i < j; i++,j--) {
            String tmp = split[i];
            split[i] = split[j];
            split[j] =tmp;
        }
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < split.length; i++) {
            if(i!=split.length-1)
                stringBuilder.append(split[i]).append(" ");
            else
                stringBuilder.append(split[i]);
        }
        return stringBuilder.toString();
    }
}
```

##### 917. 仅仅反转字母
这题很容易，通过双指针碰撞，如果不是字母的就跳过。如果两个指针都是字母则交换。

```
class Solution {
    public String reverseOnlyLetters(String S) {
        char[] array = S.toCharArray();
        for (int i = 0,j=array.length-1; i <j;) {
            if(isletter(array[i])&&isletter(array[j])){
                char tmp =array[i];
                array[i] =array[j];
                array[j]=tmp;
                i++;
                j--;
            }else{
                if(!isletter(array[i])){
                    i++;
                }
                if(!isletter(array[j])){
                    j--;
                }
            }
        }
        return new String(array);

    }

    private boolean isletter(char c){
        if((c>='A'&&c<='Z')||(c>='a'&&c<='z')){
            return true;
        }
        return false;
    }
}
```
##### 205. 同构字符串
这题通过开辟两个char数组判断同构。当然也可以用hashmap,不过用数组开销小一些。
```
class Solution {
    public boolean isIsomorphic(String s, String t) {
        char[] sc = s.toCharArray();
		char[] tc = t.toCharArray();
		char[] map = new char[128];
		char[] test = new char[128];
		for (int i = 0; i < tc.length; i++) {
			if (map[sc[i]] == 0 && test[tc[i]] == 0) {
				map[sc[i]] = tc[i];
				test[tc[i]] = sc[i];
			} else if (map[sc[i]] == 0 && test[tc[i]] != 0) {
				return false;
			} else if (map[sc[i]] != tc[i]) {
				return false;
			}
		}
		return true;
    }
}
```

##### 680. 验证回文字符串 Ⅱ
这题我做得跟答案一样，但就是没法通过，很奇怪，先背下来吧。
```
class Solution {
    public boolean validPalindrome(String s) {
        int low = 0, high = s.length() - 1;
        while (low < high) {
            char c1 = s.charAt(low), c2 = s.charAt(high);
            if (c1 == c2) {
                ++low;
                --high;
            } else {
                return validPalindrome(s, low, high - 1) || validPalindrome(s, low + 1, high);
            }
        }
        return true;
    }

    public boolean validPalindrome(String s, int low, int high) {
        for (int i = low, j = high; i < j; ++i, --j) {
            char c1 = s.charAt(i), c2 = s.charAt(j);
            if (c1 != c2) {
                return false;
            }
        }
        return true;
    }
}
```

##### 63. 不同路径 II
这题就是要处理遇到不能走的时候就不走即可。
```
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        if(obstacleGrid[m-1][n-1]==1)
            return 0;
        int[][] dp = new int[m][n];
        init(obstacleGrid, m, n, dp);
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0)
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }

    private void init(int[][] obstacleGrid, int m, int n, int[][] dp) {
        for (int i = 0; i < m; i++) {
            if(obstacleGrid[i][0]==1)
            {
                break;
            }else{
                dp[i][0]=1;
            }
        }

        for (int i = 0; i < n; i++) {
            if(obstacleGrid[0][i]==1)
            {
                break;
            }else{
                dp[0][i]=1;
            }
        }
    }
}
```


##### 8. 字符串转换整数 (atoi)
这个看了题解，感觉很绕，自己做总是有些逻辑没走到。

```
class Solution {
    public int myAtoi(String s) {
        if (s == null || s.length() == 0) return 0;
        long res = 0;
        int i = 0;
        boolean fu = false;
        //去空格
        while (i<s.length()&&s.charAt(i) == ' ')
            i++;
        if (i == s.length())
            return (int) res;
        if (s.charAt(i) == '-' || s.charAt(i) == '+') {
            if (s.charAt(i) == '-')
                fu = true;
            i ++;
        } else if (s.charAt(i) > '9' || s.charAt(i) < '0')
            return (int) (fu ? res * -1 : res);
        while (i<s.length()) {
            int c = s.charAt(i)-'0';
            if(c<0||c>9)
                return (int) (fu ? res * -1 : res);
            else
                res=res*10+c;
            if(!fu && res > Integer.MAX_VALUE) return Integer.MAX_VALUE;
            if(fu && res * -1 < Integer.MIN_VALUE) return Integer.MIN_VALUE;
            i++;
        }
        return (int) (fu ? res * -1 : res);
    }
}
```
##### 438. 找到字符串中所有字母异位词
这题很简单，通过开辟两个数组计数，然后使用check函数判断是否一致即可。这题我3年前做过，当时的时间复杂度度是480ms，现在是8ms，进步了。
```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int[] map1 = new int[26];
        int[] map2 = new int[26];
        List res =new LinkedList<Integer>();
        if(s.length()==0||s.length()<p.length())
            return res;
        char[] array = p.toCharArray();
        for (char c : array) {
            map1[c-'a']+=1;
        }
        char[] array1 = s.toCharArray();
        for (int i = 0; i < p.length(); i++) {
            map2[array1[i]-'a']+=1;
        }
        int lo=0;
        int hi = p.length()-1;
        while (true){
            if(check(map1,map2))
                res.add(lo);
            lo++;
            hi++;
            if(hi>=s.length())
                break;
            map2[array1[lo-1]-'a']-=1;
            map2[array1[hi]-'a']+=1;
        }
        return res;
    }
    private boolean check(int[] map1,int[] map2){
        for (int i = 0; i < map1.length; i++) {
            if(map1[i]!=map2[i])
                return false;
        }
        return true;
    }
}
```

##### 5. 最长回文子串
这题使用dp做即可。老师课上有讲过。

```
class Solution {
    public String longestPalindrome(String s) {
        if(s.equals(""))
            return s;
        int len = s.length();
        int max = 0 ;
        boolean[][] dp = new boolean[len][len];
        int start = 0;
        int end = 0;
        for (int j = 0; j < len; j++) {
            for (int i = 0; i <= j; i++) {
                if (i == j)
                    dp[i][j] = true;
                else if (j == i + 1)
                    dp[i][j] = (s.charAt(i) == s.charAt(j));
                else
                    dp[i][j] = dp[i + 1][j - 1] && (s.charAt(i) == s.charAt(j));
                if(dp[i][j]&&j-i+1>max) {
                    max = j - i + 1;
                    start = i;
                    end = j;
                }
            }
        }
        System.out.println(max);
        return s.substring(start, end + 1);
    }
}
```

# 还没做的
##### 300. 最长递增子序列
##### 91. 解码方法
##### 44. 通配符匹配
##### 32. 最长有效括号
##### 115. 不同的子序列
##### 818. 赛车