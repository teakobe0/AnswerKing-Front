<style>
.inform-right {
  width: 1000px;
  float: left;
  padding: 20px 40px 0px 40px;
  overflow: hidden;
}
#inform h3 {
  border-bottom: 1px solid #dddddd;
  color: #999999;
  line-height: 40px;
  padding-bottom: 6px;
  margin-bottom: 20px;
}
.message {
  margin-top: 24px;
}
.message div {
  margin-bottom: 8px;
  overflow: hidden;
  word-wrap: break-word;
  line-height: 28px;
  color: #444;
}
.message div el-button {
  float: right;
}
.message .sendname {
  color: #5b9dfd;
  vertical-align: auto;
}
.message .btn-next {
  background-color: #fafafa !important;
}
</style>


<template>
  <div id="inform">
    <div class="inform-right">
      <h3>{{ $t("personal.nav5") }}</h3>
      <div class="message">
        <div style="text-align: center" v-show="dataNull == true">
          {{ $t("inform.con1") }}
        </div>
        <div v-for="item in messages">
          <div style="float: left; width: 850px">
            <span class="sendname">{{ item.sendname }}</span>
            <span v-show="item.type == 1 || item.type == 3"
              >{{ $t("inform.con2") }}：</span
            >
            <span v-show="item.type == 2">{{$t("inform.con5")}}:</span>
            <span v-html="item.contentsUrl"></span>
          </div>
          <div style="float: right">
            <el-button
              @click="del(item.id)"
              v-show="item.type == 1 || item.type == 3"
              style="float: right; margin-left: 10px"
              size="mini"
              >{{ $t("inform.con4") }}</el-button
            >
            <el-button
              @click="gomessage(item.contentsId, item.id, item.type)"
              v-show="item.type == 1 || item.type == 3"
              style="float: right"
              size="mini"
              >{{ $t("inform.con3") }}</el-button
            >

            <el-button
              @click="del(item.id)"
              v-show="item.type == 2"
              style="float: right"
              size="mini"
              >{{ $t("inform.con4") }}</el-button
            >
            <el-button
              @click="inform(item.sendId, item.sendname)"
              v-show="item.type == 2 && item.sendId != 0"
              style="float: right"
              size="mini"
              >{{ $t("content.con14") }}</el-button
            >
          </div>
        </div>
        <el-pagination
          @current-change="handleCurrentChange"
          :page-size="pagesizes"
          layout="total, prev, pager, next"
          :total="totals"
          style="text-align: center"
          v-show="totals > 16"
        >
        </el-pagination>
      </div>
    </div>
  </div>
</template>
<script type="es6">
import { formatDate } from "@/common/js/date.js";

