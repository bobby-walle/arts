## Algorithm
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成

<code>
    
    class Solution {
        public static int removeDuplicates(int[] nums) {
            if(nums == null || nums.length <= 0){
                return 0;
            }

            if(nums.length == 1){
                return 1;
            }

            int tv = nums[0];
            int tempIndex = 0;
            int length = nums.length;
            for(int i = 1; i < length; i++){
                int num = nums[i];
                if(tv != num){
                    if(i > tempIndex + 1){
                        nums[tempIndex + 1] = num;
                        tempIndex = tempIndex + 1;
                    } else {
                        tempIndex = i;
                    }
                    tv = num;
                }
            }

            return tempIndex + 1;
        }
    }
</code>

## Review
阅读文章为[One Simple Trick That Will Save You Hours When Developing Android Apps](https://medium.com/atomic-robot/one-simple-trick-that-will-save-you-hours-when-developing-android-apps-6902c3aef226)
Live Templates
