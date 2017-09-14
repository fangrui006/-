8. String to Integer (atoi)
```
I think we only need to handle four cases:

discards all leading whitespaces
sign of the number
overflow
invalid input
Is there any better solution? Thanks for pointing out!

int atoi(const char *str) {
    int sign = 1, base = 0, i = 0;
    while (str[i] == ' ') { i++; }
    if (str[i] == '-' || str[i] == '+') {
        sign = 1 - 2 * (str[i++] == '-'); 
    }
    while (str[i] >= '0' && str[i] <= '9') {
        if (base >  INT_MAX / 10 || (base == INT_MAX / 10 && str[i] - '0' > 7)) {
            if (sign == 1) return INT_MAX;
            else return INT_MIN;
        }
        base  = 10 * base + (str[i++] - '0');
    }
    return base * sign;
}

```

33. Search in Rotated Sorted Array
先跟targe比较，然后根据nums[left]和nums[mid]的大小确定哪边是有序的。通过判断target是不是在有序的区间内，决定left和right下一轮的取值。
