# MEDIUM-1：位级操作  
## 模块一：启动引导区 —— 语言的本源  
### 1.请写出十进制 211 对应的二进制表示。  
- 利用将211一直除以2，然后反向取余可得211的二进制表示为11010011.  
### 2.请写出二进制 10110110 对应的十进制表示。 
- 根据二进制到十进制的转换（即有一处对应的2的n次方相加）得出结果为182.  
### 3.在 C/C++ 中，如何快速判断一个十进制整数是奇数还是偶数？  
- 看这个十进制整数与2取余（即n%2）的结果是0为偶数，是1为奇数。  
## 模块二：能量阵列 —— 逻辑门的重构  
### 1.阅读并推导以下 C/C++ 代码的输出结果，并简要解释你的计算结果：  
````
#include <stdio.h>

int main() {
    unsigned char a = 12;  // 二进制: 00001100
    unsigned char b = 25;  // 二进制: 00011001

    unsigned char res1 = a & b;
    unsigned char res2 = a | b;
    unsigned char res3 = a ^ b;
    unsigned char res4 = (a << 2) | (b >> 1);

    printf("%d %d %d %d\n", res1, res2, res3, res4);
    return 0;
}
````  
- **输出结果**： 8 29 21 60  
- **推导过程**：  
1.因为&是按位与，又因为a的二进制表示为00001100，b的为00011001。每一对应位都为1才为1，所以结果为00001000，转换为十进制为8。  
2.因为|是按位或，对应位只要有1就为1，否则为0,所以结果是00011101，转换为十进制为29。  
3.因为^是按位异或，对应位不同为1，相同为0。所以结果为00010101，转换为十进制为21。  
4.因为<<是左移，将00001100左移两位为00110000，>>是右移，将00011001右移1位为00001100，再将这两个数按位或得，00111100，转换为十进制为60。  
- **运行结果**  
<img width="1635" height="663" alt="image" src="https://github.com/user-attachments/assets/56e560d0-b98b-4288-a1d7-f15e37f23e6d" />
 
### 2.给定一个整数x（十进制），同时指定一个位数n，确定这个整数x的二进制表示上第n位是0还是1。例如：251的二进制表示是11111011，n=3，结果返回0。   
- **可执行代码**  
````
#include<stdio.h>
int main(){
    int x,n;
    printf("请输入一个整数，同时指定一个位数\n");
    scanf("%d %d",&x,&n);
    int result=(x>>(n-1))&1;       //运用了位级运算简化了过程
    printf("%d",result);
    return result;
}
````  
这里为了方便展示加入了打印出结果的部分。
- **运行结果**   
<img width="1843" height="1258" alt="image" src="https://github.com/user-attachments/assets/2c287e03-8d51-4058-8d10-e516ac5fb403" />
    
### 3.给定一个整数x（十进制），指定一个位数n，给定一个修正值t（0或者1），将整数x的二进制表示上的第n位改为修正值t。输出被修改后的整数。  
- **可执行代码**   
````  
#include<stdio.h>
int main(){
    int x,n,t,result;
    printf("请输入一个整数，同时指定一个位数，给定一个修正值(0或者1)\n");
    scanf("%d %d %d",&x,&n,&t);
    if (t==1)
    {
       result=x|(1<<(n-1));          //同样的用到了位级运算
    }
    else{
        result=x&~(1<<(n-1));
    }
    printf("%d",result);
    return 0;
}
````  
- **运行结果**  
<img width="2050" height="1342" alt="image" src="https://github.com/user-attachments/assets/a8090bd1-607b-4822-a731-aecc0155bb24" />
  
### 4.给定一个有符号32位整数x，找到其二进制表示上从右开始的第一个1，并输出该数，例如：x=10,二进制表示为1010,第一个1在第二位，所以输出二进制表示为10的数2。  
- **可执行代码**   
````
#include<stdio.h>
int main(){
    int x,result;
    int i=1;
    printf("请输入一个整数\n");
    scanf("%d",&x);
    while (i<32)
    {
       if (((x>>(i-1))&1)==1)   //判断该位是否为1
       {
        printf("%d",i);
        return 0;
       }
        i++;
    }
    printf("该数为0,没有含1位\n");
    return 0;
}
```` 
- **运行结果**  
<img width="2053" height="1236" alt="image" src="https://github.com/user-attachments/assets/fdf5ef95-3028-463a-bd2f-1d9d817ee722" />
  
