<style>
.classesStudy {
  width: 100%;
  height: 100%;
  position: relative;
  padding-bottom: 276px;
}

.clearfix:after {
  content: ".";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}

/* Hides from IE-mac \*/

* html .clearfix {
  height: 1%;
}

/* End hide from IE-mac */
.school-con {
  width: 100%;
  /* margin-top: 80px; */
}

.class-query {
  width: 100%;
  height: 400px;
  background-color: #f2f2f2;
  background-image: url("../assets/学校9.jpg");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.query-con {
  width: 1300px;
  height: 400px;
  margin: 0 auto;
  overflow: hidden;
  text-align: center;
}

.query-con-h1 {
  margin-top: 120px;
  margin-bottom: 20px;
  color: #fff;
}

.query-con-describe {
  color: #fff;
}

.query-con-search input {
  width: 800px;
  height: 100px;
  margin: 0 auto;
  margin-top: 20px;
}

.query-con-search i {
  height: 14px;
  margin-top: 25px;
}

.query-searchbox input {
  height: 50px;
  box-shadow: 1px 1px 10px #cacaca, -1px -1px 10px #cacaca;
}

.query-searchbox input:focus {
  border: 1px solid #cacaca;
}

.find-school {
  width: 1300px;
  margin: 0 auto;
}

.find-school-h1 {
  margin-top: 50px;
  margin-bottom: 20px;
  text-align: center;
}

.find-school-describe {
  margin-bottom: 50px;
  text-align: center;
}

.find-classes {
  width: 1200px;
  margin: 0 auto;
  border-left: 1px solid #dbdee5;
  border-right: 1px solid #dbdee5;
  border-bottom: 1px solid #dbdee5;
  border-radius: 5px;
}

.find-classes-top {
  width: 1200px;
  height: 60px;
}

.select-class {
  width: 400px;
  height: 60px;
  /*background-color: #1d5aff;*/
  float: left;
}

.select-state {
  width: 400px;
  height: 60px;
  /*background-color: #d3f6fe;*/
  float: left;
}

.select-continent {
  width: 400px;
  height: 60px;
  /*background-color: #ff5a00;*/
  float: left;
}

#aaa {
  width: 400px;
  height: 60px;
}

.find-class-bootom {
  width: 1140px;
  margin: 0 auto;
  padding: 30px 30px 60px 30px;
  position: relative;
  font-size: 0;
  z-index: 0;
}

.find-class-bootom ul {
  overflow: hidden;
  margin: 0;
  padding: 0;
  font-size: 0;
}

.find-class-bootom li {
  width: 555px;
  display: inline-block;
  list-style-type: none;
  margin-top: 10px;
  margin-bottom: 10px;

  font-size: 0;
}
.find-class-bootom li:nth-child(odd) {
  margin-right: 30px;
}
.find-class-bootom li:nth-child(even) {
}
.find-class-bootom li a {
  display: block;
  text-decoration: none;
  position: relative;
}
.classes-boo-num {
  font-size: 13px;
  color: #cacaca;
  cursor: pointer;
  font-weight: 100;
}
.classesPaging {
  text-align: center;
  position: relative;
  bottom: 25px;
}
.classes-boo-name {
  color: #08b4e1;
  font-size: 16px;
  font-weight: 700;
  display: block;
  width: 500px;
  height: 18px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 5px;
}
.classes-boo-name:hover {
  color: #bb1308;
}
.classes-boo-order {
  font-size: 13px;
  color: #cacaca;
  cursor: pointer;
  font-weight: 100;
  position: absolute;
  right: 0px;
  bottom: 0px;
}
.find-serchinput {
  width: 100%;
  height: 60px;
}
.find-serchinput span:nth-child(2) {
  left: 25px;
}
.find-serchinput .el-input--prefix .el-input__inner {
  padding-left: 50px;
  height: 60px;
}
</style>

