<style>
/*右侧*/
.pd-con-head-right {
  width: 1000px;
  height: 960px;
  float: left;
  padding: 20px 40px 0px 40px;
  overflow: hidden;
}
#modifiedData h3 {
  border-bottom: 1px solid #dddddd;
  color: #999999;
  line-height: 40px;
  margin-bottom: 20px;
  padding-bottom: 6px;
}
</style>


<template>
  <div id="modifiedData">
    <div class="pd-con-head-right">
      <h3>{{$t('personal.nav3')}}</h3>
      <el-form ref="modifiedDatas" :model="modifiedDatas" label-width="80px">
        <el-form-item :label="$t('modified.con1')">
          <el-col :span="11">
            <el-input v-model="modifiedDatas.Name"></el-input>
          </el-col>
        </el-form-item>
        <el-form-item label="QQ">
          <el-col :span="11">
            <el-input v-model="modifiedDatas.QQ"></el-input>
          </el-col>
        </el-form-item>
        <el-form-item :label="$t('modified.con2')">
          <el-col :span="11">
            <el-input v-model="modifiedDatas.Tel"></el-input>
          </el-col>
        </el-form-item>
        <el-form-item :label="$t('modified.con3')">
          <el-col :span="11">
            <el-input v-model="modifiedDatas.School"></el-input>
          </el-col>
        </el-form-item>
        <el-form-item :label="$t('modified.con4')">
          <el-radio-group v-model="modifiedDatas.Sex">
            <el-radio :label="$t('modified.con5')"></el-radio>
            <el-radio :label="$t('modified.con6')"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item :label="$t('modified.con7')">
          <el-col :span="11">
            <el-date-picker
              type="date"
              placeholder="选择日期"
              v-model="modifiedDatas.Birthday"
              style="width: 100%;"
              value-format="yyyy-MM-dd"
            ></el-date-picker>
          </el-col>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">{{$t('modified.con8')}}</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script type="es6">
import { formatDate } from "@/common/js/date.js";

export default {
  name: "modifiedData",
  components: {},
  data() {
    //在ES6中添加数据是在return{}中
    return {
      modifiedDatas: {
        Name: "",
        QQ: "",
        Tel: "",
        School: "",
        Sex: "",
        Birthday: ""
      }
    };
  },
  created: function() {
    if (localStorage.getItem("token")) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Client/GetClient`,
          async: false,
          xhrFields: {
            withCredentials: true
          },
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`
          }
        })
        .then(function(res) {
          _this.modifiedDatas.Name = res.data.data.name;
          _this.modifiedDatas.QQ = res.data.data.qq;
          _this.modifiedDatas.Tel = res.data.data.tel;
          _this.modifiedDatas.Sex = res.data.data.sex;

          if (res.data.data.birthday == "0001-01-01T00:00:00") {
            _this.modifiedDatas.Birthday = "1901-01-01T00:00:00";
          } else {
            _this.modifiedDatas.Birthday = res.data.data.birthday;
          }

          _this.modifiedDatas.School = res.data.data.school;
        })
        .catch(function(error) {
          console.log(error);
        });
    } else {
    }
  },
  filters: {
    formatDate: function(time) {
      let date = new Date(time);
      return formatDate(date, "yyyy-MM-dd");
    }
  },
  methods: {
    // 修改资料
    onSubmit() {
      const _this = this;

      _this
        .axios({
          method: "put",
          url: `${_this.URLport.serverPath}/client/clients`,
          async: false,
          data: _this.modifiedDatas,
          xhrFields: {
            withCredentials: true
          },
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`
          }
        })
        .then(function(res) {
          _this.GetClient();
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    // 获取个人信息
    GetClient: function() {
      const _this = this;
      if (localStorage.getItem("token")) {
        _this
          .axios({
            method: "get",
            url: `${_this.URLport.serverPath}/Client/GetClient`,
            async: false,
            xhrFields: {
              withCredentials: true
            },
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`
            }
          })
          .then(function(res) {
            _this.$store.state.modified.Name = res.data.data.name;
            if (res.data.status == 1) {
              _this.$message({
                message: _this.$t('modified.con9'),
                type: "success"
              });
            } else {
              _this.$message({
                message: _this.$t('modified.con10z'),
                type: "error"
              });
            }
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    }
  }
};
</script>
