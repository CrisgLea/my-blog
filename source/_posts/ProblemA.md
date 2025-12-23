---
title: Problem A
tags:
  - ACM
date: 2025-12-23 19:01
categories: 武汉新生赛
---
# 题面

Z 有一个仅由大写英文字母组成的字符串 ss。

Z 想知道：在五所主办大学的英文缩写中，哪一个在字符串中作为 **子序列** 出现的次数最多？你需要找出出现次数最多的缩写；如果存在多个这样的缩写，**请你找到字典序最小的缩写**。

五所主办大学及其缩写如下：

- Huazhong University of Science and Technology —— `HUST`
- Wuhan University —— `WHU`
- Wuhan University of Technology —— `WHUT`
- Huazhong Agricultural University —— `HZAU`
- Central China Normal University —— `CCNU`

### 注意

- 字符串 TT 在字符串 SS 中“以子序列的形式出现”意味着我们可以从 SS 中删除若干个（也可能是零个）字符来得到 TT。
- 字典序比较规则：从左到右比较字符，第一个不同的位置字符较小的字符串字典序更小；若一个字符串是另一个的前缀，则较短者字典序更小。

---

### 输入格式

输入一行一个字符串 ss，其中 ss 仅包含大写英文字母，且长度不超过 100100。

### 输出格式

输出一行，包含一个字符串和一个整数，两者之间用空格分隔，分别表示出现次数最多的大学缩写及其出现次数。

如果多个缩写的最大出现次数相同，则输出字典序最小的缩写。

---

#### 样例
输入数据

`NUAHUNHCSNWUWAAAUHSU`

输出数据
 
`WHU 2`
# 思路
拿到题目之后 发现题目给了如果计数相同时候的处理办法 即 **输出字典序最小的缩写** 那很轻易的就会想对各个学校的简称进行排序 即`CCNU HUST HZAU WHU WHUT`
比赛的时候只想着要满足前一个字符串存在的条件忽视了字符串中的字符顺序一定要严格才能拼出目标 导致我的写法会使whuhhhhhh多出后面的h数量种情况
事实上运用二维数组dp只有在顺序正确时才会影响dp结果（因为后续新增字符的排列情况受前一个字符数量影响）
|**dp[i][j]**|**空 (0)**|**W (1)**|**H (2)**|**U (3)**|**逻辑说明**|
|---|---|---|---|---|---    |
|**初始状态**|**1**|0|0|0|基础：空串匹配空串为1|
|**w (1)**|1|**1**|0|0|匹配W：0(上)+1(左上)=1|
|**h (2)**|1|1|**1**|0|匹配H：0(上)+1(左上)=1|
|**u (3)**|1|1|1|**1**|匹配U：0(上)+1(左上)=**1**|
|**h (4)**|1|1|**2**|1|匹配H：1(上)+1(左上)=**2** (发现新WH)|
|**h (5)**|1|1|**3**|1|匹配H：2(上)+1(左上)=**3** (发现新WH)|
```cpp
#include <bits/stdc++.h>
#define int long long 
using namespace std;
long long count(const string& s, const string& target) {
    int n = s.length();
    int m = target.length();
    vector<vector<long long>> dp(n + 1, vector<long long>(m + 1, 0));
    for (int i = 0; i <= n; i++) {
        dp[i][0] = 1;
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = dp[i - 1][j];
            if (s[i - 1] == target[j - 1]) {
                dp[i][j] += dp[i - 1][j - 1];
            }
        }
    }
    return dp[n][m];
}
signed main() {
    string s , best_uni ;
  	cin >> s ;
    vector<string> unis = {"CCNU", "HUST", "HZAU", "WHU", "WHUT"};
    string scl = "";
    int mx = -1;
    for (const string& uni : unis) {
        int now = count(s, uni);
        if (now > mx ) {
            mx = now;
            best_uni = uni;
        }
    }
    cout << best_uni << " " << mx << endl;
}