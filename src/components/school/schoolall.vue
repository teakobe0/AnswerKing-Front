<style>
#schoolall {
  /* background-color: #f0f0f0; */
  min-height: 282px;
  overflow: hidden;
}

/*课程*/
.classes-con {
  width: 1300px;
  margin: 0 auto;
  overflow: hidden;
  margin-top: 24px;
  margin-bottom: 24px;
}
.classes-con-info {
  overflow: hidden;
  margin-bottom: 16px;
}

.classes-con-info p {
  font-size: 18px;
}

.classes-con-info i {
  display: block;
  float: right;
  font-size: 20px;
  font-weight: bold;
  font-style: normal;
  line-height: 40px;
}

.serchinput {
  width: 100%;
  /* float: right; */
}

.classes-con-courseSerch {
  overflow: hidden;
  margin-bottom: 16px;
}

.classes-con-courseSerch ul {
  overflow: hidden;
}

.classes-con-courseSerch ul li {
  list-style-type: none;
  float: left;
}

.classes-con-courseSerch ul li a {
  line-height: 40px;
  color: #000;
  text-decoration: none;
  width: 48.14px;
  text-align: center;
  display: inline-block;
  color: #5b9dfd;
}

.classes-con-courseSerch ul li:first-of-type a {
  text-align: left;
}

.classes-con-courseSerch ul li:first-of-type a:hover {
  background-color: #fff;
  color: #000;
}

.classes-con-courseSerch ul li:first-of-type a:active {
  color: rgb(173, 173, 173);
}

.classes-con-courseSerch ul li a:hover {
  background-color: #08b4e1;
  color: #fff;
  border-radius: 2px;
}
.classes-con-courseSerch ul li a:active {
  background-color: #38bfe0;
}
.classes-con-course {
}

.classes-con-course div {
  display: inline-block;
  list-style-type: none;
  border-bottom: 1px dashed #757575;
  width: 49%;
  margin-bottom: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  overflow: hidden;
  position: relative;
}

.classes-con-course div:nth-last-child(2n + 0) {
  float: right;
  margin-left: 20px;
}

.classes-con-course div a {
  display: block;
  color: #5b9dfd;
  text-decoration: none;
  font-size: 16px;
  white-space: nowrap;
  width: 603px;
  overflow: hidden;
  text-overflow: ellipsis;
  /*border-bottom: 1px dashed #a2a2a2;*/
}

.classes-con-course div a:hover {
  color: #fd2490;
}

.classes-con-course div p {
  color: #b8b8b8;
  font-size: 14px;
  /*float: right;*/
}
.classes-con-course div i {
  position: absolute;
  right: 0px;
  top: 10px;
  cursor: pointer;
  color: #979797;
}
.classes-con-course div i:hover {
  color: red;
}
</style>


<template>
  <div id="schoolall">
    <!-- 所有课程 -->
    <div class="classes-con">
      <div class="classes-con-info">
        <div style="margin-bottom:16px;">
          <p>{{$t('university.con3')}}({{classeslength}})</p>
          <!--<i>找到你的课程</i>-->
        </div>

        <div class="serchinput">
          <el-input :placeholder="$t('university.con4')" v-model="input1" @change="queryname" clearable>
            <i slot="prefix" class="el-input__icon el-icon-search"></i>
          </el-input>
        </div>
      </div>
      <div class="classes-con-courseSerch">
        <ul>
          <li v-for="item in ABC" @click="queryClass(item.name)">
            <a href="javascript:void(0)">{{item.name}}</a>
          </li>
        </ul>
      </div>
      <div class="classes-con-course">
        <div v-for="(item,index) in classes">
          <router-link :to="/classes/+item.id">{{item.name}}</router-link>
          <p>{{$t('university.con5')}}:{{item.order}}</p>
          <i
            class="el-icon-star-off"
            @click="attention(item,index)"
            v-show="item.attentions == false"
            :title="$t('university.con10')"
          ></i>
          <i
            class="el-icon-star-on"
            style="color:red;"
            @click="attention(item,index)"
            v-show="item.attentions == true"
            :title="$t('university.con11')"
          ></i>
        </div>
      </div>
    </div>
  </div>
