<template>
   <div style="margin-top:30px">
    
            <div class="article-single-image">
                <img  :src="article.cover"  />
            </div>

            <div class="article-single-content">
                <header class="article-header">
                    <div class="article-header-inner">
                        <div class="article-topics">
                            <a :href="'/topic?name='+topic" v-for="topic in topicLiat" :key="topic">{{topic}}&nbsp&nbsp</a>
                        </div>
                        <h3 class="article-title">{{article.title}}</h3>
                    </div>
                </header>
                <div class="article-wrapper">
                    <div class="article-meta">
                        <div class="author">
                        <div class="details">
                                <div class="byline">
                                    <a>{{article.founder}}</a>
                                </div>
                                <span class="published">{{article.create_time}}</span>
                        </div>
                        </div>
                    </div>

                    <div >
                        <div class="article-summary">
                          {{article.summary}}  
                        </div>
                        <div class="ql-container ql-snow" style="border:none">
                             <div class="ql-editor" style="padding:0"  v-html="article.content">
                             <div>{{article.content}}</div>
                             </div>
                           
                            
                        </div>
                    </div>
               
            </div>
       </div>
    </div>
</template>
<script>
import articleApi from "@/api/article";
import { quillEditor, Quill } from "vue-quill-editor"; //调用编辑器
export default {
    data(){
        return{
            article:{},
            topicLiat:[]
        }
    },
    created(){
        var id=this.$route.params.year+this.$route.params.month+this.$route.params.day+this.$route.params.item
         articleApi.findById(id).then(res => {
             if(res.flag){
                this.article=res.data
            this.topicLiat = res.data.topic.split(",");
            console.log( res.data.topic)
        
             }
          })    
    }
}
</script>

<style>

.article-summary {
    font-size: 24px;
    line-height: 1.6;
    color: #aaa;
    margin-bottom: 1em;
}
.article-meta {
  
    padding: 15px 0 0;
    margin-bottom: 35px;
    overflow: hidden;
    border-top: 1px solid #ebebeb;
    border-bottom: 1px solid #ebebeb;
}
.article-meta .author {
    margin-bottom: 15px;
}
.article-meta .details {
    overflow: hidden;
}
.article-meta .byline {
    padding-top: 5px;
}
.article-meta .published {
    color: #aaa;
    display: block;
}

.article-title {
    font-size: 48px;
    line-height: 1.2;
}
h1, h2, h3, h4, h5, h6 {
    margin: 0;
    font-weight: 900;
}
.article-topics {
    position: absolute;
    top: -30px;
    left: 0;
    font-weight: bold;
    background: #0e0e0e;
    padding: .5em 1.5em;
    font-size: 12px;
    color: #fff;
}
@media (min-width: 786px){
.article-wrapper {  
    position: relative;
    width: 640px;
    margin-left: auto;
    margin-right: auto;
    clear: both;
}
.article-header {
    width: 50%;
    margin-top: -125px;
    margin-left: auto;
}
.article-header-inner {
    position: relative;
    border-top: 5px solid #0e0e0e;
    background: #fff;
    margin-left: -350px;
    padding: 30px;
}
}
@media (max-width: 786px){
    .article-wrapper {
    width: auto;
    margin-left: 15px;
    margin-right: 15px;
}
.article-header {
     margin-top: 0;
    width: auto;
}
.article-header-inner {
    position: relative;
    border-top: 5px solid #0e0e0e;
    background: #fff;
    padding: 30px;
}
}

.article-single-image img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 99%;
}
.article-single-image {
    position: relative;
    padding-bottom: 56.25%;
    overflow: hidden;
}
.article-single-content {
    /* position: relative; */
    background: #fff;
}



</style>
