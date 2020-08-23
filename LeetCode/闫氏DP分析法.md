# 闫氏DP分析法
视频讲解链接：[闫氏DP分析法](https://www.bilibili.com/video/BV1X741127ZM)
## 石子合并
设有N堆石子排成一排，其编号为1，2，3，…，N。

每堆石子有一定的质量，可以用一个整数来描述，现在要将这N堆石子合并成为一堆。

每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。

例如有4堆石子分别为 1 3 5 2， 我们可以先合并1、2堆，代价为4，得到4 5 2， 又合并 1，2堆，代价为9，得到9 2 ，再合并得到11，总代价为4+9+11=24；

如果第二步是先合并2，3堆，则代价为7，得到4 7，最后一次合并代价为11，总代价为4+7+11=22。

问题是：找出一种合理的方法，使总的代价最小，输出最小代价。

输入格式
第一行一个数N表示石子的堆数N。

第二行N个数，表示每堆石子的质量(均不超过1000)。

输出格式
输出一个整数，表示最小代价。

数据范围
1≤N≤300

输入样例：
```
4
1 3 5 2
```
输出样例：
```
22
```

C++代码实现
```cpp
/*
DP算法
状态表示：
f[i][j] 从i到j的所有组合的最小消耗
s[i] 前缀和
状态转移：
for (int k=i;k<j;k++)
    f[i][j] = min(f[i][j], f[i][k]+f[k+1][j],s[j]-s[i])
*/
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 310;

int f[N][N];
int s[N];
int n;

int main(){
    cin >> n;
    for(int i=1; i<=n; i++) cin >> s[i], s[i] += s[i-1];
    
    for(int len=2; len<=n; len++){
        for(int i=1; i<=(n-len+1); i++){
            int j = i+len-1;
            f[i][j] = 1e8;
            for (int k=i; k<j; k++)  // 从i开始的意思是，第一个元素可以自己算做一堆
                f[i][j] = min(f[i][j],f[i][k]+f[k+1][j]+s[j]-s[i-1]);
        }
    }
    cout << f[1][n] << endl;
    return 0;
}
```

## 最长公共子序列
给定两个长度分别为N和M的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少。

输入格式
第一行包含两个整数N和M。

第二行包含一个长度为N的字符串，表示字符串A。

第三行包含一个长度为M的字符串，表示字符串B。

字符串均由小写字母构成。

输出格式
输出一个整数，表示最大长度。

数据范围
1≤N,M≤1000

输入样例：
```
4 5
acbd
abedc
```
输出样例：
```
3
```
C++代码实现：
```cpp
/*
求最大值的情况的DP算法可以出现重复
状态表示f[i][j] a[1~i]和b[1~j]之间的最大公共子序列长度
状态转移，根据边界情况，一共存在四种情况：00,01,10,11,表示0和1分别表示是否包含a[i]和b[j]
1) 00: a[i]和b[j]都不包括：f[i-1][j-1]
2) 01: 只包含a[i]: f[i][j-1]虽然这种表示并不完全等价“末尾包含a[i]”，但是他涵盖了“末尾包含a[i]”子集，所以在max问题中可以使用
3) 10：只包含b[j]: f[i-1][j]
4) 11: a[i]==b[j]: f[i-1][j-1]+1
以上四种情况取max
*/

#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int f[N][N];
int n,m;
char a[N], b[N];

int main(){
    cin >> n >> m >> a+1 >> b+1;  //a,b字符串的长度, 这输入方式有点帅哦
    // for (int i=1; i<=n; i++) cin >> a[i];
    // for (int i=1; i<=m; i++) cin >> b[i];
    for (int i=1; i<=n; i++)
        for (int j=1; j<=m; j++){
            if (a[i]==b[j]) f[i][j] = max(f[i][j], f[i-1][j-1]+1);  // 11的情况
            else f[i][j] = max(f[i][j], f[i-1][j-1]); //00
            f[i][j] = max(f[i][j], f[i-1][j]); //01
            f[i][j] = max(f[i][j], f[i][j-1]); //10
        }
    cout << f[n][m] << endl;
    return 0;
}
```
简化版：
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

int f[N][N];
int n,m;
char a[N], b[N];

int main(){
    cin >> n >> m >> a+1 >> b+1;  //a,b字符串的长度, 这输入方式有点帅哦
    for (int i=1; i<=n; i++)
        for (int j=1; j<=m; j++){
            f[i][j] = max(f[i][j-1], f[i-1][j]); //01
            if (a[i]==b[j]) f[i][j] = max(f[i][j], f[i-1][j-1]+1);  // 11的情况
        }
    cout << f[n][m] << endl;
    return 0;
}
```