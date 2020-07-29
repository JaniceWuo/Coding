## 题目
现给定任意正整数n，请寻找并输出最小的正整数m(m>9)，使得 m 的各位（个位、十位、百位 ... ...）之乘积等于n，若不存在则输出 -1。

## 思考
如果n本身就是10以内的数，比如9，则m为19，所以10以内就直接输出10 + n。
又因为要找最小的数，所以能被n整除的越小的数应该越往位数高的位置排。所以先计算出个位的数字，也就是最大的能被n整除的数，如果n=36，则m的个位为9，然后一步步缩小n（这里n变为了4)以及找更小的能被其整除的数。

## code
```java
public class Main {
    public static void main(String[] args) {
        System.out.println(solution( 15 ));
    }
    public static int solution(int n){
        if(n < 10) return 10 + n;
        int res = 0,base = 1;
        for(int i = 9; i > 1; i--){   //从9开始作为除数
            while ( n % i == 0){
                res += i * base;
                base *= 10;
                n /= i;
            }
        }
        if(n > 1) return -1;
        return res;
    }
}

```