export default {
  name: "inform",
  components: {},
  data() {
    //在ES6中添加数据是在return{}中
    return {
      activeName: "first",
      message: "reply",
      messages: [],
      personreviewsid: "",
      ids: "",
      classinfoid: "",
      dataNull: false,
      pagenums: 1,
      pagesizes: 16,
      totals: 0,
    };
  },
  created: function () {
    const _this = this;
    _this.gainpersonal();
  },
  filters: {
    formatDate: function (time) {
      let date = new Date(time);
      return formatDate(date, "yyyy-MM-dd");
    },
  },
  methods: {
    // 获取个人信息
    gainpersonal: function () {
      const _this = this;
      if (localStorage.getItem("token")) {
        _this
          .axios({
            method: "get",
            url: `${_this.URLport.serverPath}/Client/GetClient`,
            async: false,
            xhrFields: {
              withCredentials: true,
            },
            headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`,
            },
          })
          .then(function (res) {
            _this.personreviewsid = res.data.data.id;
            _this.gainmessage();
          })
          .catch(function (error) {
            console.log(error);
          });
      } else {
      }
    },
    // 检索通知
    gainmessage: function () {
      const _this = this;
      // type : 0(系统通知)1(题库集)2(对话聊天)3(问答详情)
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Notice/Notices`,
          async: false,
          params: {
            pagenum: _this.pagenums,
            pagesize: _this.pagesizes,
          },
          xhrFields: {
            withCredentials: true,
          },
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then(function (res) {
          if (res.data.status == 1) {
            _this.$store.state.logo.message = res.data.data.length;
            _this.messages = res.data.data.data;
            _this.totals = res.data.data.pageTotal;
            if (_this.messages.length == 0) {
              _this.dataNull = true;
            } else {
              // for (var i = 0; i < _this.messages.length; i++) {
              //   _this.$set(_this.messages[i], "content", []);
              //   _this.$set(_this.messages[i], "types", 0);
              //   var a = _this.messages[i].contentsUrl.split(",");
              //   var regPos = /^\+?[1-9][0-9]*$/;
              //   if (regPos.test(a[a.length - 1])) {
              //     _this.messages[i].types = 1; //1代表这是从评论过来的
              //     var b = a[0].split(":");
              //     var c = [];
              //     for (var j = 1; j < a.length - 2; j++) {
              //       c.push(a[j]);
              //     }
              //     if (b.length >= 2) {
              //       b.splice(0, 1);
              //       _this.messages[i].content = b[0] + c.join();
              //     } else {
              //       _this.messages[i].content = b[0] + c.join();
              //     }
              //   } else {
              //     _this.messages[i].types = 2; //2代表这是从问答留言过来的
              //     _this.messages[i].content = _this.messages[i].contentsUrl;
              //   }
              // }
            }
          }
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    // 跳转评论页面
    gomessage: function (url, ids, type) {
      const _this = this;
      // var c = "women,jiushi,234234m2,234,23,45";
      // var a = c.split(",");
      // // console.log(a)
      // var b = [];
      // var c = "";
      // for (var i = 0; i < a.length - 2; i++) {
      //   b.push(a[i]);
      // }
      // console.log(b.join());
      // console.log(c);

      // _this.ids = a[a,length-2];
      // _this.classinfoid = a[a.length-1];
      if (type == 1) {
        var a = url.split(",");
        _this.$router.push({
          path:
            "/classes/" +
            a[a.length - 2] +
            "/content/" +
            a[a.length - 1] +
            "/weeks/" +
            0 +
            "/weektype/" +
            0,
        });
      } else if (type == 3) {
        _this.$router.push({
          path: "/questionDetails/" + url,
        });
      }
    },
    // 删除留言
    del(ids) {
      const _this = this;
      _this
        .axios({
          method: "put",
          url: `${_this.URLport.serverPath}/Notice/ChangeStatus`,
          async: false,
          params: {
            id: ids,
          },
          xhrFields: {
            withCredentials: true,
          },
        })
        .then(function (res) {
          _this.gainmessage();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    // 通知留言
    inform(item, name) {
      const _this = this;

      this.$prompt("对" + " " + name + " " + "留言:", "CourseWhale", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
      })
        .then(({ value }) => {
          var json = {};
          json.ReceiveId = item; //接收人ID
          json.ContentsUrl = value; //通知内容
          _this
            .axios({
              method: "post",
              url: `${_this.URLport.serverPath}/Notice/Add`,
              async: false,
              data: json,
              xhrFields: {
                withCredentials: true,
              },
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`,
              },
            })
            .then(function (res) {
              if (res.data.status == 1) {
                _this.gainmessage();
                _this.$message({
                  message: "发布成功",
                  type: "success",
                });
              } else {
                _this.$message({
                  message: "发布失败",
                  type: "error",
                });
              }
            })
            .catch(function (error) {
              console.log(error);
            });
        })
        .catch(() => {});
    },
    // 分页
    handleCurrentChange(val) {
      const _this = this;
      _this
        .axios({
          method: "get",
          url: `${_this.URLport.serverPath}/Notice/Notices`,
          async: false,
          params: {
            pagenum: val,
            pagesize: _this.pagesizes,
          },
          xhrFields: {
            withCredentials: true,
          },
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then(function (res) {
          if (res.data.status == 1) {
            _this.$store.state.logo.message = res.data.data.length;
            _this.messages = res.data.data.data;
            _this.totals = res.data.data.pageTotal;
            if (_this.messages.length == 0) {
              _this.dataNull = true;
            }
          }
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
};
</script>
