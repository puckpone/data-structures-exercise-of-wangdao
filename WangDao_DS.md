# 

# 线性表

**10.【2010统考真题】**设将n（n>1）个整数存放到一维数组R中。设计一个在时间和空间两方面都尽可能高效的算法。将R中保存的序列循环左移p（O<p<n）个位置，即将R中的数据由(Xo,X1,..,Xn-1)变换为(Xp,Xp+1,...,X-1,Xo,X1,…,Xp-1)。

**解答**

1）：将整个数组R逆置，再将前n-p个元素和后p个元素分别逆置

2）：

```C++
void reverse(int *array,int l, int r){   //l,r代表逆置的区间
    while(l<r){
        swap(a[l],a[r])
        l++,r--;
    }
}
void LeftShift(int *R, int p){   //p为左移的位数
    reverse(R,0,n-1);  //将整个数组逆置
    reverse(R,0,n-p-1);  //将前n-p个元素逆置
    reverse(R,n-p,n-1); //将最后p个元素逆置
}
```

3)：时间复杂度为O(n)  空间复杂度为O(1)

**11.【2011统考真题】**一个长度为L（L≥1）的升序序列S，处在第L/2个位置的数称为S的中位数。例如，若序列S=(11,13,15,17,19)，则S的中位数是15，两个序列的中位数是含它们所有元素的升序序列的中位数。例如，若S2=(2,4,6,8,20)，则S和S2的中位数是11。现在有两个等长升序序列A和B，试设计一个在时间和空间两方面都尽可能高效的算法，找出两个序列A和B的中位数。

**解答：**

1）：设置两个指针分别从A、B的第一个位置开始遍历，如果A[i]>=B[j],就让j自加1,如果A[i]<B[j]，就让i自加1

再设置一个计数器cnt，每次判断自加1，加到n/2时就找到了中位数

2):

```C++
int FindMiddle(int *a, int *b){
    int i=0,j=0;
    int cnt=0;  //计数器
    while(i<n&&j<n){
        cnt++;
        if(a[i]>=b[j]) {
            if(cnt==n) return b[j];
            j++;
        }
        else {
            if(cnt==n) return a[i];
            i++;
        }
    }
}
```

3): 时间复杂度O(n),空间复杂度O(1)

**12.【2013统考真题】**已知一个整数序列A=(a0,α1，...,αn-1)，其中 0≤a<n（0≤i<n）。若存在ap1=αp2=...=αpm=x且 m>n/2（0≤pk<n,1≤k≤m），则称x为 A的主元素。例如A=(0,5,5,3,5,7,5,5)，则5为主元素；又如A=(0,5,5,3,5,1,5,7)，则A中没有主元素。假设A中的n个元素保存在一个一维数组中，请设计一个尽可能高效的算法，找出A的主元素。若存在主元素，则输出该元素；否则输出-1。要求：

**解答：**

1）：使用hash的思想，使用一个辅助数组count[n]来存储各个元素出现的次数，如count[2]=5就代表2在序列中出现了5次。遍历一遍数组将所有元素出现的次数求出来，再遍历数组count[],找到最大的那个数max，如果max>n/2,则主元素就是max在count中对应的下标，否则没有主元素

2):

```C++
 const int n=10;
 int count[n]={0};
 int max=0;
 int max_idx;  //count中最大元素的下标
 void primeElem(int *a){
     for(int i=0 ; i<n ; i++)  //计算每个元素出现的次数
         count[a[i]]++;
     for(int i=0 ; i<n ; i++) { //找到出现次数最多的元素
         if(count[i]>max) {
             max=count[i];
             max_idx=i;
         }
     }
     if(max>n/2) return i;
     return -1;
 }
```

3): 时间复杂度O(n)  空间复杂度O(n)

**13.【2018统考真题】**给定一个含n（n≥1）个整数的数组，请设计一个在时间上尽可能高效的算法，找出数组中未出现的最小正整数。例如，数组{-5,3,2,3}中未出现的最小正整数是1；数组{1,2,3}中未出现的最小正整数是4。

1): 先使用快速排序将数组从小到大排序，然后从前往后遍历找到第一个正整数x，若x>=2,则未出现的最小正整数为1.  否则x<1,继续往后遍历，并比较相邻的两个元素, 若a[i+1]-a[i] >=2, 则答案为a[i]+1.  若遍历到了末尾还没找到答案，则答案为a[n-1]+1

2): 

