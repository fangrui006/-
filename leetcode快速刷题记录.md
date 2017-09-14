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

12. Integer to Roman

罗马数字表示：M D C L X V I，分别表示1000,500,100,50,10,5,1。
给定一个数字1493 = 1000 + 400 + 90 + 3，每一位分别表示就是M CD XC III，合起来就是最终结果。
特别的表示法就是4和9：4用减法表示为IV（5 - 1），9也用减法表示为IX（10 - 1）。
```C++
class Solution {
public:
    string intToRoman(int num) {
        string M[] = {"", "M", "MM", "MMM"};
        string C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        string X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        string I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        return M[num / 1000] + C[(num / 100) % 10] + X[(num % 100) / 10] + I[num % 10];
    }
};
```

13. Roman to Integer
罗马数字中大部分都是加法，少部分是减法。分开考虑即可。
当s[i]表示的base比s[i+1]表示的base小的时候，他们组成的数字做减法。
```C++
class Solution {
public:
    int romanToInt(string s) {
        vector<int> base(256, 0);
        base['M'] = 1000; base['D'] = 500;
        base['C'] = 100; base['L'] = 50;
        base['X'] = 10; base['V'] = 5; base['I'] = 1;
        int n = s.size(), res = 0, i = 0;
        while (i < n) {
            if (i != n - 1 && base[s[i]] < base[s[i + 1]]) {
                res += (base[s[i + 1]] - base[s[i]]);
                i += 2;
            } else {
                res += base[s[i]];
                i++;
            }
        }
        return res;
    }
};
```

14. Longest Common Prefix

按列比较
abcd
abc
abbc

先比较第一列，所有的字符都是a；继续比较第二列，所有的字符都是b；再比较第三列，有不相同的字符。于是最长公共前缀就是ab。
1. 具体操作起来：第一行为基准，所有行都跟第一行的字符作比较。
2. corner case：第一行为空；传入的是空数组；每一行全都相等。

```C++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0 || strs[0].size() == 0) return "";
        for (int j = 0; j < strs[0].size(); j++) {
            char ch = strs[0][j];
            for (int i = 1; i < strs.size(); i++) {
                if (strs[i].size() <= j || strs[i][j] != ch) {
                    return strs[0].substr(0, j);
                }
            }
        }
        return strs[0];
    }
};
```

