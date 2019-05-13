<template>
  <div id="index">
    <div class="content">
      <div class="title">
        <h1>Live Info</h1>
        <mt-button type="primary" class="new iconfont icon-refresh" @click="refresh"></mt-button>
      </div>
      <div class="type">
        <mt-button type="default" class="all" @click="toAll">All</mt-button>
        <mt-button type="primary" @click="toOnline">Online</mt-button>
        <mt-button type="danger" @click="toOffline">Offline</mt-button>
      </div>
      <div class="search">
        <input placeholder="Filter by name" v-model="name">
        <mt-button type="primary" icon="search" @click="search"></mt-button>
      </div>

      <div class="info">
        <div class="info-all" v-show="allShow">
          <div class="detial" v-for="(item,index) in allList" :key="index">
            <img :src="item.avatar_small" >
            <span>{{item.nickname}}</span>
            <span>{{item.online}}</span>
          </div>

        </div>
        <div class="info-online" v-show="onlineShow">
          <div class="detial" v-for="(item,index) in onlineList" :key="index">
            <img :src="item.avatar_small" >
            <span>{{item.nickname}}</span>
            <span>{{item.online}}</span>
          </div>
        </div>
        <div class="info-offline" v-show="offlineShow">
          <div class="detial" v-for="(item,index) in offlineList" :key="index">
            <img :src="item.avatar_small" >
            <span>{{item.nickname}}</span>
            <span>{{item.online}}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Button } from 'mint-ui'
import {MessageBox} from 'mint-ui'

import api from './../axios/api.js'
export default {
  data(){
    return{
      name:'',
      allList:[],
      onlineList:[],
      offlineList:[],
      allListFirst:[],
      onlineListFirst:[],
      offlineListFirst:[],
      // allList:[{
      //   src: './../assets/logo.png',
      //   user: '1',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '11',
      //   state: 'online'
      // },
      // {
      //   src: './../assets/logo.png',
      //   user: '1111',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '112',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '3',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '4',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '5',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '6',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '7',
      //   state: 'online'
      // }],
      // onlineList:[{
      //   src: './../assets/logo.png',
      //   user: '1',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '2',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '3',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '4',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '5',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '6',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '7',
      //   state: 'online'
      // }],
      // offlineList:[{
      //   src: './../assets/logo.png',
      //   user: '1',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '2',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '3',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '4',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '5',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '6',
      //   state: 'online'
      // },{
      //   src: './../assets/logo.png',
      //   user: '7',
      //   state: 'online'
      // }],

      allShow: true,
      onlineShow: false,
      offlineShow:false,
    }
  },
  created() {
    this.getDatas()
  },
  methods: {
    getDatas: function() {
      api.getData("http://capi.douyucdn.cn/api/v1/live?offset=0")
      .then(res=>{
        // console.log(res)
        console.log(res.data)
        this.allList=res.data
        this.allListFirst=res.data//获取数据后做一个缓存，初始数据
        let online=[]
        let offline=[]
        let onlineNum=[]
        for (let index = 0; index < res.data.length; index++) {
          onlineNum.push(res.data[index].online)
          if(res.data[index].online>=1000000){//假设online数超过1000000状态为online
            online.push(res.data[index])
          }else {//假设online数低于1000000状态为offline
            offline.push(res.data[index])
          }
        }
        this.onlineList=online
        this.onlineListFirst=online//获取数据后做一个缓存，初始数据
        this.offlineList=offline
        this.offlineListFirst=offline//获取数据后做一个缓存，初始数据
        console.log(onlineNum)
      })
      .catch(err=>{
        console.log(err)
      })
    },

    refresh: function () {
      this.name=''
      this.$options.methods.getDatas()
      this.offlineShow=false
      this.onlineShow=false
      this.allShow=true

    },
    toAll: function () {
      
      this.offlineShow=false
      this.onlineShow=false
      this.allShow=true
      this.name=''
      this.allList=this.allListFirst
    },
    toOnline: function () {
      this.allShow=false
      this.offlineShow=false
      this.onlineShow=true
      this.name=''
      this.onlineList=this.onlineListFirst
    },
    toOffline: function () {
      this.allShow=false
      this.onlineShow=false
      this.offlineShow=true
      this.name=''
      this.offlineList=this.offlineListFirst
    },
    search: function () {
      let self = this
      console.log(self.name)
      console.log(self.allList)
      let all= self.allShow
      let online= self.onlineShow
      let offline = self.offlineShow
      let state
      if (all) {
        state = 0
        console.log("0all")
      } else if (!all&&online) {
        state = 1
        console.log("1online")
      } else {
        state = 2
        console.log("2offline")
      }
      let allSel=[]
      let onlineSel=[]
      let offlineSel=[]
      if (self.name=='') {
        MessageBox('Notice','请输入搜索内容')
      }else {
        // console.log("allShow"+self.allShow)
        // console.log("onlineShow"+self.onlineShow)
        // console.log("offlineShow"+self.offlineShow)
        switch (state) {
          case 0:
            console.log("all")
            for (let index = 0; index < self.allList.length; index++) {
              if(self.allList[index].nickname.indexOf(self.name)>=0){
                console.log("111allShow")
                allSel.push(self.allList[index])
              }
            }
            if (allSel=='') {
              console.log("未找到")
              MessageBox.confirm('', { 
                message: '未找到',
                title:   'Notice',
                showCancelButton: false,
                confirmButtonText: '确定',
                // cancelButtonText: '取消'
              })
              .then(action => { 
                console.log("确认")
                self.name=''
              })
              .catch(err => { 
                console.log(err)
              })
            } else {
              console.log(allSel)
              self.allList=allSel
              self.name=''
            }
            break;

          case 1:
            console.log("online")
            for (let index = 0; index < self.onlineList.length; index++) {
              if(self.onlineList[index].nickname.indexOf(self.name)>=0){
                console.log("222onlineShow")
                onlineSel.push(self.onlineList[index])
              }
            }
            if (onlineSel=='') {
              console.log("未找到")
              MessageBox.confirm('', { 
                message: '未找到',
                title:   'Notice',
                showCancelButton: false,
                confirmButtonText: '确定',
                // cancelButtonText: '取消'
              })
              .then(action => { 
                console.log("确认")
                self.name=''
              })
              .catch(err => { 
                console.log(err)
              })
            } else {
              console.log(onlineSel)
              self.onlineList=onlineSel
              self.name=''
            }
            break;
          
          case 2:
            console.log("offline")
            for (let index = 0; index < self.offlineList.length; index++) {
              if(self.offlineList[index].nickname.indexOf(self.name)>=0){
                console.log("333offlineShow")
                offlineSel.push(self.offlineList[index])
              }
            }
            if (offlineSel=='') {
              console.log("未找到")
              MessageBox.confirm('', { 
                message: '未找到',
                title:   'Notice',
                showCancelButton: false,
                confirmButtonText: '确定',
                // cancelButtonText: '取消'
              })
              .then(action => { 
                console.log("确认")
                self.name=''
              })
              .catch(err => { 
                console.log(err)
              })
            } else {
              console.log(offlineSel)
              self.offlineList=offlineSel
              self.name=''
            }
            break;

          default:
            break;
        }
      }
    }
  }
}
</script>

