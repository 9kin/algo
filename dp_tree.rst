Дп на поддеревьях
"""""""""""""""""

:math:`O(V + E)` Задача о независимом множестве
""""""""""""""""""""""""""""""""""""""""""""""""

#. * dp[u][0] - максимальное множество без вершины :math:`u`

   * dp[u][1] - максимальное множество с вершиной :math:`u`

#. :math:`v` - дети вершины :math:`u`

   * dp[u][0] = sum(max(dp[v][0], dp[v][1]))

   * dp[u][1] = sum(dp[v][0]) + 1

#. * обычный dfs 
   
   * пересчёт dp[u]

Невзвешанное дерево (вес 1)
===========================


.. image:: https://i.imgur.com/7eBP9Pj.png



.. code-block:: cpp

	#include <bits/stdc++.h>
 
	using namespace std;
	 
	typedef long long ll;
	 
	vector<bool> used;
	vector<pair<ll, ll>> dp;
	vector<vector<ll>> v;
	 
	void dfs(ll u) {
	    used[u] = true;
	    for (auto q : v[u]) {
	        dfs(q);
	    }
	    ll sum1 = 0, sum0 = 0;
	    for (auto q: v[u]) {
	        dp[u].second += dp[q].first;
	        dp[u].first += max(dp[q].first, dp[q].second);
	    }
	    dp[u].second++;
	}
	 
	int main() {
	    ios::sync_with_stdio(0);
	    cin.tie(0);
	    ll n, x, root;
	    cin >> n;
	    used.resize(n);
	    dp.resize(n);
	    v.resize(n);
	    for (int i = 0; i < n; i++) {
	        cin >> x;
	        if (x == 0) {
	            root = i;
	        } else {
	            x--;
	            v[x].push_back(i);
	        }
	    }
	    dfs(root);
	    cout << max(dp[root].first, dp[root].second);
	    return 0;
	}


Взвешанное дерево 
==================
Добавим в случае dp[u].second += w[u];

.. image:: https://i.imgur.com/aiwhCMB.png

.. code-block:: cpp

	#include <bits/stdc++.h>
	 
	using namespace std;
	 
	typedef long long ll;
	 
	vector<bool> used;
	vector<pair<ll, ll>> dp;
	vector<vector<ll>> v;
	vector<ll> w;
	 
	void dfs(ll u) {
	    used[u] = true;
	    for (auto q : v[u]) {
	        dfs(q);
	    }
	    ll sum1 = 0, sum0 = 0;
	    for (auto q: v[u]) {
	        dp[u].second += dp[q].first;
	        dp[u].first += max(dp[q].first, dp[q].second);
	    }
	    dp[u].second += w[u];
	}
	 
	int main() {
	    ios::sync_with_stdio(0);
	    cin.tie(0);
	    ll n, x, root, y;
	    cin >> n;
	    used.resize(n);
	    dp.resize(n);
	    v.resize(n);
	    w.resize(n);
	    for (int i = 0; i < n; i++) {
	        cin >> x >> y;
	        w[i] = y;
	        if (x == 0) {
	            root = i;
	 
	        } else {
	            x--;
	            v[x].push_back(i);
	        }
	    }
	    dfs(root);
	    cout << max(dp[root].first, dp[root].second);
	    return 0;
	}