```C++
//快排的思想，随便找一个数作为中间值，将大于这个数的元素放到左边，小于的放到右边 
void quickSort(int *a, int left, int right){
    if(left>=right) return; //递归结束条件(只有一个元素时)
    int i=left,j=right;
    int tmp = a[left];
    while(i<j){
        while(i<j&&a[j]>tmp) j--;   //找到右边第一个小于tmp的数
        if(i<j){
            a[i]=a[j];  //将找到的j位置的元素移动到左边
            i++;
        }
        while(i<j&&a[i]<tmp) i++;   //找到左边第一个大于tmp的数
        if(i<j){
            a[j]=a[i];  //将找到的i位置的元素移动到右边
            j--;
        }
    }
    a[i]=tmp;  //此时i=j，将中间值归位
    quickSort(a,left,i-1);
    quickSort(a,i+1,right);
}

int MinInterger(int *a, int n){
    quickSort(a,0,n-1);
    int idx=0;
    while(a[idx]<=0) idx++; //找到第一个正整数
    if(a[idx]>=2) return 1;
    for(; idx<n-1 ; idx++){
        if(a[idx+1]-a[idx]>=2) return a[idx]+1;
    }
    return a[n-1]+1;
}
```

3): 时间复杂度 O(nlogn)  空间复杂度O(1)

**14.【2020统考真题】**定义三元组（a,b,c)（a,b,c均为整数）的距离D=|a-b|+|b-c|+|c-a|。给定3个非空整数集合S、S2和S3，按升序分别存储在3个数组中。请设计一个尽可能高效的算法，计算并输出所有可能的三元组(a,b,c)（a∈S，beS，c∈S;）中的最小距离。例如S={-1,0,9},S2={-25,-10,10,11}，S={2,9,17,30,41}，则最小距离为2，相应的三元组为(9,10,9)。

1): 

1. 初始化三个指针 i、j 和 k，分别指向集合 S、S2 和 S3 的第一个元素。
2. 计算当前三元组 (S[i], S2[j], S3[k]) 的距离，并更新最小距离。
3. 移动距离最小的元素所在集合的指针，直到其中一个指针到达了末尾。
4. 重复步骤 3 和 4，直到遍历完所有可能的三元组。

2):

```C++
int minDistance(vector<int>& S, vector<int>& S2, vector<int>& S3) {
    int min_distance = INT_MAX;

    int i = 0, j = 0, k = 0;

    while (i < S.size() && j < S2.size() && k < S3.size()) {
        int current_distance = abs(S[i] - S2[j]) + abs(S2[j] - S3[k]) + abs(S3[k] - S[i]);
        min_distance = min(min_distance, current_distance);

        // 移动距离最小的元素所在集合的指针
        int min_value = min({S[i], S2[j], S3[k]});
        if (min_value == S[i]) {
            i++;
        } else if (min_value == S2[j]) {
            j++;
        } else {
            k++;
        }
    }

    return min_distance;
}
```

3）：时间复杂度O(n) 空间复杂度O(1)

**17.【2009统考真题】**已知一个带有表头结点的单链表，结点结构为|data|link|，假设该链表只给出了头指针list。在不改变链表的前提下，请设计一个尽可能高效的算法，查找链表中倒数第k个位置上的结点（k为正整数）。若查找成功，算法输出该结点的data域的值，并返回1；否则，只返回0。要求：

1）描述算法的基本设计思想

2）描述算法的详细实现步骤。

3）根据设计思想和实现步骤，采用程序设计语言描述算法（使用C、C++或Java语言实现），关键之处请给出简要注释。

**解答**

1. 用两个指针，一个从头节点往后移动k个位置，再两个指针同时移动，即两个指针相差k个元素，当前面的指针指向了NULL，此时后面的指针就指向的是倒数第k个位置的节点
2.  
   1. 定义两个指针fast,slow,将头指针list赋值给它们
   2. fast向后移动k次
   3. fast和slow一起向后移动，直到fast为NULL
   4. 此时slow指向倒数第k个位置

```C++
//单链表的结构
typedef struct DNode{
    int data;
    DNode *link;
}DNode, *Linklist;

int FindLastK(Linklist list,int k){
    DNode *fast=list;
    DNode *slow=list;
    while(k--&&fast!=nullptr)
        fast = fast->link;
    if(k!=-1) return 0;  //元素小于k，查找失败
    while(fast!=nullptr){
        fast=fast->link;
        slow=slow->link;
    }
    cout<<slow->data;
    return 1;
}
```

**18.【2012统考真题】**假定采用带头结点的单链表保存单词，当两个单词有相同的后缀时，可共享相同的后缀存储空间，例如，loading和being的存储映像如下图所示。

![img](./imgs/list1.png)

设str1和str2分别指向两个单词所在单链表的头结点，链表结点结构为|data|next|请设计一个时间上尽可能高效的算法，找出由str1和str2所指向两个链表共同后缀的起始位置（如图中字符i所在结点的位置p）。要求：

**解答**

1. 
   1. 用两个指针i，j分别指向str1和str2的头节点
   2. 若str1比str2的长度长k，则i先向后移动k个位置，然后i，j同步移动
   3. 同步移动的过程中，使用标记若i->data==j->data且标记为flag==0,则用指针p记录位置，并将标记位flag置为1.若i->data！=j->data,flag置0（flag为0表示还未开始匹配）
   4. 直到i，j移动到末尾，此时p指向后缀的起始位置

