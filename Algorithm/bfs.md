# bfs
图的bfs遍历。bfs采用queue来保证先到先访问。

    #include <iostream>
    #include <vector>
    #include <queue>
    
    using namespace std;
    
    void bfs_matrix(vector<vector<bool> > &matrix) {
        int n = matrix.size();
        queue<int> s;
        vector<bool> visited(n, false);
    
        s.push(0);
        visited[0] = true;
        while (s.size() > 0) {
            int cur = s.front();
            s.pop();
    
            cout << "visiting " << cur << endl;
            for (int i = 0; i < n; ++i) {
                if (matrix[cur][i] && visited[i] == false) {
                    s.push(i);
                    visited[i] = true;
                }
            }
        }
    }
    
    void bfs_link(vector<vector<int> > &edges) {
        int n = edges.size();
        queue<int> s;
        vector<bool> visited(n, false);
    
        s.push(0);
        visited[0] = true;
        while (s.size() > 0) {
            int cur = s.front();
            s.pop();
    
            cout << "visiting " << cur << endl;
    
            for (int i = 0;i < edges[cur].size(); ++i) {
                if (visited[edges[cur][i]] == false) {
                    s.push(edges[cur][i]);
                    visited[edges[cur][i]] = true;
                }
            }
        }
    }
    
    int main() {
        int n;
        cin >> n;
        vector<vector<bool> > matrix(n, vector<bool>(n));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                int input;
                cin >> input;
                if (input == 1) {
                    matrix[i][j] = true;
                } else {
                    matrix[i][j] = false;
                }
            }
        }
        bfs_matrix(matrix);
    
        cin >> n;
        vector<vector<int> > edges(n);
        for (int i = 0; i < n; ++i) {
            int num;
            cin >> num;
            for (int j = 0; j < num; ++j) {
                int input;
                cin >> input;
                edges[i].push_back(input);
            }
        }
        bfs_link(edges);
    
        return 0;
    }

