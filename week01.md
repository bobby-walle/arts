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

> 文章主要说明在Android项目开发时会有很多的重复性工作，提供了一个方式让我们不再做这些重复性的工作。
  可以通过Live Templates的定制，在Android studio中设置模版内容，将重复性的内容作为模版，进行快速开发。

## Tip
更多时候，面对负责的分歧可以沉心思考，但是对于简单且显而易见的分歧会很容易激动。而激动就会导致不理智和不全面，不能回顾问题的全貌，导致简单问题解决起来反而更耗时。

Tips：先分析核心诉求，不要迷惑于表象

## Share
<!-- 本来是想着分享自己的文章和观点，但是不得不承认之前的计划太乐观自己的能力，也算是给自己上了一课。
总体目标不变，后续会有自己的文章和观点分享。-->

本周分享一个最近自己一直在使用的工具吧，一个网页端的音乐播放器，看起来逼格满满
[播放](https://musicforprogramming.net/)

