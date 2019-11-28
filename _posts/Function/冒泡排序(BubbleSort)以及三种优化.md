---
title: 冒泡排序(BubbleSort)以及三种优化
date: 2019-10-25 15:14:42
tags:
    - 算法
---
## 冒泡算法思路
冒泡排序 - 依次比较相邻两元素，若前一元素大于后一元素则交换之，直至最后一个元素即为最大；然后重新从首元素开始重复同样的操作，直至倒数第二个元素即为次大元素；依次类推。如同水中的气泡，依次将最大或最小元素气泡浮出水面。

### 一般写法
```php
    public function bubbleSort($arr)
    {
        $len = count($arr)-1;
        //该层循环控制 需要冒泡的轮数
        for($i = 0;$i < $len;$i++)
        { //该层循环用来控制每轮需要比较的次数
            for($j = 0;$j < $len-$i;$j++)
            {
                if($arr[$j] > $arr[$j+1])
                {
                    $tmp = $arr[$j+1];
                    $arr[$j+1] = $arr[$j];
                    $arr[$j] = $tmp;
                }
            }
        }
        var_dump($arr);
    }
```
### 优化一
> 假设我们现在排序arr[]={1,2,3,4,5,6,7,8,10,9}这组数据，按照上面的排序方式，
  第一趟排序后将10和9交换已经有序，接下来的8趟排序就是多余的，什么也没做。
  所以我们可以在交换的地方加一个标记，如果那一趟排序没有交换元素，说明这组数据已经有序，不用再继续下去。

代码实现：
```php
    public function bubbleSort($arr)
    {
        $len = count($arr)-1;
        $flag = 0; //用于标记
        
        //该层循环控制 需要冒泡的轮数
        for($i = 0;$i < $len;$i++)
        {   
            $flag = 0; 
            //该层循环用来控制每轮需要比较的次数
            for($j = 0;$j < $len-$i;$j++)
            {   
                if($arr[$j] > $arr[$j+1])
                {
                    $tmp = $arr[$j+1];
                    $arr[$j+1] = $arr[$j];
                    $arr[$j] = $tmp;
                    $flag = 1; //添加标记
                }
            }
            //若无标记元素则已近有序，结束循环
            if($flag == 0)
            {
                break;
            }
        }
        var_dump($arr);
    }
```
### 优化二
> 优化一仅仅适用于连片有序而整体无序的数据(例如：1， 2，3 ，4 ，7，6，5)。但是对于前面大部分是无序而后边小半部分有序的数据(1，2，5，7，4，3，6，8，9，10)排序效率也不可观，
对于种类型数据，我们可以继续优化。既我们可以记下最后一次交换的位置，后边没有交换，必然是有序的，然后下一次排序从第一个比较到上次记录的位置结束即可。

![](http://img-blog.csdn.net/2018062700091521?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhbnNpb256/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
代码实现：
```php
    public function bubbleSort($arr)
    {
        $len = count($arr)-1;
        $k = $len;
        $flag = 0; //用于标记
        
        //该层循环控制 需要冒泡的轮数
        for($i = 0;$i < $len;$i++)
        { 
            $flag = 0; 
            //该层循环用来控制每轮需要比较的次数
            for($j = 0;$j < $k;$j++)
            {
                if($arr[$j] > $arr[$j+1])
                {
                    $tmp = $arr[$j+1];
                    $arr[$j+1] = $arr[$j];
                    $arr[$j] = $tmp;
                    $flag = $j; //添加标记,并记录最后交换的位置
                }
            }
            //若无标记元素则已近有序，结束循环
            if($flag == 0)
            {
                break;
            }
            $k = $flag;
        }
        var_dump($arr);
    }
```
### 优化三
> 优化二的效率有很大的提升，还有一种优化方法可以继续提高效率。
  大致思想就是一次排序可以确定两个值，正向扫描找到最大值交换到最后，反向扫描找到最小值交换到最前面。
  例如：排序数据1，2，3，4，5，6，0

代码实现：
```php
    public function bubbleSort($arr)
    {
        $len = count($arr)-1;
        $k = $len;
        $flag = 0; //用于标记
        $n = 0;//同时找最大值的最小需要两个下标遍历
        
        //该层循环控制 需要冒泡的轮数
        for($i = 0;$i < $len;$i++)
        { 
            $flag = 0;
            //正向找最大值
            for($j = 0;$j < $k;$j++)
            {
                if($arr[$j] > $arr[$j+1])
                {
                    $tmp = $arr[$j+1];
                    $arr[$j+1] = $arr[$j];
                    $arr[$j] = $tmp;
                    $flag = $j; //添加标记,并记录最后交换的位置
                }
            }
            //若无标记元素则已近有序，结束循环
            if($flag == 0)
            {
                break;
            }
            $k = $flag;
            //反向找最小值
            for($j = $k;$j > $n;$j--)
            {
                if($arr[$j] < $arr[$j-1])
                {
                    $tmp = $arr[$j-1];
                    $arr[$j] = $arr[$j-1];
                    $arr[$j] = $tmp;
                    $flag = $j-1; //添加标记,并记录最后交换的位置
                }
            }
            $n++;
            if($flag == 0)
            {
                break;
            }
        }
        var_dump($arr);
    }
```