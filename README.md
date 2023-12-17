# 03SHash

242.有效的字母异位词

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1: 输入: s = "anagram", t = "nagaram" 输出: true

解题思路：查找某某是否在一个数组中出现过，或者出现的次数，经常会考虑用hash，因为 hash可以0(1)的时间复杂度。哈希一般有三种方式：
1.  数组：如果查询范围有限如果26个字母，100以内的数字。可以考虑直接开一个这么大size的数组，然后loop查询内容，他们的值就是数组的下标，重复就++，或者--；
2.  hash set: 如果查询范围是类似integer这种大范围，只需要保存一个variable（key）。
3.  hash map:如果查询范围是类似integer这种大范围， 并且需要保存两个variables（key, value),比如值和下标，或者值和出现次数。
   这题因为是有限的26个字母，所以可以直接开一个26位数组，然后loop长的string，把string 的每个char都当做新数组的下标（ch - 'a')，出现次数作为数值下标对应的值+1,或者-1；最后判断数组是否全部为0；

代码实现：
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] record = new int[26];

        for (int i = 0; i < s.length(); i++) {
            record[s.charAt(i) - 'a']++;     // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
        }

        for (int i = 0; i < t.length(); i++) {
            record[t.charAt(i) - 'a']--;
        }
        
        for (int count: record) {
            if (count != 0) {               // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        return true;                        // record数组所有元素都为零0，说明字符串s和t是字母异位词
    }
}

349. 两个数组的交集

题意：给定两个数组，编写一个函数来计算它们的交集。

示例 1: 输入: nums1 = [1,2,2,1], nums2 = [2,2] 输出: [2]

解题思路，因为题目给出的数组数值范围很大，然后只要将查询到的数值保存，涉及一个variable，所以只要用到set. 1.用一个set来保存其中一个nums1, 然后loop另外一个nums2, 当前值如果set包含了，就把这个值存起来到set里面。最后把存好结构的set转化为array。

代码实现：
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) {
            return new int[0];
        }
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resSet = new HashSet<>();
        //遍历数组1
        for (int i : nums1) {
            set1.add(i);
        }
        //遍历数组2的过程中判断哈希表中是否存在该元素
        for (int i : nums2) {
            if (set1.contains(i)) {
                resSet.add(i);
            }
        }
        int[] arr = new int[resSet.size()];
        int j = 0;
        for(int i : resSet){
            arr[j++] = i;
        }
        
        return arr;
    }
}

第202题. 快乐数

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。

如果 n 是快乐数就返回 True ；不是，则返回 False 。

示例：

输入：19
输出：true
解释：
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1

解题思路： 这个算法计算的结构是无限循环或者最终变为1，洗个一个方法来计算下一次运行得到的结构，只要结果 != 1，就继续往下求, 因此可以用set来保存每次的结果，一旦出现了重复说明出现了循环，就可以判断不是快乐数。
         取余的计算也是常用的，一般while( n > 0{ int digit = n % 10; n = n / 10}

代码实现：class Solution {
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }
        return n == 1;
    }

    private int getNextNumber(int n) {
        int res = 0;
        while (n > 0) {
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }
}

1. 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9

所以返回 [0, 1]

解题思路：因为题目给出的数组数值范围很大，然后查询的是具体数值，但是最后要保存的是下标，所以就需要key 和 value两个variables，因此用map. 又因为我们要查询的是两个数相加等于target，因此是对于具体值在操作，map里面查询是否包含智能用containsKey,所以数值是key,小标是value。 把每一个值，跟下标都作为一个pair存到map，然后每次存新的之前检查一下，map是否包含了target-新的key,包含的话说明找了一组，当前的小标是第二个，然后对应的target-新的key这个的下标是第一次出现的下标。

代码实现：
public int[] twoSum(int[] nums, int target) {
    int[] res = new int[2];
    if(nums == null || nums.length == 0){
        return res;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for(int i = 0; i < nums.length; i++){
        int temp = target - nums[i];   // 遍历当前元素，并在map中寻找是否有匹配的key
        if(map.containsKey(temp)){
            res[1] = i;
            res[0] = map.get(temp);
            break;
        }
        map.put(nums[i], i);    // 如果没找到匹配对，就把访问过的元素和下标加入到map中
    }
    return res;
}

第454题.四数相加II

给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

例如:

输入:

A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]
输出: 2

解题思路：
四个数字的相加，可以考虑为两两相加后的值再相加，就变成了two sum问题，分别用两个for loop 来求解AB的sum，因为还要考虑部分数字重复出现，所以用map, key是要累计的值sum1，value是出现的次数。然后第一个for loop 存储，第二个for loop用于查找，查到map.containsKey(-sum1)，把count +=map.get(-sum1) sum出现的次数。

代码实现：
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int count = 0;
        for(int num1 : nums1){
            for(int num2 : nums2){
                map.put(num1+num2, map.getOrDefault(num1+num2,0)+1);
            }
        }
        for(int num3 : nums3){
            for(int num4 : nums4){
                if(map.containsKey(-(num3+num4))){
                    count = count + map.get(-(num3+num4));
                }
            }
        }
        return count;
    }
}

第15题. 三数之和
力扣题目链接(opens new window)

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意： 答案中不可以包含重复的三元组。

示例：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为： [ [-1, 0, 1], [-1, -1, 2] ]

解题思路，可以使用hasH，先两层loop计算sum，存到set，再forloop一次查询等于0，但是还有一些去重很麻烦。
也可以使用指针法，forloop 遍历的时候，设置一个left指针为1，设置一个right指针为size-1，每次都比较这3个数和大于0: 所以和太大，right需要变小，小于0: 所以和太小，left需要变大，如果等于0的话，找到一组，可以存起来先，然后避免后面还有可以用left++ ? left / right + ?right来跳过相同的。去了left和right的重，还要考虑i是否重复，可以用i ？i-1来检验是不是已经出现过了。

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
	// 找出a + b + c = 0
        // a = nums[i], b = nums[left], c = nums[right]
        for (int i = 0; i < nums.length; i++) {
	    // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (nums[i] > 0) { 
                return result;
            }

            if (i > 0 && nums[i] == nums[i - 1]) {  // 去重a
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;
            while (right > left) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);
                    temp.add(nums[left]);
                    temp.add(nums[right]);
                    result.add(temp);
		    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    right--; 
                    left++;
                }
            }
        }
        return result;
    }
}


