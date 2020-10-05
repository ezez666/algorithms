1. int到long  注意范围 

2. =的情况

3. O(N)，找n，例如以a[i]为结尾，（0<= i <n）

4. PriorityQueue<E> 在java.util.PrioriryQueue包里，大小表示用.size(),弹出是.poll()

5. debug时可在控制台正常输入，来调试

6. .peek等操作时要确定是否为空不然会产生Exception in thread "main"的bug

7. 递归到动态规划异地要明确dp的横纵坐标和dp值的含义，遍历即可·

8. sout
```jav
StringBuilder builder = new StringBuilder();
        for (int tmp : arr) {
            builder.append(tmp).append(" ");
        }
        System.out.println(builder.toString());
        
     for() .......
     
     
     Integer.valueOf()   与   Integer.parseInt(s[i]);
```

9. 快速幂取模
10. l+((r-l)>>1) 与 l+(r-l)>>1 不一样

