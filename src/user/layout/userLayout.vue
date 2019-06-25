<template>
<body >
        <div id="low_right">
             <a @mousedown="click()" style="width:70px;float: left;"  @mouseleave="click2()"   >
                    <ul style="margin: 0;
                    font-weight: bold;
                    font-size: 170%;
                    padding-left: 20px;">
                    <li>__</li>
                    <li>__</li>
                    <li>__</li>
                </ul>
               </a>
               <div style="width:80%;float: right;"></div>
        </div>
         <!-- 头部 -->
        <div id="vice-network-bar">
            
            <!-- <div class="logo">
                    bolg
            </div> -->
            <!-- <div @mousemove="display='block'" @mouseout="display='none'" class="family dropdown">
                <span class="trigger">关于我</span>
                <ul  :style="'display:'+display">
                    <li >
                            <a href="https://github.com/ediconss">
                                gitHub
                            </a>
                    </li>
        
                </ul>
            </div> -->
        </div >
        <!-- LOGO -->
        <div id="site-header">
            <a @mousedown="click()" style="width:70px;float: left;"  @mouseleave="click2()"   >
                    <ul style="margin: 0;
                    font-weight: bold;
                    font-size: 170%;
                    padding-left: 20px;">
                    <li>__</li>
                    <li>__</li>
                    <li>__</li>
                </ul>
               </a>
               <div style="width:80%;float: right;"></div>
        </div>

        <div  id="main" class="container">
        <router-view/>
        </div>

        <!-- 侧边菜单-->
        <div :style="'display:'+dlgdisplay" :class="'side-backdrop'+dlg"  @mousedown="click3()" @mouseup="click4()"></div> 
         <div :style="'overflow: hidden;display:'+dlgdisplay" id="side-nav" :class="'side-layer'+dlg" >
            <div class="side-hd">
               <a  @mouseup="click3()" @mouseleave="click4()" > <font  size="10px" style="padding-top:1px;float: left">×</font></a>
              </div>
        <div style="height:100%">
            <div style="padding-bottom:100%;background: #F1F1F1;margin-right: 1px;">
                <div class="search-bar">
                           <button style="font-size:200%"  @click="search()" class="el-icon-search"></button>
                    <input type="text" v-model="keyword" @keydown="show($event)" placeholder="搜索">    
                </div>
                <ul class="menu">
                        <li class="nav" style="padding-bottom:0">
                           <a id="atest"  href="/" >首页</a>
                        </li>
                    </ul>
            </div>
            <div style="padding-top: 10px;">
                    
                    <a class="icon" href="https://github.com/ediconss"> 
                      <img  style="width: 60px;
                                    margin-left:10px;
                                    "  src="@/assets/github_1226494_easyicon.net.svg" />
                        <h4 style="margin: 0;
    width: 80px;
    
    padding-left: 12px;">GitHub</h4>    
                    </a>
               
            
            </div>
        </div>
         </div>
   </body>
</template>

<script>

