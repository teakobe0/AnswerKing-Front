<style>
.schoolStydy {
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

.school-query {
  width: 100%;
  height: 400px;
  background-color: #f2f2f2;
  background-image: url("../assets/学校14.jpg");
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

.find-select {
  width: 1200px;
  margin: 0 auto;
  border-left: 1px solid #dbdee5;
  border-right: 1px solid #dbdee5;
  border-bottom: 1px solid #dbdee5;
  border-radius: 5px;
}

.find-select-top {
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

.find-select {
  width: 1200px;
  margin: 0 auto;
  margin-bottom: 20px;
}

.find-select-bootom {
  width: 1140px;
  margin: 0 auto;
  padding: 30px 30px 60px 30px;
  position: relative;
  font-size: 0;
  z-index: 0;
}

.find-select-bootom ul {
  overflow: hidden;
  margin: 0;
  padding: 0;
  font-size: 0;
}

.find-select-bootom li {
  width: 555px;
  display: inline-block;
  list-style-type: none;
  margin-top: 10px;
  margin-bottom: 10px;
  font-size: 0;
}
.find-select-bootom li:nth-child(odd) {
  margin-right: 30px;
}

.find-select-bootom li a {
  display: block;
  color: #08b4e1;
  font-size: 16px;
  font-weight: 700;
  text-decoration: none;
}

.find-select-bootom li a:hover {
  color: #bb1308;
  text-decoration: none;
}

.select-boo-num {
  font-size: 13px;
  color: #c3c3c3;
  margin-top: 5px;
  cursor: pointer;
}
.viewmores {
  width: 100%;
  height: 40px;
  background: rgb(247, 247, 247);
  text-align: center;
  line-height: 40px;
  cursor: pointer;
}
</style>

<template>
  <div class="schoolStydy clearfix">
    <homeNav></homeNav>
    <div v-title data-title="学校资源-CourseWhale"></div>
    <div class="school-con">
      <div class="school-query">
        <div class="query-con">
          <h1 class="query-con-h1">{{ $t("schools.schools1") }}</h1>

          <p class="query-con-describe">{{ $t("schools.schools2") }}</p>

          <div class="query-con-search">
            <el-autocomplete
              class="query-searchbox"
              v-model="state1"
              :fetch-suggestions="querySearchs"
              :placeholder="$t('schools.schools3')"
              prefix-icon="el-icon-search"
              :trigger-on-focus="false"
            >
              <template slot-scope="{ item }">
                <router-link
                  :to="'/university/' + item.id"
                  style="width: 100%; height: 34px; display: block"
                >
                  <span style="color: #878787; float: left">{{
                    item.value
                  }}</span>
                  <span style="color: #878787; float: right">{{
                    item.type
                  }}</span>
                </router-link>
              </template>
            </el-autocomplete>
          </div>
        </div>
      </div>
      <div class="find-school">
        <h1 class="find-school-h1">{{ $t("schools.schools4") }}</h1>

        <p class="find-school-describe">{{ $t("schools.schools5") }}</p>

        <div class="find-select">
          <div class="find-select-top">
            <div class="select-class">
              <el-select
                v-model="value1"
                filterable
                id="aaa"
                placeholder="国家"
                @change="handleSelectcountry"
              >
                <el-option
                  v-for="item in country"
                  size:medium
                  :key="item.country"
                  :label="item.country"
                  :value="item.country"
                ></el-option>
              </el-select>
            </div>
            <div class="select-state">
              <el-select
                v-model="value2"
                filterable
                clearable
                id="aaa"
                placeholder="州/省"
                @change="handleSelectstate"
              >
                <el-option
                  v-for="item in state"
                  size:medium
                  :key="item.state"
                  :label="item.state"
                  :value="item.state"
                ></el-option>
              </el-select>
            </div>
            <div class="select-continent">
              <el-select
                v-model="value3"
                filterable
                clearable
                id="aaa"
                placeholder="市"
                @change="handleSelectcity"
              >
                <el-option
                  v-for="item in city"
                  size:medium
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </div>
          </div>
          <div
            class="find-select-bootom"
            v-loading="loading"
            element-loading-text="拼命加载中"
          >
            <ul>
              <li v-for="item in universitys">
                <router-link :to="'/university/' + item.id">{{
                  item.name
                }}</router-link>
                <div class="select-boo-num">
                  {{ item.number }}{{ $t("schools.schools7") }}
                </div>
              </li>
            </ul>
          </div>
          <div @click="viewMore" class="viewmores" v-show="viewMores == true">
            {{ $t("schools.schools6") }}
            <i class="el-icon-caret-bottom"></i>
          </div>
        </div>
      </div>
    </div>
    <div style="height: 40px"></div>
    <homeFooter></homeFooter>
  </div>
</template>

<script type="es6">
import homeNav from "@/components/public/homeNav.vue";
import homeFooter from "@/components/public/homeFooter.vue";

export default {
  name: "schoolStydy",
  components: {
    homeNav,
    homeFooter,
  },
  data() {
    return {
      state1: "",
      value1: "美国",
      value2: "",
      value3: "",
      // 初始容器
      alluniversitys: [],
      // 显示容器
      universitys: [],
      uns: [],
      country: [],
      state: [],
      city: [],
      pageSize: 40,
      queryString: "",
      loading: true,
      viewMores: false,
    };
  },
  created: function () {
    const _this = this;
    _this.Countrys();
    _this.handleSelectcountry();
    document.documentElement.scrollTop = 0;
  },
  methods: {
    // 检索国家
    Countrys() {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/University/Countrys`,
          async: false,
          xhrFields: {
            withCredentials: true,
          },
        })
        .then(function (res) {
          _this.country = res.data.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    // 查看更多按钮
    viewMore: function () {
      const _this = this;
      _this.universitys = _this.alluniversitys.slice(
        0,
        _this.universitys.length + _this.pageSize
      );
      if (_this.universitys.length - 1 == _this.alluniversitys.length - 1) {
        _this.viewMores = false;
      }
      return sortByKey(_this.universitys, "number");
      //数组对象排序
      function sortByKey(array, key) {
        return array.sort(function (a, b) {
          var x = a[key];
          var y = b[key];
          return y < x ? -1 : x > y ? 1 : 0;
        });
      }
    },
    // 输入框触发检索学校
    querySearchs(queryString, cb) {
      const _this = this;
      var valuestr = queryString.trim();
      var patt = /^[\s]*$/; //以空格开头并且已空格结尾，中间多次或者零次空格
      clearTimeout(_this.timeout);
      if (valuestr.length == 0) {
        console.log("空格");
      } else {
        _this.timeout = setTimeout(() => {
          if (queryString.length >= 1) {
            var results = [];
            _this
              .axios({
                method: "get",
                url: `${_this.URLport.serverPath}/University/Universitys`,
                async: false,
                params: {
                  name: valuestr,
                },
                xhrFields: {
                  withCredentials: true,
                },
              })
              .then(function (res) {
                if (res.data.data.length > 0) {
                  for (var i = 0; i < 10; i++) {
                    if (res.data.data[i]) {
                      results.push({
                        value: res.data.data[i].name,
                        type: _this.$t("upload.u10"),
                        id: res.data.data[i].id,
                      });
                    }
                  }
                } else {
                  results.push({
                    value: _this.$t("schools.schools8"),
                    type: null,
                  });
                }

                cb(results);
              })
              .catch(function (error) {
                console.log(error);
              });
          }
        }, 1000 * Math.random());
      }
    },
    //根据国家 州/省份检索学校
    GetUniversitys: function (index) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/University/GetUniversitys`,
          async: false,
          params: {
            name: _this.value1,
            state: _this.value2,
          },
          xhrFields: {
            withCredentials: true,
          },
        })
        .then(function (res) {
          _this.alluniversitys = res.data.data;
          _this.loading = false;
          _this.viewMores = true;
          sortByKey(_this.alluniversitys, "number");
          //数组对象排序
          function sortByKey(array, key) {
            array.sort(function (a, b) {
              var x = a[key];
              var y = b[key];
              return y < x ? -1 : x > y ? 1 : 0;
            });
          }
          _this.viewMore();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    sort: function () {
      const _this = this;
      _this.universitys = _this.alluniversitys.slice(0, _this.pageSize);
    },
    // 根据国家检索州
    handleSelectcountry: function (item) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/University/States`,
          async: false,
          params: {
            name: _this.value1,
          },
          xhrFields: {
            withCredentials: true,
          },
        })
        .then(function (res) {
          _this.state = res.data.data;
          _this.value2 = "";
          _this.value3 = "";
          _this.GetUniversitys();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    // 根据州检索市
    handleSelectstate: function () {
      const _this = this;
      _this.value3 = "";
      _this.GetUniversitys();
    },
    // 根据市检索
    handleSelectcity: function () {
      const _this = this;
    },
  },
};
</script>