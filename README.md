# GoogleKickstart
Solution to Google Kickstart Rounds

## 2020
### Round A
1. **Allocation**
> There are **_N_** houses for sale. The **_i-th_** house costs **_Ai_** dollars to buy. You have a budget of **_B_** dollars to spend.
What is the **_maximum number of houses_** you can buy?

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define ar array

int n, b, a[100000];

void solve() {
	cin >> n >> b;
	for(int i=0; i<n; ++i)
		cin >> a[i];
	sort(a, a+n);
	int ans=0;
	for(int i=0; i<n; ++i) {
		if(b>=a[i]) {
			b-=a[i];
			++ans;
		}
	}
	cout << ans << "\n";
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);

	int t, i=1;
	cin >> t;
	while(t--) {
		cout << "Case #" << i << ": ";
		solve();
		++i;
	}
}
```
***

2. **Plates**
> Dr. Patel has N stacks of plates. Each stack contains K plates. Each plate has a positive beauty value, describing how beautiful it looks.
Dr. Patel would like to take exactly P plates to use for dinner tonight. If he would like to take a plate in a stack, he must also take all of the plates above it in that stack as well.
Help Dr. Patel pick the P plates that would maximize the total sum of beauty values.

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define ar array

int n, k, p, a[50][30];
int dp[51][1501];

void solve() {
	cin >> n >> k >> p;
	memset(dp, 0xc0, sizeof(dp));
	dp[0][0]=0;
	for(int i=0; i<n; ++i) {
		memcpy(dp[i+1], dp[i], sizeof(dp[0]));
		for(int j=0, s=0; j<k; ++j) {
			cin >> a[i][j];
			s+=a[i][j];
			//use j+1 plates
			for(int l=0; l+j+1<=p; ++l)
				dp[i+1][l+j+1]=max(dp[i][l]+s, dp[i+1][l+j+1]);
		}
	}
	cout << dp[n][p] << "\n";
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);

	int t, i=1;
	cin >> t;
	while(t--) {
		cout << "Case #" << i << ": ";
		solve();
		++i;
	}
}
```
***

3. **Workout**
> Tambourine has prepared a fitness program so that she can become more fit! The program is made of N sessions. During the i-th session, Tambourine will exercise for Mi minutes. The number of minutes she exercises in each session are strictly increasing.
The difficulty of her fitness program is equal to the maximum difference in the number of minutes between any two consecutive training sessions.
To make her program less difficult, Tambourine has decided to add up to K additional training sessions to her fitness program. She can add these sessions anywhere in her fitness program, and exercise any positive integer number of minutes in each of them. After the additional training session are added, the number of minutes she exercises in each session must still be strictly increasing. What is the minimum difficulty possible?

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define ar array

int n, k, a[100000];

void solve() {
	cin >> n >> k;
	for(int i=0; i<n; ++i)
		cin >> a[i];

	int lb=1, rb=a[n-1]-a[0];
	while(lb<rb) {
		int mb=(lb+rb)/2;
		int k2=0;
		for(int i=1; i<n; ++i) {
			int d=a[i]-a[i-1];
			//ceil(d/(n+1))<=mb
			//d<=mb*(n+1)
			//d/mb-1<=n
			k2+=(d+mb-1)/mb-1;
		}
		if(k2<=k)
			rb=mb;
		else
			lb=mb+1;
	}
	cout << lb << "\n";
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);

	int t, i=1;
	cin >> t;
	while(t--) {
		cout << "Case #" << i << ": ";
		solve();
		++i;
	}
}
```
***

4. **Bundling**
> Pip has N strings. Each string consists only of letters from A to Z. Pip would like to bundle their strings into groups of size K. Each string must belong to exactly one group.
The score of a group is equal to the length of the longest prefix shared by all the strings in that group. For example:
The group {RAINBOW, RANK, RANDOM, RANK} has a score of 2 (the longest prefix is 'RA').
The group {FIRE, FIREBALL, FIREFIGHTER} has a score of 4 (the longest prefix is 'FIRE').
The group {ALLOCATION, PLATE, WORKOUT, BUNDLING} has a score of 0 (the longest prefix is '').
Please help Pip bundle their strings into groups of size K, such that the sum of scores of the groups is maximized.

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define ar array

int n, k, c[2000001][26], m, cnt[2000001];
ll ans;

void dfs(int u=0, int d=0) {
	for(int v=0; v<26; ++v)
		if(c[u][v])
			dfs(c[u][v], d+1), cnt[u]+=cnt[c[u][v]];
	while(cnt[u]>=k) {
		cnt[u]-=k;
		ans+=d;
	}
}

void solve() {
	cin >> n >> k;
	m=1;
	for(int i=0; i<n; ++i) {
		string s;
		cin >> s;
		int u=0;
		for(char d : s) {
			if(!c[u][d-'A'])
				c[u][d-'A']=m++;
			u=c[u][d-'A'];
		}
		++cnt[u];
	}
	ans=0;
	dfs();
	cout << ans << "\n";
	memset(c, 0, m*sizeof(c[0]));
	memset(cnt, 0, m*4);
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);

	int t, i=1;
	cin >> t;
	while(t--) {
		cout << "Case #" << i << ": ";
		solve();
		++i;
	}
}
```