### 5.现在有两串无符号数a和b的二进制编码，并且有a<b，每个数字的大小在100位以内，现在问区间[a,b]内&的结果。  
- **结果**：其结果为若a，b从左侧第一位开始有连续的对应位相同的数，有多少保留多少，剩下的位全为0。若无相同的则全部位都为0。比如当a=1100，b=1110，则这之间的所有数的按位与为1100。  
- **原因**：因为在数不断增大的时候后面那些不一样的位一定经过了0和1之间的变换，所以因为并不都是1，最后按位与出来的结果就一定是0，只有前面那些不变的位才能保持不变，因为1&1=1，0&0=0。  
### 思考题1 在接触了C语言这么长时间后，相信你已经对逻辑运算了如指掌了，那么你能说说看逻辑运算和位运算的区别和联系吗。思考并提交于markdown文档。  
- **区别**： 
1.作用的对象不同，逻辑运算的作用对象为命题a>b之类的，而位运算的作用对象是数(二进制)的每一位。  
2.运算的结果不同，逻辑运算的结果只有1或0(真和假)，而位运算的结果依旧是一个二进制的数。  
3.位运算还多出了一个位移运算。  
- **联系**：  
1.符号长得像（不知道算不算），如&&与&，||和|。  
2.计算的方式相似，逻辑与是两个都为真才是真即都为1才为1.这与按位与&的对应位都为1才为1很相似。同样的还有逻辑或和按位或，异或和按位异或等。  

### 思考题2 位运算的定义很简单，但是再简单的定义也可能产生一些有用的性质（比如运算律）。查阅资料了解位运算有哪些性质。  
1. &，|，^都具有交换律，结合律，分配律的规律。（与数的运算相似）  
2. &还具有a&a=a,a&0=0,a&全部位为1的数=全部位为1的数等的性质，还可以用来取出特定位的数就像上面的第二题中的`int result=(x>>(n-1))&1;`一样。  
3. |同样有a|a=a,a|0=a,a|全部位为1的数=全部位为1的数等性质，还可用于把特定位置的数变为1，可参考题三中`result=x|(1<<(n-1))`的用法。 
4. ^也有a^a=0,a^0=a,a^b^b=a等性质还可用于不借助第三个变量来进行两个数的交换。  
5. ~a=-(a+1)由补码的公式推出。  
6. a<< n可看成a*2的n次方，同样的a>>n可看成a/2的n次方。   
### 思考题3 移位运算中，右移有两种，其中算数右移对应有符号数，逻辑右移对应无符号数，那么为什么会有这种区别呢。查阅资料理解整数的补码表示后回答于markdown文档。  
- 原因：因为有符号数的最高位是用来表示正负的，如果右移在高位补0，其正负就发生了变化，同时也不符合a>>n可看成a/2的n次方（负数是向负无穷取整）这个结论所以有符号数需要用算数右移，这样也方便有符号数运算的进行，同时也依旧符合负数的补码规则（原码除符号位之外都取反加一）。而逻辑右移只是把其看成含01的一串数处理，与无符号数刚好匹配，其运算也符合a>>n可看成a/2的n次方这个规律。     
## 模块三：通信模块 —— 无碰撞协议  
### 编程题：字符串交集检测  
- **补全的代码如下**  
```` 
#include <stdio.h>

int hasCommonChar(const char *s1, const char *s2) {
    int mask1 = 0;
    int mask2 = 0;
    const char *p1=s1;
    const char *p2=s2;
    while (*p1)
    {
       mask1|=1<<(*p1-'a');   //让其用二进制来表示含有26个字母中的哪些
       (p1++);
       
    }
    while (*p2)           //同理
    {
       mask2|=1<<(*p2-'a');
       (p2++); 
    }
    if ((mask1&mask2)!=0)  //让这两个数按位与，看是否在相同位都有1（即含有相同字母）
    {
      return 1;  
    }
    else{
        return 0;
    }
}

````  
- **运行结果**  
<img width="1738" height="1248" alt="image" src="https://github.com/user-attachments/assets/c2940b67-25ec-40b9-b855-410ac10724cf" />

<img width="1794" height="1351" alt="image" src="https://github.com/user-attachments/assets/6008b404-c368-42bf-97e0-7f2f57d397b7" />

