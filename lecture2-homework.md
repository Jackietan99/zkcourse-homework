# 第2课 课后作业

> 题目链接：https://zkshanghai.xyz/notes/exercise2.html


## 第1题： 转换为bit位Num2Bits

```rust
pragma circom 2.1.4;


template Num2Bits(n) {

    var res = 0;
    var lv = 1;
    
    signal input in;
    signal output out[n];
   
    for (var i =0 ;i< n ;i++) {
         //向右移动 i 位，然后按位与运算 最右边一位（即第 i 位），最后将结果存储到数组 out 的第 i 个位置上
        out[i] <-- (in >> i) & 1;
        //约束
        out[i]*(out[i] - 1) === 0; 

        res += out[i]*lv;

        //计算下一个更高位的权重。
        lv = lv + lv;
    }
    //结果约束
    
    res === in;
}

template Main () {
    signal input in;
    signal output o;

    component n2b = Num2Bits(4);
    n2b.in <== in;

    o <== n2b.out[0];
}

component main = Main();

/* INPUT = {
    "in": "11"
} */

```

## 第2题： 判零 IsZero


```rust

template IsZero() {
    signal input in;
    signal output out;

    signal inv;

    inv <-- in!=0 ? 1/in : 0;

    out <== -in*inv +1;
    in*out === 0;
}

```



## 第3题： 相等 IsEqual

``` rust 

template IsEqual() {
    signal input in[2];
    signal output out;

    component isz = IsZero();

    in[1] - in[0] ==> isz.in;

    isz.out ==> out;
}
```


## 第4题： 少于 LessThan



## 第5题： 选择器 Selector


