<!--  -->
<template>
  <div class>
    <!-- 顶部开始 -->
    <div class="app-content-top">
      <div :class="['content-top-left',{chooseAll:chooseAll1&&remaining==0}]" @click="chooseAllTodos">V</div>
      <div class="content-top-right">
        <input @keyup.enter.stop="addTodo" v-model="inputVal" type="text" placeholder="你想干啥？" />
      </div>
    </div>
    <!-- 顶部结束 -->
    <!-- 中部开始 -->
    <div class="app-content-middle">
      <section class="content-list" v-for="(item,index) in filterTodos" :key="item.id">
        <section class="content-left">
          <input type="checkbox" v-model="item.completed" />
        </section>
        <section class="content-middle" @dblclick="dbWriteTodo(index)">
          <span :class="[{comlete:item.completed},{hidden:dbHidden==index}]">{{item.text}}</span>
          <input
            type="text"
            autofocus="autofocus"
            @blur="saveTodo(index)"
            :class="{hidden:dbHidden!=index}"
            v-model="item.text"
          />
        </section>
        <section class="content-right" @click="removeTodo(index)">X</section>
      </section>
    </div>
    <!-- 中部结束 -->
    <!-- 底部开始 -->
    <div class="app-content-botton">
      <div class="content-botton-left">剩下{{remaining}}项</div>
      <div class="content-botton-mid">
        <a href="#/all">全部</a>
        <a href="#/active">未完成</a>
        <a href="#/comlete">已完成</a>
      </div>
      <div class="content-botton-right" @click.stop="removeComletedTodos">删除已完成</div>
    </div>
    <!-- 底部结束 -->
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存放数据
    return {
      todos: [
        { id: 1, text: "待办事项1", completed: false },
        { id: 2, text: "待办事项2", completed: true },
        { id: 3, text: "待办事项3", completed: true },
      ],
      inputVal: "",
      visibility: "all",
      chooseAll1: false,
      dbHidden: -1,
      l:0,
    };
  },
  //监听属性 类似于data概念
  computed: {
    filterTodos: function () {
      // 根据你激活状态 返回不同的todos
      if (this.visibility === "all") {
        return this.todos;
      } else if (this.visibility === "active") {
        return this.todos.filter((v, i) => {
          if (!v.completed) {
            return true;
          } else {
            return false;
          }
        });
      } else {
        return this.todos.filter((v, i) => {
          if (v.completed) {
            return true;
          } else {
            return false;
          }
        });
      }
    },

    //监并返回未完成的数量
    remaining: function () {
      let remainTodos = this.todos.filter((v, i) => {
        if (!v.completed) {
          return true;
        } else {
          return false;
        }
      });
      return this.l = remainTodos.length;
    },
  },
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    saveTodo: function (index) {
      this.dbHidden = -1;
    },
    //双击编辑todo
    dbWriteTodo: function (index) {
      this.dbHidden = index;
    },
    //全选todos
    chooseAllTodos: function () {
      console.log(this.l)
      if (this.l>0) {
        this.todos.forEach((v, i) => {
          v.completed = true;
        });
        this.chooseAll1 = true;
        this.l = this.todos.length
      } else {
        this.todos.forEach((v, i) => {
          v.completed = false;
        });
        this.chooseAll1 = false;
      }
    },
    //删除已完成的todo
    removeComletedTodos: function () {
      let filterTodos = this.todos.filter((v, i) => {
        if (!v.completed) {
          return true;
        } else {
          return false;
        }
      });
      this.todos = filterTodos;
    },
    //删除todo
    removeTodo: function (index) {
      this.todos.splice(index, 1);
    },
    //添加todo
    addTodo: function () {
      let inputVal = this.inputVal;
      if (inputVal.trim() === "") {
        alert("你想干嘛？？？");
      } else {
        this.todos.push({
          id: Date.now(),
          text: inputVal,
          completed: false,
        });
      }
      this.inputVal = "";
    },
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {
    // 监听 Hash 的变化
    let this$1 = this;
    window.addEventListener("hashchange", function () {
      let visibility = location.hash.replace("#/", "");
      console.log(this$1);
      this$1.visibility = visibility;
    });
  },
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style lang="less" scoped>
.comlete {
  text-decoration: line-through;
  color: #ccc;
}
.chooseAll {
  color: red;
}
//中间顶部开始
.app-content-top {
  height: 40px;
  line-height: 40px;
  .content-top-left {
    float: left;
    width: 60px;
    height: 40px;
    line-height: 40px;
    text-align: right;
  }

  .content-top-right {
    float: right;
    height: 40px;
    input {
      width: 300px;
      height: 30px;
      padding-left: 8px;
      border-radius: 8px;
      border: 1px solid #ccc;
    }
  }
}
//中间顶部结束
//中部开始
.hidden {
  display: none;
}
.app-content-middle {
  width: 400px;
  min-height: 40px;
  .content-list {
    width: 400px;
    height: 40px;
    border: red 1px solid;
    line-height: 40px;
    clear: both;
    .content-left {
      width: 65px;
      float: left;
      text-align: right;
      input {
        height: 25px;
        margin-top: 10px;
        margin-right: 10px;
      }
    }

    .content-middle {
      float: left;
      span {
      }

      input {
        width: 220px;
        height: 25px;
        margin-left: 25px;
      }
    }

    .content-right {
      float: right;
      width: 40px;
      padding-right: 10px;
      text-align: right;
    }
  }
}
//中部结束
//底部开始
.app-content-botton {
  width: 400px;
  min-height: 40px;
  line-height: 40px;
  clear: both;
  .content-botton-left {
    height: 40px;
    float: left;
    text-align: left;
  }

  .content-botton-mid {
    height: 40px;
    float: left;
    margin: 0 30px;
    a {
      margin: 0 10px;
    }
  }

  .content-botton-right {
    height: 40px;
    float: left;
  }
}
//底部结束
</style>