```C++
typedef struct Node{
    int data;
    Node *next;
}Node,*Linklist;
//求链表长度
int Listlen(Linklist list){
    Node *p = list;
    int len=0;
    while(p->next!=nullptr) {
        p=p->next;
        len++;
    }
    return len;
}
Node *findSuffix(Linklist str1, Linklist str2){
    Node *i=str1, *j=str2;
    int str1_len = Listlen(str1);
    int str2_len = Listlen(str2);
    if(str1_len > str2_len){
        int k = str1_len-str2_len;
        while(k--) i = i->next;
    }
    int flag=0;
    Node *p=nullptr;
    while(i!=NULL){
        if(i->data != j->data) flag=0;
        else if(flag==0) p = i;  //i和j指向的data相等，记录p的位置，如果flag==1，说明不是后缀第一个位置
    }
    return p;
}
```

1. 时间复杂度O(m+n)  空间复杂度O(1)

**19.【2015统考真题】**用单链表保存m个整数，结点的结构为[data][link]，且|data|≤n（n为正整数）。现要求设计一个时间复杂度尽可能高效的算法，对于链表中data的绝对值相等的结点，仅保留第一次出现的结点而删除其余绝对值相等的结点。例如，若给定的单链表head如下：

![img](./imgs/list2.png)

1.  
   1. 使用一个辅助数组 bool exist[n]来链表中出现过的元素的绝对值，例如exist[21]=true则表示21在链表中出现过
   2. 遍历一遍链表，存储出现过的元素的绝对值
   3. 再遍历一遍链表，将除第一次出现位置的元素都删除

```C++
typedef struct Node{
    int data;
    Node *link;
}Node, *Linklist;

void delSameAbs(Linklist list）{
    if(list->next==nullptr) return list; //空链表 直接返回
    bool *exist = new bool(n);
    Node *p = list->next;
    while(p->next!=nullptr){
        exist[abs(p->data)] = true;
        p=p->next;
    }
    Node *p = list;
    while(p->next!=nullptr){
        if(exist[abs(p->next->data)])  //遇到第一个，跳过
            exist[abs(p->next->data]=false;
        else{   //后面的删掉
            Node *tmp = p->next;
            p->next = p->next->next;
            delete tmp;
            tmp=nullptr;
        }
        p=p->next;
    }
}
```

1. 时间复杂度O(n) 空间复杂度O(n)

**20.【2019统考真题】**设线性表L=（a1，a2，...,a(n-1)，an）采用带头结点的单链表保存，链表中的结点定义如下：

```C++
typedef struct node
{    int data;
     struct node*next;
}NODE;
```

**请设计一个空间复杂度为O(1)且时间上尽可能高效的算法，重新排列L中的各结点，得到线性表L'=（a1，an，a2,a(n-1), a3,a(n-2), ...)**

todo

```
```





# 栈与队列

**使用两个栈模拟队列**

```C++
struct Que{
    stack<int> s1,s2;
    //入栈前将s1中的元素全部转移到s2中，然后再入栈
    void push(int x){
        while(!s1.empty()){
            s2.push(s1.top());
            s1.pop();
        }
        s1.push(x);
    }
    //出栈前将s2中的元素全部转移到s1中，然后再出栈
    void pop(){
        while(!s2.empty()){
            s1.push(s2.top());
            s2.pop();
        }
        cout<<s1.top()<<endl;
        s1.pop();
    }
};
```



**假设一个算术表达式中包含圆括号，方括号和花括号3种类型的括号，编写一个算法来判别表达式中的括号是否配对，以字符“0”作为算术表达式的结束符。**

答：

**从左到右遍历表达式，遇到左括号考虑入栈，遇到右括号考虑出栈。**

规定：‘（’的优先级最高，‘}’的优先级最低  [{}] 为不合法 (若没有这项规定则不需要判断优先级)

下面来考虑不匹配的情况

- 当前遍历到的为与栈顶不匹配的**右括号**
- 当前遍历到的为**左括号**，但其优先级比栈顶的左括号低  --也就是（{，（[, [{这样的情况*
- 最终栈不为空

栈的数据结构就省略了

```c++
bool cmp(char a, char b){ // a为栈顶，b为当前元素 如果a的优先级高于b，返回true
    switch (a) {
        case '(':
            if(b=='{' || b=='[')
                return false;
            else
                return true;
            break;
        case '[':
            if(b=='{')
                return false;
            else
                return true;
            break;
        default:
            return true;
    }
}

bool brackets_cmp(string &str){
    stack s;
    for(int i=0 ; i<str.size() ; i++){
        char c = s.top(); 
        if(str[i]=='(' || str[i]=='[' || str[i]=='{'){ //左括号考虑入栈
            if(cmp(c,str[i])) //'(' 的优先级最高,如果栈顶元素优先高于当前元素，入栈
                s.push(str[i]);
            else
                return false;
        }
        else{ //右括号考虑出栈
            if(c=='(' && str[i]==')' || c=='[' && str[i]==']' || c=='{' && str[i]=='}')
                s.pop();
            else
                return false;
        }
    }
    if(s.empty()==-1)
        return true;
    else
        return false;
}
```



