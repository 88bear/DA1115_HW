# DA1115_HW
solution and some thoughts

----------------------
### 題目給的範例可以發現，越左邊的運算子會越早被處理，所以要用一個 stack 來把所有的運算子儲存下來處理

**遇到運算元的話就直接輸出**

1. 遇到優先級最高的刮號
   - 右刮號的話把 stack 輸出直到上一個左刮號的運算子
   - 左刮號直接放進 stack 裡
1. 遇到優先及第二高的乘除
   - 如果現在 stack 最上面的運算子優先級 大於等於 乘除，把 stack 最上面的輸出，因為他們應該要在現在處理的運算子前先被處理，再把目前的運算子放入 stack
   - 反之，直接把現在的運算子放入 stack
1. 遇到優先度最低的加減
   - 與乘除一樣
-------------------------------

```cpp

#include <bits/stdc++.h>

using namespace std;

signed main() {
    int n;
    string s, c;
    cin >> n;
    while(n--){
        while(getline(cin,s)){
            stack <string> st;
            vector <string> ans;
            stringstream ss; ss << s; 
            while(ss >> c) {
                if(c == ")") {
                    while(st.top() != "(") {
                        ans.push_back(st.top());
                        st.pop();
                    }
                    st.pop();
                }else if(c == "(") {
                    st.push("(");
                }else if(c == "*" || c == "/") {
                    while(!st.empty() && (st.top() == "*" || st.top() == "/")) {
                        ans.push_back(st.top());
                        st.pop();
                    } 
                    st.push(c);
                }else if(c == "+" || c == "-") {
                    while(!st.empty() && (st.top() == "*" || st.top() == "/" || st.top() == "+" || st.top() == "-")) {
                        ans.push_back(st.top());
                        st.pop();
                    } 
                    st.push(c);
                }else{
                    ans.push_back(c);
                }
            }
            
            while(!st.empty()){
                ans.push_back(st.top());
                st.pop();
            }
            for(int i = 0; i < ans.size(); ++i){
                cout << ans[i] << " \n"[i == ans.size()-1]; //這是個有趣的寫法，有興趣可以去了解一下
            }
        }
    }
}

```
-----------------------------

## 第二題就沒這題要寫得那麼多了

** 沒有括號所以很簡單 **
### 利用迴圈找到連續的 _運算元 運算元 運算子_ 再push進去stack

```cpp
#include <bits/stdc++.h>

using namespace std;

signed main(){
    string str;
    stack <int> st;
    while(cin >> str){
        if(str == "+" || str == "-" || str == "*" || str == "/" || str == "%"){
            int sec = st.top(); st.pop();
            int ft = st.top(); st.pop();
            if(str == "+") st.push(ft + sec);
            else if(str == "-") st.push(ft - sec);
            else if(str == "*") st.push(ft * sec);
            else if(str == "/") st.push(ft / sec);
            else st.push(ft%sec);
        }else{
            st.push(stoi(str));
        }
    }
    cout << st.top() << '\n';
}
```