加了main函数并打印了结果。  
## 模块四：反应堆核心 —— 绝对平衡的艺术  
### 【任务 4】给定反应堆的规模N(1≤N≤15)，请使用位运算实现求解控制棒摆放方案的总数  
- **可执行代码**  
```` 
#include <stdio.h>
int success;
int count;
void a(int x,int y1,int y2){
    if (x==success)      //判断是否达成了一个解，用x和success的二进制表达的每一位来表示每一列的占用情况（1为有，0为无）
    {
        count++;
        return;
    }
    int p=success&~(x|y1|y2);   //这里y1，y2分别表示两个对角线方向在改行的限制情况。用按位与来把三种限制情况合并，用~是为了表示该行可以放置位置的情况，而与success按位与是为了消去前面多余的1。
    while (p)
    {
        int t=p&-p;                 //先取了可以放置位置中最低位1所在的位置
        p^=t;                       //把这个情况消去
        a(t|x,(y1|t)<<1,(y2|t)>>1);  //跳到下一行，并把限制情况传下去
    }
    return ;
    
}
int main(){
    int N=15;
    success=(1<<N)-1;             //用success来表示全部占满时的情况
    for (int i = 0; i < (N/2); i++)      //利用镜像对称来减少时间
    {
      int j= 1<<i;
      a(j,j<<1,j>>1); 
    }
    count*=2;
    if (N%2==1)                    //如果N为奇数还要单独算一次开始占中间位置的情况
    {
      int j=1<<(N/2);
      a(j,j<<1,j>>1);
    }
    
    printf("%d皇后的解数有%d",N,count);            //打印出结果
    return 0;
}
````  
- **运行结果** 
<img width="1935" height="1308" alt="image" src="https://github.com/user-attachments/assets/87bc60c7-8c41-4476-ba0c-813764763fef" />

## 模块五：管道穿梭 —— 自动化寻路  
### 【任务 5】编程题：广度优先搜索 (BFS) 与位运算寻路  
- **补全代码**  
```` 
#include <stdio.h>
#include <stdbool.h>

// 定义简单队列结构用于 BFS
typedef struct {
    int pos;   // 当前节点编号 (0~15)
    int dist;  // 到达当前节点的最短步数
} Node;

int minStepsToCheese(int walls) {
    int start = 0;
    int target = 15;

    // 如果起点或终点本身是墙，直接不可达
    if ((walls & (1 << start)) || (walls & (1 << target))) {
        return -1;
    }

    // BFS 队列与访问位图
    Node queue[16];
    int front = 0, rear = 0;
    int visited = 0;

    // 起点入队并标记已访问 (请使用位运算)
    queue[rear++] = (Node){start, 0};
    visited |= (1 << start);

    // 上、下、左、右四个方向的节点偏移量
    int dr[4] = {-1, 1, 0, 0};
    int dc[4] = {0, 0, -1, 1};

    //请在TO DO 和END OF TO DO 行之间补全代码：
    //TO DO
    int j=1;
    
    while (front<rear)
    {
     int pos;            //定义一些变量用来判断是否超出范围
     int r;
     int c;
     
    for (int i = 0; i < 4; i++)       //探索这个节点的四个方向
    {
    pos=queue[front].pos+4*dr[i]+dc[i];    
    r=queue[front].pos/4;
    c=queue[front].pos%4;
    if (pos<16&&pos>=0&&!(walls&(1<<pos)) && !(visited&(1<<pos))&&r>=0&&r+dr[i]<4&&r+dr[i]>=0&&c+dc[i]>=0&&c+dc[i]<4)   //判断是否符合要求即这个节点无墙，是否在范围内，移动是否符合管道系统的网格形状
    {          
     queue[rear]=(Node){queue[front].pos+4*dr[i]+dc[i],j};  //入队
     if (queue[rear].pos==target)                           //判断是否到终点
     {
        return queue[rear].dist;
     }
      visited |=(1<<(queue[front].pos+4*dr[i]+dc[i]));        //更新访问
     rear++;                
    }
        
    }
    front++;                     
    j=queue[front].dist+1;                
    }    
    
    //END OF TO DO

    return -1; // 无法到达
}

int main() {
    int walls = (1 << 5) | (1 << 10); // 5号和10号格子是墙
    int steps = minStepsToCheese(walls);
    printf("Minimum steps: %d\n", steps); // 应输出 6
    return 0;
}
````  
- **运行结果**  
<img width="1935" height="1308" alt="image" src="https://github.com/user-attachments/assets/2a13da7a-14b6-4239-b4b7-e7dc16ef9c55" />

---  
PS：感觉这一题的主要难点在于对位级操作的熟练运用，要清晰的知道这六种基本位运算符都可以用来进行怎样的操作。第一次接触真的不知道怎么做，只好是去查资料或者问AI，特别是模块四和模块五。  
&emsp;模块四是先去上网搜了N皇后问题但我看的他们都没有用位运算来判断，我只好自己想，在经过AI的提示下，终于理解了要用一个数的二进制的每一位来表示每一行可以占用位置的情况，同时那个利用镜像对称来缩减时间的方法也是给AI检查时AI提出的建议，我才改进的不然完全想不到。  
&emsp;模块五是我上网搜了BFS算法是什么，再通过推测原有代码已有定义的变量的作用才写出来的，有借助AI检查。感觉模块五中的借助visited这个数的二进制表示访问过的节点很新奇，因为我网上看到的BFS算法是用两个数组，一个用于记录访问过的节点，另一个用于按访问顺序存储节点作用与题中给的queue数组相似。



