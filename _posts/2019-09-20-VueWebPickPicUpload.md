---
title: webPack+Vue图片上传
date: 2019-09-20 23:40:49
---
# 图片上传



## 1. 设置config

```
methods:{
       getAlldata:function(){
           console.log(localStorage)
       },
       afterRead(file) {
      // 此时可以自行将文件上传至服务器
        let params = new FormData();
        params.append("file",file.file)
        console.log(params.get('file'));
        console.log(file.file);
        let config = {
            headers:{
                "Content-Type":"multipart/form-data"
            }
        }
        axios.post('http://jizhang-api-dev.it266.com/api/upload/image?token='+localStorage.token,params,config)
        .then((res)=>{
            console.log(res)
        })
       }
   },
  components: {},
  mounted(){
      this.getAlldata()
  }
}
```

## 2. 完整

```
<template>
   <div class="Subsequent">
       <div class="Subsequent-header">
           <div class="Subsequent-header-title">
               <span>
                   后续缴费
               </span>
           </div>
       </div>
       <div class="Subsequent-body">
           <van-cell-group>
                <van-field
                    v-model="message"
                    label="付款金额"
                    type="textarea"
                    placeholder="请输入付款金额"
                    rows="1"
                    autosize
                />
            </van-cell-group>
            <div>
                <div class="Subsequent-body-select-title">
                    <span>付款账户</span>
                </div>
                <div class="Subsequent-body-select">            
                    <van-dropdown-menu>
                        <van-dropdown-item v-model="value1" :options="option1" title="" />
                    </van-dropdown-menu>
                </div>                            
            </div>
            <div class="Suseequent-body-voucher-div">
                <div class="Suseequent-body-voucher">
                    <span>支出凭证</span>
                </div>
                <div class="Subsequent-body-voycher-pic">
                    <van-uploader v-model="fileList" multiple :after-read="afterRead"  />
                </div>
            </div>
       </div>
   </div>
</template>

<script type="text/ecmascript-6">
import qs from 'qs';
const axios = require('axios');
import { Dialog } from 'vant';
import { Toast } from 'vant';
import moment from "moment"
export default {
   name: '',
   data() {
       return {
            message:'',
            value1: 0,
            value2: 'a',
            option1: [
                { text: '全部商品', value: 0 },
                { text: '新款商品', value: 1 },
                { text: '活动商品', value: 2 }
            ],
            fileList: [
                { url: 'https://img.yzcdn.cn/vant/cat.jpeg' },
                // Uploader 根据文件后缀来判断是否为图片文件
            ]
       }
   },
   methods:{
       getAlldata:function(){
           console.log(localStorage)
       },
       afterRead(file) {
      // 此时可以自行将文件上传至服务器
        let params = new FormData();
        params.append("file",file.file)
        console.log(params.get('file'));
        console.log(file.file);
        let config = {
            headers:{
                "Content-Type":"multipart/form-data"
            }
        }
        axios.post('http://jizhang-api-dev.it266.com/api/upload/image?token='+localStorage.token,params,config)
        .then((res)=>{
            console.log(res)
        })
       }
   },
  components: {},
  mounted(){
      this.getAlldata()
  }
}
</script>

<style lang="less">
.Subsequent-header{
    display:flex;
    justify-content: center;
    // align-items: center;
    padding-top: 20px;
    font-size: 18px;
    color:white;
    width:100%;
    height:30px;
    background-color: orange;
}
.Subsequent-header-title>span{
    font-size: 22px;

}
.Subsequent-header-title{
    margin-top: -10px;
}
.Subsequent-body-select{
    width:70%;
    margin-left: 30%;
}
.Subsequent-body-select-title{
    width:30%;
    background-color: #fff;
    position:absolute;
    height:49px;
    display: flex;
    align-items: center;
    justify-content: center;
}
.Suseequent-body-voucher-div{
    width: 100%;
    height:100px;
    background-color: #fff;
    display: flex;
    align-items: center;
    padding-top: 7px;
    justify-content: space-between;

}
.Suseequent-body-voucher-div>div{
    float: left;
}
.Suseequent-body-voucher{
    width:30%;
    display: flex;
    // height:100px;
    justify-content: center;
    align-items: center;
    margin-left: 3%;
}
.Subsequent-body-voycher-pic{
    height: 100%;
    margin-left: 5%;
}
.Subsequent-body-voycher-pic>.van-uploader>.van-uploader__wrapper{
    margin-left:5%;
    width: 275px;
}
</style>

```

