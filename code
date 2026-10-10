#include <bits/stdc++.h>
using namespace std;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, m;
    if(!(cin>>n>>m)) return 0;
    vector<vector<int>> adj(n+1);
    vector<pair<int,int>> edges;
    edges.reserve(m);
    for(int i=0;i<m;++i){
        int x,y;cin>>x>>y;
        adj[x].push_back(y);
        adj[y].push_back(x);
        edges.emplace_back(x,y);
    }
    // Tarjan for biconnected components
    vector<int> disc(n+1,0), low(n+1,0);
    int timer=0;
    vector<pair<int,int>> st; // edge stack
    vector<char> good(n+1,0);
    function<void(int,int)> dfs = [&](int u, int parent){
        disc[u]=low[u]=++timer;
        for(int v: adj[u]){
            if(v==parent) continue;
            if(!disc[v]){
                st.emplace_back(u,v);
                dfs(v,u);
                low[u]=min(low[u], low[v]);
                if(low[v]>=disc[u]){
                    // pop component
                    vector<pair<int,int>> compEdges;
                    unordered_set<int> verts;
                    while(!st.empty()){
                        auto e = st.back(); st.pop_back();
                        compEdges.push_back(e);
                        verts.insert(e.first);
                        verts.insert(e.second);
                        if(e.first==u && e.second==v) break;
                    }
                    if(verts.size()<3) continue; // cannot have cycle
                    // build adjacency for component
                    unordered_map<int, vector<int>> compAdj;
                    for(auto &e: compEdges){
                        compAdj[e.first].push_back(e.second);
                        compAdj[e.second].push_back(e.first);
                    }
                    // bipartite check
                    unordered_map<int,int> color;
                    bool isBip=true;
                    for(int start: verts){
                        if(color.find(start)!=color.end()) continue;
                        queue<int> q; q.push(start); color[start]=0;
                        while(!q.empty() && isBip){
                            int cur=q.front(); q.pop();
                            for(int nb: compAdj[cur]){
                                if(!color.count(nb)){
                                    color[nb]=color[cur]^1;
                                    q.push(nb);
                                }else if(color[nb]==color[cur]){
                                    isBip=false; break;
                                }
                            }
                        }
                        if(!isBip) break;
                    }
                    if(!isBip){
                        for(int vtx: verts) good[vtx]=1;
                    }
                }
            }else if(disc[v] < disc[u]){ // back edge
                st.emplace_back(u,v);
                low[u]=min(low[u], disc[v]);
            }
        }
    };
    for(int i=1;i<=n;++i){
        if(!disc[i]) dfs(i,-1);
    }
    long long goodCount=0;
    for(int i=1;i<=n;++i) if(good[i]) ++goodCount;
    cout << (n - goodCount) << "\n";
    return 0;
}
