## Moore's voting最大投票算法

找出数组中频率超过半数的元素

时间复杂度：O(n)

### 算法思想

> 每次都找出一对不同的元素，从数组中删掉，直到数组为空或只有一种元素。 不难证明，如果存在元素e出现频率超过半数，那么数组中最后剩下的就只有e。

当然，最后剩下的元素也可能并没有出现半数以上。比如说数组是[1, 2, 3]，最后剩下的3显然只出现了1次，并不到半数。排除这种false positive情况的方法也很简单，只要保存下原始数组，最后扫描一遍验证一下就可以了。

```java
class Solution {
    public int majorityElement(int[] nums) {
        /**O(nlogn)
        Arrays.sort(nums);
	    int len = nums.length;
	    return nums[len/2 + 1];
        */
        int major = nums[0];
        int count = 1;
        for(int i=1; i<nums.length; i++){
            if(count == 0){
                count++;
                major = nums[i];
            }else if(major == nums[i]){
                count++;
            }else{
                count--;
            }
        }
        return major;
        
    }
}
```




参考：
https://blog.csdn.net/huanghanqian/article/details/74188349