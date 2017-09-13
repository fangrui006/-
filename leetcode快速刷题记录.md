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