</template>
<script type="es6">
// import { constants } from "crypto";
export default {
  name: "schoolall",
  components: {},
  data() {
    //在ES6中添加数据是在return{}中
    return {
      input1: "",
      classes: [],
      classeslength: "",
      ABC: [
        { name: "All" },
        { name: "A" },
        { name: "B" },
        { name: "C" },
        { name: "D" },
        { name: "E" },
        { name: "F" },
        { name: "G" },
        { name: "H" },
        { name: "I" },
        { name: "J" },
        { name: "K" },
        { name: "L" },
        { name: "M" },
        { name: "N" },
        { name: "O" },
        { name: "P" },
        { name: "Q" },
        { name: "R" },
        { name: "S" },
        { name: "T" },
        { name: "U" },
        { name: "V" },
        { name: "W" },
        { name: "X" },
        { name: "Y" },
        { name: "Z" }
      ],
      attentions: {
        Name: "",
        TypeId: "",
        Type: ""
      },
      delAttention: "",
      attentionCon: []
    };
  },
  created: function() {
    const _this = this;
    _this.universityidClass();
  },
  methods: {
    //通过学校ID查询拥有的课程
    universityidClass: function() {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Class/Class`,
          async: false,
          params: {
            universityid: _this.$route.params.university_id
          },
          xhrFields: {
            withCredentials: true
          }
        })
        .then(function(res) {
          _this.classes = res.data.data;
          _this.classeslength = _this.classes.length;
          for (var i = 0; i < _this.classes.length; i++) {
            _this.$set(_this.classes[i], "attentions", false);
          }
          _this.retrieveAttention();
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    // 点击字母根据字母查询
    queryClass: function(alifs) {
      const _this = this;
      if (alifs == "All") {
        _this
          .axios({
            method: "get",
            url: `${_this.URLport.serverPath}/Class/Class`,
            async: false,
            params: {
              universityid: _this.$route.params.university_id
            },
            xhrFields: {
              withCredentials: true
            }
          })
          .then(function(res) {
            _this.classes = res.data.data;
            _this.classeslength = _this.classes.length;
            for (var i = 0; i < _this.classes.length; i++) {
              _this.$set(_this.classes[i], "attentions", false);
            }
            _this.retrieveAttention();
          })
          .catch(function(error) {
            console.log(error);
          });
      } else {
        _this
          .axios({
            method: "get",
            url: `${_this.URLport.serverPath}/Class/Class`,
            async: false,
            params: {
              universityid: _this.$route.params.university_id,
              alif: alifs
            },
            xhrFields: {
              withCredentials: true
            }
          })
          .then(function(res) {
            _this.classes = res.data.data;
            for (var i = 0; i < _this.classes.length; i++) {
              _this.$set(_this.classes[i], "attentions", false);
            }
            _this.retrieveAttention();
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    },
    // 输入框搜索查询课程
    queryname: function(names) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Class/Class`,
          async: false,
          params: {
            universityid: _this.$route.params.university_id,
            name: names
          },
          xhrFields: {
            withCredentials: true
          }
        })
        .then(function(res) {
          _this.classes = res.data.data;
          for (var i = 0; i < _this.classes.length; i++) {
            _this.$set(_this.classes[i], "attentions", false);
          }
          _this.retrieveAttention();
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    // 点击关注
    attention: function(item, index) {
      const _this = this;
      if (localStorage.token) {
        item.attentions = !item.attentions;
        if (item.attentions == true) {
          _this.attentions.Name = item.name.toString();
          _this.attentions.TypeId = item.id;
          _this.attentions.Type = 1;
          _this
            .axios({
              method: "post",
              url: `${_this.URLport.serverPath}/Focus/Add`,
              async: false,
              data: _this.attentions,
              xhrFields: {
                withCredentials: true
              },
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`
              }
            })
            .then(function(res) {
              _this.retrieveAttention();
              _this.$message({
                message: _this.$t('classesDetail.con19'),
                type: "success"
              });
            })
            .catch(function(error) {
              console.log(error);
            });
        } else {
          var attentionsId = "";
          for (var i = 0; i < _this.attentionCon.length; i++) {
            var v = _this.attentionCon[i].typeId;
            if (item.id == v) {
              attentionsId = _this.attentionCon[i].id;
            }
          }
          _this
            .axios({
              method: "delete",
              url: `${_this.URLport.serverPath}/Focus/del`,
              async: false,
              params: {
                id: attentionsId
              },
              xhrFields: {
                withCredentials: true
              },
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`
              }
            })
            .then(function(res) {
              _this.$message({
                message: _this.$t('classesDetail.con20'),
                type: "success"
              });
              _this.retrieveAttention();
            })
            .catch(function(error) {
              console.log(error);
            });
        }
      } else {
        _this.$message({
          message: _this.$t('classesDetail.con21'),
          type: "warning"
        });
      }
    },
    // 检索关注
    retrieveAttention: function() {
      const _this = this;
      if (localStorage.token) {
        _this
          .axios({
            method: "get",
            url: `${_this.URLport.serverPath}/Focus/Focus`,
            async: false,
            xhrFields: {
              withCredentials: true
            },
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`
            }
          })
          .then(function(res) {
            _this.attentionCon = res.data.data;
            for (var i = 0; i < res.data.data.length; i++) {
              if (res.data.data[i].type == 1) {
                for (var j = 0; j < _this.classes.length; j++) {
                  if (res.data.data[i].typeId == _this.classes[j].id) {
                    _this.classes[j].attentions = true;
                  }
                }
              }
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