export default {
     data() {
    return {
        d:"M 8 0 C 3.58 0 0 3.58 0 8 c 0 3.54 2.29 6.53 5.47 7.59 c 0.4 0.07 0.55 -0.17 0.55 -0.38 c 0 -0.19 -0.01 -0.82 -0.01 -1.49 c -2.01 0.37 -2.53 -0.49 -2.69 -0.94 c -0.09 -0.23 -0.48 -0.94 -0.82 -1.13 c -0.28 -0.15 -0.68 -0.52 -0.01 -0.53 c 0.63 -0.01 1.08 0.58 1.23 0.82 c 0.72 1.21 1.87 0.87 2.33 0.66 c 0.07 -0.52 0.28 -0.87 0.51 -1.07 c -1.78 -0.2 -3.64 -0.89 -3.64 -3.95 c 0 -0.87 0.31 -1.59 0.82 -2.15 c -0.08 -0.2 -0.36 -1.02 0.08 -2.12 c 0 0 0.67 -0.21 2.2 0.82 c 0.64 -0.18 1.32 -0.27 2 -0.27 c 0.68 0 1.36 0.09 2 0.27 c 1.53 -1.04 2.2 -0.82 2.2 -0.82 c 0.44 1.1 0.16 1.92 0.08 2.12 c 0.51 0.56 0.82 1.27 0.82 2.15 c 0 3.07 -1.87 3.75 -3.65 3.95 c 0.29 0.25 0.54 0.73 0.54 1.48 c 0 1.07 -0.01 1.93 -0.01 2.2 c 0 0.21 0.15 0.46 0.55 0.38 A 8.013 8.013 0 0 0 16 8 c 0 -4.42 -3.58 -8 -8 -8 Z",
        keyword:'',
        dlg:'',
        dlgdisplay:'none',
        display:'none',
        }
  },
  mounted(){
        // alert('au')
  },
 created(){
    //  alert('bu')
     this.keyword = this.$route.query.keyword;
 },
  methods:{
       show:function(ev){
                if(ev.keyCode == 13){
                  this.$router.push({path:'/search?keyword='+this.keyword}) 
                  this.search()
                }
        },
      search(){
           this.click3()
            this.click3()
          if(this.keyword==''){ 
             return
          }
          window.location.href='/search?keyword='+this.keyword
        //  this.$router.push({path:})    
        this.keyword=''     
      },
      click(){
          this.dlgdisplay='block';
          
      },
      click2(){
          this.dlg=' in'
      },
    click3(){
         this.dlg=''
      },
      
      click4(){
          if(this.dlg==''){
        this.dlgdisplay='none';
          }
         
      }
  }
}
</script>
<style>
@media (max-width: 500px) {
    .container {
      background: #F1F1F1;
      margin-right: 20px;
    }
    #main {
        margin-left:  20px;
        margin-bottom: 30px;
     }
    }
/* 4个 */
@media (min-width: 1000px) {
.container {
    background: #F1F1F1;
    margin-right: 60px;
}
#main {
    margin-left:  60px;
    margin-bottom: 30px;
    }
}
/* 2个 */
@media (max-width: 1000px) and (min-width: 500px) {
.container {
    background: #F1F1F1;
    margin-right: 30px;
}
#main {
    margin-left:  30px;
    margin-bottom: 30px;
    }
}

a.icon:hover, :visited {
    color: #fff;
}
#atest {
   text-decoration:none; 
color:#333;
}
li {list-style-type:none;line-height:10px;color: #393939;}
#low_right 
{ 
    position: fixed;
    width: 100%;
    height: 52px;
    background: #f1f1f1;
    background-color: #f1f1f1;
    z-index: 1;
   
} 
.side-hd {
    height: 60px;
    padding: 0 15px;
        margin-right: 1px;
    text-align: center;
    background: #F1F1F1;
}
.side-layer .scrollable-content {
    overflow: auto;
    -webkit-overflow-scrolling: touch;
}
*, *:before, *:after {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
}
.side-bd .wrapbg {
    background: #F1F1F1;
    overflow: hidden;
    
}
.side-layer .slimScrollBar {
    
    display: none !important;
    opacity: 0 !important;
    -webkit-transition: opacity .3s;
    -o-transition: opacity .3s;
    transition: opacity .3s;
}
#side-nav .search-bar {
    top: 10px;
    position: relative;
    height: 50px;
    padding-right: 20px;
    margin: 0 15px;
    border: 1px solid #d8d8d8;
}
#side-nav .search-bar input {
    width: 80%;
    padding-left: 10px;
    padding-right: 10px;
    font-size: 20px;
    border: none;
    background: none;
    outline: none;
}
#side-nav .search-bar button {
    padding-top:8px;
    float: right;
    padding-bottom:20px;
    padding-left: 10px;
    padding-right: 20px;
    border: none;
    width: 10px;
    background: none;
    outline: none;
}
input {
    line-height: normal;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    border-radius: 0;
}
input {
    padding: 1px 0px;
}
 .menu {
    top: 10px;
    font-weight: bold;
    list-style: none;
    margin: 0 15px;
    border: 0%;
    padding: 0;
    left: 10px;
}
#side-nav .menu .nav {
    font-size: 25px;
    line-height: 1;
    padding: 25px 0;
    font-weight: bold;
}