<style lang="less">
body {
  background-color: #79c3f2;
  box-sizing: border-box;
  width: 100%;
  height: auto;
  min-height: 100%;
  
}
#index {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;

  .content {
    width: 400px;
    height: auto;
    margin: 0 auto;
    padding: 20px;
    background-color: pink;

    .title {
      width: 100%;
      height: auto;
      display: flex;
      justify-content: center;

      .new {
        margin: 20px 0 0 20px;
      }
    }
    .type {
      width: 100%;
      height: auto;
      display:flex;
      justify-content: center;

      button {
        margin: 0 8px;
      }
      .all {
        background-color: green;
        color: #fff;
      }
    }
    .search {
      width: 100%;
      height: auto;
      margin-top: 10px;

      input {
        height: 30px;
      }
    }

    .info {
      width:100%;
      height: auto;
      margin-top: 20px;
      background-color: yellow;

      .detial {
          display:flex;
          justify-content:space-between;
          margin: 18px;

          img {
            width: 48px;
            height: 48px;
          }
        }
      .choosePage {
        display: flex;
        justify-content: space-between;
      }

      .info-all {
        width: 100%;
        height: auto;
        padding: 10px;
        box-sizing: border-box;
        background-color: green;

        
      }
      .info-online {
        width: 100%;
        height: auto;
        padding: 10px;
        box-sizing: border-box;
        background-color: blue;
      }
      .info-offline {
        width: 100%;
        height: auto;
        padding: 10px;
        box-sizing: border-box;
        background-color: red;
      }
    }
  }
}
</style>


