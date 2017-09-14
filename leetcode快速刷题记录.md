3. 我们的目的是求出maxLen，只要每次加入一个元素后保证start的位置是正确的即可。并不需要删除窗口中无用的元素。

4. hard

5. 最长回文串
```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<int> dp(n, 1);
        int maxLen = 1, start = 0;
        for (int i = n - 2; i >= 0; i--) {
            for (int j = n - 1; j >= i + 1; j--) {
                dp[j] = dp[j - 1] && s[i] == s[j];
                if (dp[j] && (j - i + 1) > maxLen) {
                    maxLen = j - i + 1;
                    start = i;
                }
            }
        }
        return s.substr(start, maxLen);
    }
};
```
1. 注意i和j的变化顺序，j必须是递减的，因为dp[j]依赖dp[j-1]，如果按照递增顺序计算，那么计算dp[j]时，dp[j-1]表示的已经是L(i, j-1)，而不是L(i+1, j-1)


7. reverse Integer
```C++
class Solution {
public:
    int reverse(int x) {
        long result = 0;
        while (x != 0) {
            result = result * 10 + (x % 10);
            if (result > (long)INT_MAX || result < (long)INT_MIN)
                return 0;
            x = x / 10;
        }
        return result;
    }
};
```
用long避免计算过程中的溢出。

```C++
class Solution {
public:
    int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int tmp = result * 10 + (x % 10);
            if ((tmp - (x % 10)) / 10 != result)
                return 0;
            result = tmp;
            x = x / 10;
        }
        return result;
    }
};
```
如果计算过后溢出，逆运算得不到原来的值。
-123 / 10 = -12 ... -3

8. atoi   case太多

9. Palindrome Number
判断一个整数是不是回文数。直接reverse可能会导致溢出。所以reverse一半的数，判断左半边和右半边是不是相等。
找一半的位置：x < reversedNum说明已经到达（偶数）或者刚超过（奇数）一半了。最后reversedNum可能和x位数相等，或者多一位（也可能多两位）。
corner case：
（1）4321->false：用上面的算法，最后x = 4, reversedNum = 123。虽然分割点并不在中间的位置，不过这种情况只会发生在非回文数上，而且return条件一定是false（因为reversedNum比x多两位）。
（2）10000->false：最后x = 0, reversedNum = 1。结果返回为true，得到了错误答案。实际上，以0结尾的数都不是回文数，因为最高位不可能是0。所以这种情况可以在开头提前判断。
（3）所有的负数都不是回文数。
```C++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0 || x != 0 && x % 10 == 0) return false;
        int rev = 0;
        while (x > rev) {
            rev = rev * 10 + (x % 10);
            x = x / 10;
        }
        return x == rev || x == rev / 10;
    }
};
```
