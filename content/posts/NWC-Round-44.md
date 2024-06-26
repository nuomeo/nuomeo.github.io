+++
title = 'NWC Round 44'
date = 2024-05-28T20:05:00+08:00
draft = false

+++

比赛链接：[牛客周赛 Round 44](https://ac.nowcoder.com/acm/contest/82526)

### [A 题 唐龙守则](https://ac.nowcoder.com/acm/contest/82526/A)

很简单的签到题目，根据题目要求直接输出 `n / 3` 的下取整结果就好。

```C++
#include <iostream>

using namespace std;

int main() {
  int n; cin >> n;
  cout << n / 3 << endl;
  return 0;
}
```



### [B 题 最大公约](https://ac.nowcoder.com/acm/contest/82526/B)

根据题要求，我们需要找出一个 **最长的** 所有元素的 **最大值** 和 **最大公约数** 都相等的子序列。

满足这个性质的的子序列，其中元素的值一定都相同，那么这道题目就是找出出现次数最多的元素。

```C++
#include <iostream>
#include <unordered_map>

using namespace std;

const int N = 100010;
int a[N];

int main() {
  int n; cin >> n;
  unordered_map<int, int> mp;
  
  for(int i = 0; i < n; i++) {
    cin >> a[i];
    mp[a[i]]++;
  }
  
	int mx = 0;
  for(auto [k, v] : mp) {
    mx = max(mx, v);
  }
  
  cout << mx << endl;
  return 0;
}
```



### [C 题 连锁进位](https://ac.nowcoder.com/acm/contest/82526/C)

本题需要我们统计将除首位数字以外的每一位都加到零所需要的次数，直接按位模拟即可。

```C++
#include <iostream>

using namespace std;

int main() {
    int t; cin >> t;
    while(t--) {
        string s; cin >> s;
        int ans = 0, c = 0;
        for(int i = s.size() - 1; i > 0; i--) {
            if((s[i] - '0') + c % 10 == 0) continue;
            
            if(c == 1) s[i] += 1;
            
            if(s[i] > '0' && s[i] <= '9') {
                if(c == 0) c = 1;
                ans += 10 - (s[i] - '0');
            }
        }
        cout << ans << endl;
    }
    
    return 0;
}
```



### [D 题 因子区间](https://ac.nowcoder.com/acm/contest/82526/D)

这道题需要计算出数组每个元素的因子数量，然后将其放入一个二维数组 `sum[i][j]` 中，`i` 标识当前坐标，`j` 表示因子个数，之后然后去求前缀和即可。

```C++
#include <iostream>

using namespace std;

const int N = 100010;
int a[N], sum[N][130];

int main() {
    ios_base::sync_with_stdio(0), cin.tie(0);
    int n, q; cin >> n >> q;
    for(int i = 1; i <= n; i++) {
        cin >> a[i];
        int cnt = 0;
        for(int j = 1; j <= a[i] / j; j++) {
            if(a[i] % j == 0)
            {
                cnt++;
                if(a[i] / j != j)
                    cnt++;
            }
        }
        sum[i][cnt] = 1;
    }
    
    for(int i = 2; i <= n; i++)
        for(int j = 1; j < 130; j++)
            sum[i][j] += sum[i - 1][j];
    while(q--) {
        int l, r; cin >> l >> r;
        
        long long ans = 0;
        for(int i = 1; i < 130; i++) {
            long long cnt = sum[r][i] - sum[l - 1][i];
            if(cnt != 0) ans += cnt * (cnt - 1) / 2;
        }
        
        cout << ans << endl;
    }
    
    return 0;
}
```



### [E 题 小苯的排列构造](https://ac.nowcoder.com/acm/contest/82526/E)

先列出 1 - 10 可以怎样构造，就可以发现规律，至少有8个数字才可以构造出排列，每个数与最小的可以使用的数相差四，因此直接构造即可。

```C++
#include <iostream>

using namespace std;

int main() {
  int n; cin >> n;
  if (n < 8) cout << -1 << endl;
  else {
    for (int i = 5; i <= n; i++) cout << i << ' ';
    cout << (n % 2 ? "2 1 4 3" : "1 2 3 4") << '\n';
  }

  return 0;
}
```



### [F 题 小红的基环树删边](https://ac.nowcoder.com/acm/contest/82526/F)

看到的简洁题解。[题解 | #小红的基环树删边#_牛客博客 (nowcoder.net)](https://blog.nowcoder.net/n/66152910d903429dac8b6d878b263d9e)

```C++
#include <iostream>
#include <vector>

using namespace std;

const int N = 100010;

int n, vis[N], f[N];
vector<pair<int,int> > v[N];

void dfs(int x,int dep){
  if(x == n) {
    for(int i = 1; i <= n; i++) {
      if(!vis[i]) f[i] = min(f[i], dep);
    }
    return ;
  }
  for(auto [y,id] : v[x])
    if(!vis[id]){
    	vis[id] = 1;
      dfs(y, dep + 1);
      vis[id] = 0;
  }
}

int main() {
  ios::sync_with_stdio(0); cin.tie(0);
  
  cin >> n;
  for(int i = 1; i <= n; i++) {
    int a, b; cin >> a >> b;
    f[i] = 1e9;
    v[a].push_back({b, i});
    v[b].push_back({a, i});
  }
  
  dfs(1, 0);
  
  for(int i = 1; i <= n; i++) cout << (f[i] > 1e6 ? -1 : f[i]) << endl;
  
  return 0;
}
```

