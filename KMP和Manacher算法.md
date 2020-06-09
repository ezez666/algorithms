## KMP算法

子序列：可以连续也可以不连续

子数组，子串：一定连续

int getIndexof(str1,str2)    用的就是KMP    O(N) c++、java的实际时间复杂度要更好

1、一个字符的信息：前面字符的最长前缀与最长后缀的匹配长度

2、0位置-1,1位置0，......

过程

实质：否定

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

求一个字符的最长回文子串的长度

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