<template>
  <div class="app-container">
    <el-form ref="form" label-width="120px">
      <el-form-item label="标题">
        <el-input v-model="pojo.title"/>
      </el-form-item>
      <el-form-item label="概要">
        <el-input v-model="pojo.summary"/>
      </el-form-item>
      <el-form-item label="封面">
        <div>
          <img id="img" @mousedown="clickDiv" v-if="imageUrl" :src="imageUrl" class="avatar">
          <i
            @mousedown="clickDiv"
            style="box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)"
            v-else
            class="el-icon-plus avatar-uploader-icon"
          ></i>
          <input
            id="uploadImg"
            type="file"
            style="display:none"
            accept="image/png, image/jpeg, image/gif"
            @change="uploadImage"
          >
        </div>
      </el-form-item>
      <el-form-item label="概要">
        <quill-editor v-model="content" ref="quillEditor" :options="editorOption"></quill-editor>
      </el-form-item>
      <el-form-item label="标签">
        <el-select v-model="selectLabelList" multiple placeholder="请选择">
          <el-option
            v-for="item in labelList"
            :key="item.id"
            :label="item.labelName"
            :value="item.labelName"
          ></el-option>
        </el-select>
        <el-button icon="el-icon-edit" @click="dialogLabelFormVisible = true" circle></el-button>
      </el-form-item>
      <el-form-item label="主题">
        <el-select v-model="selectTopicList" multiple placeholder="请选择">
          <el-option
            v-for="item in topicList"
            :key="item.id"
            :label="item.topic_name"
            :value="item.topic_name"
          ></el-option>
        </el-select>
        <el-button icon="el-icon-edit" @click="dialogTopicFormVisible = true" circle></el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="save">提交</el-button>
        <el-button>清空</el-button>
      </el-form-item>
    </el-form>
    <el-dialog title="添加标签" width="30%" :visible.sync="dialogLabelFormVisible">
      <el-form>
        <el-form-item label="标签名称">
          <el-input v-model="label.labelName" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogLabelFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addLabel()">确 定</el-button>
      </div>
    </el-dialog>

    <el-dialog title="添加主题" width="30%" height="40%" :visible.sync="dialogTopicFormVisible">
      <el-form>
        <el-form-item label="主题名称">
          <el-input v-model="topic.topic_name" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogTopicFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addTopic()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { quillEditor, Quill } from "vue-quill-editor"; //调用编辑器
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
import articleApi from "@/api/article";
import labelApi from "@/api/label";

import topicApi from "@/api/topic";
export default {
  components: {
    quillEditor
  },
  data() {
    return {
      dialogLabelFormVisible: false,
      dialogTopicFormVisible: false,
      selectLabelList: [],
      labelList: [],
      selectTopicList: [],
      topicList: [],
      content: "",
      editorOption: { 
        modules:{
          toolbar:[
              ['bold', 'italic', 'underline', 'strike'],    //加粗，斜体，下划线，删除线
              ['blockquote', 'code-block'],     //引用，代码块
              [{ 'header': 1 }, { 'header': 2 }],        // 标题，键值对的形式；1、2表示字体大小
              [{ 'list': 'ordered'}, { 'list': 'bullet' }],     //列表
              [{ 'script': 'sub'}, { 'script': 'super' }],   // 上下标
              [{ 'indent': '-1'}, { 'indent': '+1' }],     // 缩进
              [{ 'direction': 'rtl' }],             // 文本方向
              [{ 'size': ['small', false, 'large', 'huge'] }], // 字体大小
              [{ 'header': [1, 2, 3, 4, 5, 6, false] }],     //几级标题
              [{ 'color': [] }, { 'background': [] }],     // 字体颜色，字体背景颜色
              [{ 'font': [] }],     //字体
              [{ 'align': [] }],    //对齐方式
              ['clean'],    //清除字体样式
              ['image']    //上传图片、上传视频
            ],
　　　　　　　　
              
          }
          },
      id: "",
      imageUrl: "",
      pojo: {},
      label: {},
      topic: {}
    };
  },
  created() {
    this.getLabelList();
    this.getTopicList();
    this.id = this.$route.query.id;
    if (this.id != undefined) {
      articleApi.findById(this.id).then(res => {
        this.pojo = res.data;
        this.content = res.data.content;
        this.imageUrl = res.data.cover;
        if (res.data.label.isEmpty) {
          this.selectLabelList = res.data.label.split(",");
        }
        if (res.data.topic.isEmpty) {
          this.selectTopicList = res.data.topic.split(",");
        }
      });
    }
  },
  computed: {
    editor() {
      return this.$refs.myQuillEditor.quill;
    }
  },
  methods: {
    uploadImage(event) {
      var reader = new FileReader();
      reader.readAsDataURL(event.target.files[0]);
      this.imageUrl = "test";
      reader.onload = function() {
        document.getElementById("img").src = reader.result;
      };
    },
    clickDiv() {
       const input = document.querySelector("#uploadImg");
      input.value = "";
      input.click();
    },
    addLabel() {
      if(this.label.labelName==undefined){
        return false
      }
      this.dialogLabelFormVisible = false;
      labelApi.save(this.label).then(res => {
        this.$message({
          message: "添加成功",
          type: res.flag ? "success" : "error"
        });
        if (res.flag) {
          this.getLabelList();
        }
      });
    },
    addTopic() {
      if(this.topic.topic_name==undefined){
        return false
      }
      this.dialogTopicFormVisible = false;

      topicApi.save(this.topic).then(res => {
        this.$message({
          message: "添加成功",
          type: res.flag ? "success" : "error"
        });
        if (res.flag) {
          this.getTopicList();
        }
      });
    },

    getLabelList() {
      labelApi.getList().then(response => {
        this.labelList = response.data;
      });
    },
    getTopicList() {
      topicApi.getList().then(response => {
        this.topicList = response.data;
      });
    },
    save() {
      var imgurl = document.getElementById("img").src;
      this.pojo.cover = imgurl;
      if (this.pojo.title == undefined || this.pojo.summary == undefined) {
        return false;
      }
      if (this.selectLabelList != undefined) {
        for (var i = 0; i <= this.selectLabelList.length; i++) {
          this.pojo.label = this.selectLabelList.join(",");
        }
      }
      if (this.selectTopicList != undefined) {
        for (var i = 0; i <= this.selectTopicList.length; i++) {
          this.pojo.topic = this.selectTopicList.join(",");
        }
      }
      this.pojo.content = this.content;
      if (this.pojo.id != undefined) {
        articleApi.update(this.pojo.id, this.pojo).then(res => {
          this.$message({
            message: "更新成功",
            type: res.flag ? "success" : "error"
          });
        });
      } else {
        articleApi.save(this.pojo).then(res => {
          this.$message({
            message: "发布成功",
            type: res.flag ? "success" : "error"
          });
          if (res.flag) {
            this.pojo.id = res.data;
          }
        });
      }
    },
    onCancel() {
      this.$message({
        message: "cancel!",
        type: "warning"
      });
    }
  }
};
</script>

<style scoped>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
.line {
  text-align: center;
}
.quill-editor {
  min-height: 200px;
  max-height: 400px;
  overflow-y: auto;
}
</style>