<template>
  <div class="classesStudy clearfix">
    <homeNav></homeNav>
    <div v-title :data-title="$t('nav.nav2')+'-CourseWhale'"></div>
    <div class="school-con">
      <div class="class-query">
        <div class="query-con">
          <h1 class="query-con-h1">{{$t('classes.classes1')}}</h1>

          <p class="query-con-describe">{{$t('classes.classes2')}}</p>

          <div class="query-con-search">
            <el-autocomplete
              class="query-searchbox"
              v-model="state1"
              :fetch-suggestions="querySearch"
              :placeholder="$t('classes.classes3')"
              prefix-icon="el-icon-search"
              :trigger-on-focus="false"
            >
              <template slot-scope="{ item }">
                <router-link :to="'/classes/'+ item.id" style="width: 100%;height: 34px;display: block;">
                  <span style="color:#878787;float:left;">{{item.value}}</span>
                  <span style="color:#878787;float:right;">{{item.type}}</span>
                </router-link>
              </template>
            </el-autocomplete>
          </div>
        </div>
      </div>
      <div class="find-school">
        <h1 class="find-school-h1">{{$t('classes.classes4')}}</h1>

        <p class="find-school-describe">{{$t('classes.classes5')}}</p>

        <div class="find-classes">
          <div class="find-serchinput">
            <el-input
              :placeholder="$t('classes.classes6')"
              v-model="input1"
              @change="GetClasses"
              clearable
            >
              <i slot="prefix" class="el-input__icon el-icon-search"></i>
            </el-input>
          </div>
          <div class="find-class-bootom" v-loading="loading" element-loading-text="拼命加载中">
            <ul>
              <li v-for="item in classesAll">
                <router-link :to="'/classes/'+item.id">
                  <span class="classes-boo-name">{{item.name}}</span>
                  <span class="classes-boo-order">{{item.order}}{{$t('classes.classes8')}}</span>
                </router-link>

                <div class="classes-boo-num">
                  <span>{{$t('classesDetail.con2')}}:{{item.university}}</span>
                </div>
              </li>
            </ul>
          </div>
          <div class="classesPaging" v-if="classesPagings == true">
            <el-pagination
              @current-change="handleCurrentChange"
              :current-page="currentPage4"
              :page-size="40"
              layout="total, prev, pager, next, jumper"
              :total="pageTotal"
            ></el-pagination>
          </div>
        </div>
      </div>
    </div>
    <div style="height: 40px;"></div>
    <homeFooter></homeFooter>
  </div>
</template>

<script type="es6">
import homeNav from "@/components/public/homeNav.vue";
import homeFooter from "@/components/public/homeFooter.vue";

export default {
  name: "classesStudy",
  components: {
    homeNav,
    homeFooter
  },
  data() {
    return {
      state1: "",
      value1: "美国",
      value2: "",
      value3: "",
      // 初始容器
      classesAll: [],
      // 显示容器
      universitys: [],
      uns: [],
      country: [],
      state: [],
      city: [
        //{
        //    value: '选项1',
        //    label: '加利福尼亚州'
        //},
        //{
        //    value: '选项2',
        //    label: '加州'
        //}
      ],
      pageSize: 40,
      queryString: "",
      loading: true,
      currentPage4: 1,
      pageTotal: 0,
      classesPagings: false,
      // 搜索课程
      input1: ""
    };
  },
  created: function() {
    const _this = this;
    _this.GetClasses();
    document.documentElement.scrollTop = 0;
  },
  methods: {
    // 搜索框搜索课程
    querySearch(queryString, cb) {
      const _this = this;
      var valuestr = queryString.trim();
      var patt = /^[\s]*$/; //以空格开头并且已空格结尾，中间多次或者零次空格
      clearTimeout(_this.timeout);
      if (valuestr.length == 0) {
        console.log("空格");
      } else {
        _this.timeout = setTimeout(() => {
          if (queryString.length >= 3) {
            _this.inputLoad = true;
            _this.state2 = queryString;
            _this.queryString = queryString;
            var results = [];
            _this
              .axios({
                method: "get",
                url: `${_this.URLport.serverPath}/Class/ClassPage`,
                async: false,
                params: {
                  name: valuestr,
                  pagenum: 1,
                  pagesize: 10
                },
                xhrFields: {
                  withCredentials: true
                }
              })
              .then(function(res) {
                if (
                  res.data.data.data.length > 0
                ) {
                  for (var i = 0; i < 10; i++) {
                    if (res.data.data.data[i]) {
                      results.push({
                        value: res.data.data.data[i].name,
                        type: res.data.data.data[i].university,
                        class: "classes",
                        num: i,
                        id: res.data.data.data[i].id
                      });
                    }
                  }
                } else {
                  results.push({ value: _this.$t('classes.classes7') });
                }

                cb(results);
              })
              .catch(function(error) {
                console.log(error);
              });
          }
        }, 1000 * Math.random());
      }
    },
    // 根据课程名称检索课程分页
    GetClasses: function(index) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Class/ClassPage`,
          async: false,
          params: {
            name: _this.input1,
            pagenum: 1,
            pagesize: 40
          },
          xhrFields: {
            withCredentials: true
          }
        })
        .then(function(res) {
          _this.classesAll = res.data.data.data;
          _this.loading = false;
          _this.classesPagings = true;
          _this.pageTotal = res.data.data.pageTotal;
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    // 课程分页跳转
    handleCurrentChange(val) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Class/ClassPage`,
          async: false,
          params: {
            name: _this.input1,
            pagenum: val,
            pagesize: 40
          },
          xhrFields: {
            withCredentials: true
          }
        })
        .then(function(res) {
          _this.classesAll = res.data.data.data;
          _this.loading = false;
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>