## KMP算法

子序列：可以连续也可以不连续

子数组，子串：一定连续

int getIndexof(str1,str2)    用的就是KMP    O(N) c++、java的实际时间复杂度要更好

1、一个字符的信息：前面字符的最长前缀与最长后缀的匹配长度

2、0位置-1,1位置0，......

```jav
public static int getIndexOf(char[] str1,char[] str2){
	if(str1.length<str2.length || str2.length<1){
		return -1;
	}
	int i1=0,i2=0;
	int[] next = getNextArray(str2);
	while(i1<str1.length && i2<str2.length){
		if(str1[i1]==str2[i2]){
			i1++;
			i2++;
		}else if(next[i2]==-1){
			i1++;
		}else{
			i2=next[i2];
		}
	}
	return i2==str2.length? i1-i2 : -1;
}
public static int[] getNextArray(char[] str2){
	if(str2.length == 1){
		return new int[]{-1};
	}
	int[] next = new int[str2.length];
	next[0] = -1;
	next[1] = 0;
	int i=2;
	int cn=0;
	while(i<str2.length){
		if(str[i-1] == str2[cn]){
			next[i++] = ++cn;
		}else if(next[cn]>0){
			cn = next[cn];
		}else{
			next[i++] = 0;
		}
	}
	return next;
}
```

* 求原始串添加成为最短的两个原始串

* 求两树包含问题
* 怎么确定一个字符串是由另一个字符串重复多次得到的

## Manacher算法

求一个字符串的最长回文子串的长度

> 暴力解法：需区分奇字符串和偶字符串
> 技巧：在每个字符左右加个字符

3个概念

1. 半径回文数组
2. 回文右边界（所有回文半径中最靠右的位置）
3. 回文右边界的中心（第一次的位置）

* 回文右边界在左边，暴力扩（重新建立回文右边界）

* 在回文右边界里面：
  * i 对应 i' 的回文半径在回文左右边界里面，i 位置的回文半径跟 i'一样
  * i 对应 i' 的左回文半径在外面，i 位置的回文半径到回文右边界R
  * i 对应 i' 的左回文半径压线，i 位置的回文半径到回文右边界R肯定是，之后具体判断

1和4，一直是R往外扩的，O(N)

应用：添加最少的字符使其成为回文字符串

```java
public class Manacher{
    public static char[] manacherString(String str){
        char[] charArr = str.toCharArray();
        char[] res = new char[str.length()*2+1];
        int index=0;
        for(int i=0;i!=res.length;i++){
            res[i] = (i & 1)==0?'#' : charArr[index++];
        }
        return res;
    }
    public static int maxLcpsLength(String str){
        if(str == null || str.length() == 0){
            return 0;
        }
        char[] charArr = manacherString(str);
        int[] pArr = new int[charArr.length];
        int C = -1;
        int pR = -1;
        int max = Integer.MIN_VALUE;
        for(int i=0;i<charArr.length;i++){
            pArr[i] = pR > i ? Math.min(pArr[2*c-i],pR-i):1;
            while(i + pArr[i]<charArr.length && i-pArr[i] > -1){
                if(charArr[i+pArr[i]]==charArr[i-pArr[i]])
                    pArr[i]++;
                else break;
            }
            if(i+pArr[i] > pR){
                pR = i+pArr[i];
                C=i;
            }
            max = Math.max(max,pArr[i]);
        }
        return max-1;
    }
}
```

## BFPRT算法

在一个无序数组中找到一个第k小（或第k大）的数

随机取数，partation，判断，递归      O(NlogN)

BFPRTT算法就是将取数改进

1. 5个一组分组	O(1)
2. 组内排序     O(N)
3. 把每个组的中位数拿出来组成新的数组   O(N)
4. 递归调用bfprt求得新数组的中位数     T(5/N)
5. 以此中位数为依据，partation     (N)
6. 判断是否命中，未命中则递归    T(7/10N) 

> 此时若求小的，大的部分至少为3/10N，则小的部分最大为7/10N，反之亦然

于是整个过程变为：T(N) = T(5/N) + T(7/10N) + O(N)

算法导论第9章证明出：上式时间复杂度为O(N)

## 滑动窗口

窗口的定义  l ,r

双端队列（双向链表）

保持从大到小，要进的数比尾部的数小，直接进，若大于等于，则所有比该数小的队列里的数全部弹出（求得滑动窗口的最大值或最小值并不影响，因为新进的数总是在原来队列里的数的右边）

E:\BaiduNetdiskDownload\学习\java\code\basic_class_08\Code_03_SlidingWindowMaxArray.java

## 最大值减去最小值小于等于num的子数组的数量

> 给定数组arr和整数num，共返回有多少个子数组满足如下情况：max(arr[i...j]) - min(arr[i...j]) <= num
>
> max(arr[i...j])表示子数组arr[i...j]中的最大值 ，min(arr[i...j])表示子数组arr[i...j]中的最小值

> 时间复杂度O(N)

大范围达标，其中的小范围一定达标

不达标的范围，其更大的范围一定不达标

滑动窗口

E:\BaiduNetdiskDownload\学习\java\进阶\算法进阶班资料\advanced_class_01\Code_05_AllLessNumSubArray.java

## 单调栈

在一个数组中，左边离它最近比它大的，和右边离它最近比它大的

栈依次压入，底到顶满足从大到下，当不满足时，弹出（此时开始收集信息，弹出数右边离他最近比它大的数为要压入的数，栈在弹出数下面的数为左边的....）

相等的数下标压在一起

## MAXTREE

定义二叉树节点如下：
public class Node { public int value; public Node left; public Node right;
public Node(int data) { this.value = data; }
}
一个数组的MaxTree定义如下。 数组必须没有重复元素。 MaxTree是一棵二叉树，数组的每一个值对应一个二叉树节点。 包括MaxTree树在内且在其中的每一棵子树上，值最大的节点都是树的头。 给定一个没有重复元素的数组arr，写出生成这个数组的MaxTree的函数，要求如果数组长 度为N，则时间复杂度为O(N)、额外空间复杂度为O(N)。



> 可以直接用大根堆来搞定

左右都为空，根节点

有一个不为空，作为父节点

都有，小的作为父节点



不可能为森林(所有数都不同，只有一个最大值)，最多是二叉树（a在一侧只能有一个数挂在它底下）

## 求最大子矩阵的大小



子问题：求直方图的最大面积

E:\BaiduNetdiskDownload\学习\java\进阶\算法进阶班资料\advanced_class_06\Code_04_MaximalRectangle.java

必须以每一行作为底

## 环形山峰可见问题

2*i-3