## 26题
##### 这个是三年前第一次提交的，代码很丑
```
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length;
		int fr = 0, fl = 0, k = 0;
		while (fr < len) {
			if (fr == fl && fr == len - 1) {
				nums[k++] = nums[fr];
				break;
			} else {
				if (nums[fl] == nums[fr]) {
					if(fr==len-1){
						nums[k++] = nums[fl];
						break;
					}
					fr++;
					continue;
				}
				nums[k++] = nums[fl];
				fl = fr;
			}
		}
		return k;
    }
}
```

这个是这一次提交的，代码精简很多而且时间复杂度低

```
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length==0)return 0;
        int i=0,j =1;
        while(j<nums.length) {
            if(nums[i]!=nums[j]) {
                nums[i+1] = nums[j];
                i++;
            }
            j++;
        }
        return i+1;
    }
}
```
## 189题
开辟多一个数组很快就写完，使用原地换位做了挺久也没解决

```
public class Solution {
    public void rotate(int[] nums, int k) {
        int[] a = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            a[(i + k) % nums.length] = nums[i];
        }
        for (int i = 0; i < nums.length; i++) {
            nums[i] = a[i];
        }
    }
}
```
这个的解法很快速，这是外网上的

```
public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

public void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
```
## 21题
这个是三年前写的代码
```
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null)
			return l2;
		if (l2 == null)
			return l1;
		if (l1.val < l2.val) {
			l1.next =mergeTwoLists(l1.next, l2);
			return l1;
		} else {
			l2.next = mergeTwoLists(l1, l2.next);
			return l2;
		}
    }
}
```
## 88题
三年前的代码

```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] tmp = new int[m + n];
		for (int i = 0; i < m; i++) {
			tmp[i] = nums1[i];
		}
		int t = 0;
		for (int i = m; i < m + n; i++) {
			tmp[i] = nums2[t++];
		}
		int mid = m;
		int l = 0, k = mid;// 左，右
		for (int i = 0; i < m + n; i++) {
			if (l >= mid)
				nums1[i] = tmp[k++];
			else if (k >= m + n)
				nums1[i] = tmp[l++];
			else if (tmp[l] <= tmp[k])
				nums1[i] = tmp[l++];
			else
				nums1[i] = tmp[k++];

		}
    }
}
```
现在写的代码
```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int len1 = m - 1;
        int len2 = n - 1;
        int len = m + n - 1;
        while(len1 >= 0 && len2 >= 0) {
            if(nums1[len1]>nums2[len2]){
                nums1[len] = nums1[len1];
                len1--;
                len--;
            }else{
                nums1[len] = nums2[len2];
                len2--;
                len--;
            }
        }
        System.arraycopy(nums2, 0, nums1, 0, len2 + 1);
    }
}
```

## 1题
三年前暴力求解

```
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        int nn[] = new int[2];
		for(int i=0;i<nums.length;i++){
			for(int j=i+1;j<nums.length;j++)
			{
				if(nums[i]+nums[j]==target){
					nn[0]=i;
					nn[1]=j;
				}
			}
		}
		return nn;
    }
}
```
现在使用hashmap

```
public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i< nums.length; i++) {
            if(map.containsKey(target - nums[i])) {
                return new int[] {map.get(target-nums[i]),i};
            }
            map.put(nums[i], i);
        }
    }
```


## 283题
o(n)的时间复杂度，使用双指针

```
class Solution {
    public void moveZeroes(int[] nums) {
        int len = nums.length;
        if(len<=1){
            return;
        }
        int i=0,j=0;
        while(i<len){
            //找到为0的数字
            while ( j < len && nums[j] != 0)
                j++;
            i = j+1;
            //找到不为0的数字
            while ( i < len-1 && nums[i] == 0 )
                i++;
            if(i>=len)
                break;
            nums[j] = nums[i];
            nums[i] = 0;
            i++;
            j++;
        }
    }
}
```
看了一下别人写的代码，大家说是该方法类似于快排的思路，于是我修改了一下我的代码，看起来没那么凌乱。

