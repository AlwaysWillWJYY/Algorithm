### lc668

### 思路：二分

```java
class Solution {
    int n, m, k;
    public int findKthNumber(int _m, int _n, int _k) {
        m = _m; n = _n; k = _k;
        int l = 0, r = m * n - 1;
        while (l <= r) {
            int mid = l + r >> 1, cnt = getCnt(mid);
            if (cnt >= k) r = mid - 1;
            else l = mid + 1;
        }
        return l;
    }
    int getCnt(int mid) {
        int a = 0, b = 0;
        for (int i = 1; i <= m; i++) {//m行n列
            if (i * n < mid) {
                a += n;//a代表小于mid的个数
            } else {
                if (mid % i == 0 && ++b >= 0) a += mid / i - 1;//b代表等于mid的个数
                else a += mid / i;
            }
        }
        return a + b;
    }
}
```


