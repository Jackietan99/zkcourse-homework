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


## 第4题： 选择器 Selector

``` rust

template Sum(n) {
    signal input in[n];
    signal output out;

    signal sums[n];

    sums[0] <== in[0];

    for (var i = 1; i < n; i++) {
        sums[i] <== sums[i-1] + in[i];
    }

    out <== sums[n-1];
}

template Selector(choices) {
    signal input in[choices];
    signal input index;
    signal output out;
    
    component lessThan = LessThan(4);
    lessThan.in[0] <== index;
    lessThan.in[1] <== choices;
    lessThan.out === 1;

    component sum = Sum(choices);
    component eqs[choices];

    for (var i = 0; i < choices; i ++) {
        eqs[i] = IsEqual();
        eqs[i].in[0] <== i;
        eqs[i].in[1] <== index;
        sum.in[i] <== eqs[i].out * in[i];
    }
    out <== sum.out;
}
```



## 第5题：判负 IsNegative



## 第6题：少于 LessThan  

``` rust
template LessThan (n) {
    signal input in[2];
    signal output out;

    assert(n < 252);

    component n2b = Num2Bit(n+1);
    n2b.in <== in[0] + (1<<n) - in[1];

    out <== 1 - n2b.b[n];
}
```


## 第7题： 整数除法 IntegerDivide


## 第8题:排序 Sort