```
public void moveZeroes(int[] nums) {
        if(nums==null)
            return;
        int len = nums.length;
        int i=0,j=0;
        for(;i<len;i++){
            if(nums[i]!=0){
                int tmp = nums[j];
                nums[j] = nums[i];
                nums[i] = tmp;
                j++;
            }
        }
    }
```



## 66题
```
class Solution {
    public int[] plusOne(int[] digits) {
        int addnum = 1;
        for(int i=digits.length-1;i>=0;i--){
            int stay = (digits[i]+addnum)%10;
            addnum = (digits[i]+addnum)/10;
            digits[i] = stay;
            if(addnum==0)
                return digits;
        }
        if(addnum>=1){
            int[] res = new int[digits.length+1];
            res[0] = addnum;
            return res;
        }
        return digits;
    }
}
```

## 641题

```
class MyCircularDeque {
    int count;
    int size;
    DoubleListNode head;
    DoubleListNode tail;

    /**
     * Initialize your data structure here. Set the size of the deque to be k.
     */
    public MyCircularDeque(int k) {
        this.size = k;
        this.count = 0;
    }

    /**
     * Adds an item at the front of Deque. Return true if the operation is successful.
     */
    public boolean insertFront(int value) {
        if (count == size)
            return false;
        DoubleListNode node = new DoubleListNode(value);
        if (count == 0)
            this.head = this.tail = node;
        else {
            node.next = head;
            head.pre = node;
            this.head = node;
        }
        count++;
        return true;
    }

    /**
     * Adds an item at the rear of Deque. Return true if the operation is successful.
     */
    public boolean insertLast(int value) {
        if (count == size)
            return false;
        DoubleListNode node = new DoubleListNode(value);
        if (count == 0) {
            this.head = this.tail = node;
        } else {
            node.pre = tail;
            tail.next = node;
            tail = node;
        }
        count++;
        return true;
    }

    /**
     * Deletes an item from the front of Deque. Return true if the operation is successful.
     */
    public boolean deleteFront() {
        if (count == 0 || size == 0)
            return false;
        else if (count == 1)
            head = tail = null;
        else {
            DoubleListNode tmp = this.head.next;
            tmp.pre = null;
            head = null;
            this.head = tmp;
        }
        count--;
        return true;
    }

    /**
     * Deletes an item from the rear of Deque. Return true if the operation is successful.
     */
    public boolean deleteLast() {
        if (count == 0 || size == 0)
            return false;
        else if (count == 1)
            head = tail = null;
        else {
            DoubleListNode tmp = this.tail.pre;
            tmp.next = null;
            tail = tmp;
        }
        count--;
        return true;

    }

    /**
     * Get the front item from the deque.
     */
    public int getFront() {
        return this.head == null ? -1 : this.head.val;
    }

    /**
     * Get the last item from the deque.
     */
    public int getRear() {
        return this.tail == null ? -1 : this.tail.val;
    }

    /**
     * Checks whether the circular deque is empty or not.
     */
    public boolean isEmpty() {
        return this.count == 0;
    }

    /**
     * Checks whether the circular deque is full or not.
     */
    public boolean isFull() {
        return this.count == this.size;
    }

    class DoubleListNode {
        DoubleListNode next, pre;
        int val;

        DoubleListNode(int val) {
            this.val = val;
            this.next = null;
            this.pre = null;
        }
    }
}
```




## 42题
这个题只想到了用暴力解开，用双指针的时候脑子没转过来后来看了题解。发现是这样做的，第一遍有点失败，对于leetcode的hard题目还是不太会做

```
public int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int ans = 0;
    int left_max = 0, right_max = 0;
    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= left_max) {
                left_max = height[left];
            } else {
                ans += (left_max - height[left]);
            }
            ++left;
        } else {
            if (height[right] >= right_max) {
                right_max = height[right];
            } else {
                ans += (right_max - height[right]);
            }
            --right;
        }
    }
    return ans;
}

```