form {
    display: block;
    margin-top: 0em;
}
#vice-network-bar {
    padding-top: 1px;
    position: relative;
    height: 50px;
    background: #393939;
    color: #fff;
    font-size: 12px;
    z-index: 120;
}
body {
    margin: 0;/* 外边界距离 */
    background: #F1F1F1;
    font-family: "Helvetica Neue", Arial, "PingFang SC", "Hiragino Sans GB", "Source Han Sans CN", Roboto, "Microsoft YaHei", sans-serif;
    line-height: 1.5;
    word-wrap: break-word;
}
div {
    display: block;
}
.side-backdrop.in {
    opacity: 0.55;
}
.side-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: #0e0e0e;
    opacity: 0;
    z-index: 200;
    display: none;
    -webkit-transition: 1s ease-out;
    -o-transition: 1s ease-out;
    transition: 1s ease-out;
}
#vice-network-bar .logo{
    float:left;
   line-height: 40px;
    padding-left: 40%
}
#vice-network-bar .butten{
    line-height: 40px;
    float: left;
}
#vice-network-bar .family {
    float: right;
    border-left: 1px solid #0e0e0e;
}
#vice-network-bar .dropdown {
    position: relative;
    height: 100%;
}
#vice-network-bar .dropdown .trigger {
    display: inline-block;
    padding: 0 2em 0 .7em;
    color: #fff;
    border: none;
    font-weight: bold;
    line-height: 40px;
    opacity: 0.8;
}
#vice-network-bar .dropdown .trigger:after {
    border: 0.5em solid transparent;
    border-top-color: #fff;
    content: " ";
    height: 0;
    bottom: auto;
    left: auto;
    position: absolute;
    right: 0;
    -webkit-transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    top: 50%;
    width: 0;
}
#vice-network-bar .family ul {
    width: 100%;
}
#vice-network-bar .dropdown ul {
    position: absolute;
    top: 100%;
    left: 0;
    white-space: nowrap;
    background: #0e0e0e;

    list-style: none;
    margin: 0;
    padding: 0;
}
#vice-network-bar .dropdown li a {
    display: block;
    color: #fff;
    font-weight: bold;
    opacity: 0.7;
    line-height: 1.25em;
    padding: 0.5em 0.7em;
    border: none;
}
#vice-network-bar .dropdown .shou ul{
    display:block;
}
#site-header {
    clear: both;
    position: relative;
    height: 40px;
    background: #f1f1f1;
        z-index: 3;
}
#site-header .site-header-container {
    margin: 0 30px;
}
#site-header .logo {
    position: absolute;
    top: 15px;
    left: 50%;
    width: 92px;
    height: 30px;
    margin-left: -46px;
    background: url(/public/assets/images/vice/vice-logo-black.svg) no-repeat 50%;
    -webkit-background-size: cntain cntain;
    background-size: cntain;
}
#site-header .nav-toggle {
    float: left;
    font-size: 16px;
    margin-left: -9px;
}
[class^="icon-"], [class*=" icon-"] {
    font-family: 'icomoon' !important;
    speak: none;
    font-style: normal;
    font-weight: normal;
    font-variant: normal;
    font-variant-ligatures: normal;
    font-variant-caps: normal;
    font-variant-numeric: normal;
    font-variant-east-asian: normal;
    text-transform: none;
    line-height: 1;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
ul, menu, dir {
    display: block;
    list-style-type: disc;
    margin-block-start: 1em;
    margin-block-end: 1em;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    padding-inline-start: 40px;
}
.icon-menu:before {
    content: "\e901";
    font-size: 17px;
}
*, *:before, *:after {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
}
a {
    color: inherit;
    text-decoration: none;
}
#side-nav {
    left: 0;
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
}
.side-layer.in {
    -webkit-transform: none !important;
    -ms-transform: none !important;
    transform: none !important;
}
.side-layer {
    position: fixed;
    top: 0;
    right: 0;
    height: 100%;
    width: 250px;
    background: #000;
    z-index: 250;
    display: none;
    -webkit-box-shadow: 0 0 50px #000;
    box-shadow: 0 0 50px #000;
    -webkit-transition: .5s ease-out;
    -o-transition: .5s ease-out;
    transition: .5s ease-out;
}
</style>

