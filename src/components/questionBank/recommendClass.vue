<style>
.content-tag-con-right-con-div {
  width: 290px;
  padding: 8px;
  border: 1px solid #e1e1e1;
}

.content-tag-con-right-con-div ul {
  list-style-type: none;
  margin-left: 50px;
}

.content-tag-con-right-con-div ul li {
  margin-top: 10px;
  margin-bottom: 10px;
  position: relative;
}

.content-tag-con-right-con-div ul li i {
  position: absolute;
  left: -38px;
  top: 8px;
  color: #5d5d5d;
  font-size: 30px;
}

.content-tag-con-right-con-div ul a {
  font-size: 16px;
  color: #4b4b4b;
  cursor: pointer;
  text-decoration: none;
  width: 230px;
  overflow: hidden;
  display: block;
  word-wrap: break-word;
}
.content-tag-con-right-con-div ul a:hover {
  font-size: 16px;
  color: red;
}
.content-tag-con-right-con-div ul span {
  font-size: 14px;
  color: #838383;
  display: block;
  margin-top: 3px;
  width: 230px;
  overflow: hidden;
  word-wrap: break-word;
}
.kong {
  height: 40px;
}
</style>

<template>
  <div class="content-tag-con-right-con-div" v-loading="loading">
    <div class="kong" v-show="kong == true"></div>
    <ul v-for="item in universityClass">
      <li>
        <i class="el-icon-tickets"></i>
        <router-link :to="'/classes/'+item.id">{{item.name}}</router-link>
        <span>{{item.university}}</span>
        <span>{{$t('content.con22')}}:{{item.order}}</span>
      </li>
    </ul>
  </div>
</template>

<script type="es6">
export default {
  name: "recommendClass",
  data() {
    return {
      // 学校课程
      universityClass: [],
      loading: false,
      kong: true
    };
  },
  props: ["universityId"],
  created: function() {
    const _this = this;
    _this.universityidClass();
  },
  methods: {
    // 通过学校ID查询所有课程并展示题库数量多的
    universityidClass: function() {
      const _this = this;
      _this.loading = true;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Class/Class`,
          async: false,
          params: {
            universityid: _this.universityId
          },
          xhrFields: {
            withCredentials: true
          }
        })
        .then(function(res) {
           
          _this.loading = false;
          _this.kong = false;
          sortByKey(res.data.data, "order");
          //数组对象排序
          function sortByKey(array, key) {
            array.sort(function(a, b) {
              var x = a[key];
              var y = b[key];
              return y < x ? -1 : x > y ? 1 : 0;
            });
          }
          for (var i = 0; i < 5; i++) {
            if (res.data.data[i].id != _this.$route.params.classes_id) {
              _this.universityClass.push(res.data.data[i]);
            }
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>

