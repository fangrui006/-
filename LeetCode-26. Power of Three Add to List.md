Given an integer, write a function to determine if it is a power of three.

Follow up:
Could you do it without using any loop / recursion?

Credits:
Special thanks to @dietpepsi for adding this problem and creating all test cases.

Subscribe to see which companies asked this question.

判断是否是三的幂
不能用循环


如果能用循环的话：
 bool isPowerOfThree(int n) {
        if(n==0)
           return false;
        while(n!=1)
        {
        int tmp = n % 3;
          if(tmp !=0)
            return false;
        n = n / 3;
           
        }
        return true;
    }
	
如果是求2或者4的幂：	
还有一些位运算的做法：
	https://leetcode.com/problems/power-of-three/#/description
	
	1、判断是不是2的幂

将2的幂写成二进制很容易看出，2的幂的二进制只有一个1，其余全是0，如下所示：
000010000...00
而将2的幂的二进制减1，其二进制变为：
000001111...11
所以判断一个数是不是2的幂的方法为使用按位与操作，如果结果为0，则是2的幂：
n & (n-1)   
ps(记得n-1要加括号啊，靠)

2、判断是不是4的幂

4的幂首先是2的幂，因为4^n = (2^2)^n，所以4的幂的二进制同样只有一个1，与2的幂不同的是，4的幂的二进制的1在偶数位上，所以判断一个数是不是4的幂的方式为：
1）首先判断是不是2的幂，使用 n & (n-1)
2）进一步判断与0x55555555的按位与结果，0x55555555是用十六进制表示的数，其奇数位上全是1，偶数位上全是0，判断 n & 0x55555555

3、判断是不是3的幂

此方法较为通用，我们首先分析3的幂的特点，假设一个数Num是3的幂，那么所有Num的约数都是3的幂，
如果一个数n小于Num且是3的幂，那么这个数n一定是Num的约数。
了解上述性质，我们只需要找到一个最大的3的幂，
看看参数n是不是此最大的幂的约数就行了，假设参数是整型，那么3的最大的幂的求法为：
[java] view plain copy 在CODE上查看代码片派生到我的代码片
int maxPower = (int) Math.pow(3,(int)(Math.log(0x7fffffff)/Math.log(3)));  

0x7fffffff是整型最大值，也就是Integer.maxValue()。表达式后面两个对数相处结果为double，要转化为整型。
下一步只要判断n是不是maxPower的约数即可：
maxPower % n == 0 

我自己写一下
 bool isPowerOfThree(int n) {
       if(n<=0)   //必须要判断0和负数
           return false;
        
        int max = (int)pow(3,int(log(0x7fffffff)/log(3)));
        cout << max<<endl;
        if(max % n ==0)
            return true;
         else
            return false;
    }
	
	
网上的参考   思路是一样的，只是直接得到3的最大x次方是1162261467
3，任何一个3的x次方一定能被int型里最大的3的x次方整除，如下所示：
[html] view plain copy print?
return n>0?!(1162261467 % n):0; 



还能用log的方法 ，看输入n是3的多少次方，如果求出来的是不是正整数，那么n就不是3的幂。
  bool isPowerOfThree(int n) {
       if(n<=0)
           return false;
        
        double res = log10(n) / log10(3);  //有精度问题，不要用以指数2.718为低的log函数  
        return (res - int(res) == 0) ? true : false;  
    }
	
	我靠，这个方法，简直无敌，实测好像判断哪个数的幂都行

#include<stdio.h>
#include<math.h>
#include<iostream>
using namespace std;
int main()
{    

	int n,x;
	cin>>n>>x;
	//n = (pow(7,10));
	
	x = 7;
	cout << n << " "<<x << endl;
	if (n <= 0)
		return false;

	double res = log10(n) / log10(x);  //有精度问题，不要用以指数2.718为低的log函数  
	if (res - int(res) == 0)
		cout << true << endl;
	else
		cout << false << endl;

	return 0;

}
