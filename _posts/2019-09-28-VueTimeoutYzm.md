---
title: vue定时器做验证码倒计时
date: 2019-09-28 22:46:49
---
# vue定时器做验证码倒计时

```
<button class="noyzmshow" @click="nopushData" v-if="noyzmshow">获取验证码({{second}})</button>

 data() {
    return {
      second: 59
    };
  },
   methods: {
   pushData(){
   this.noyzmshow = true;
      this.yzmshow = false;
      var k = 59;
      let timer = setInterval(() => {
        k--;
        this.second = k;
        if (this.second <= 0) {
          this.second = 59;
          clearInterval(timer);
          this.noyzmshow = false;
          this.yzmshow = true;
        }
        //    console.log(this.second)
      }, 1000);
   	}
   }
   
   定时器用()=>{} 箭头函数
```

