<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <img src="../assets/logo.png" alt="" />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside class="left-bar" :width="isCollapse?'64px':'200px'">
        <div class="toggle-button" @click="toggleClick">|||</div>
        <!-- 侧边栏 -->
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409EFF"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
        <!-- 一级菜单 -->
          <el-submenu :index="item.id+''" v-for="item in  menulist" :key="item.id">
            <!-- 一级菜单模板区 -->
            <template slot="title">
              <i :class="iconsObj[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="'/'+subItem.path"
             v-for="subItem in item.children" 
             :key="subItem.id"
             @click="saveNavState('/'+subItem.path)"
             >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <!-- 路由站位 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import { log } from 'util';
export default {
  data(){
    return {
      //左侧菜单数据
      menulist:[],
      iconsObj:{
        '125':'iconfont icon-users',
        '103':'iconfont icon-tijikongjian',
        '101':'iconfont icon-shangpin',
        '102':'iconfont icon-danju',
        '145':'iconfont icon-baobiao',
      },
      //是否折叠
      isCollapse:false,
      //被激活的链接地址
      activePath:''
    }
  },
  created() {
      this.getMenuList()
      this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.removeItem("token");
      this.$router.push("/login");
    },
    //获取所有菜单
    async getMenuList(){
      const {data: res} = await this.$http.get('menus')
      if(res.meta.status !== 200) return this.$message.err(res.meta.msg)
      this.menulist = res.data
      console.log(this.menulist);
    },
    //点击切换菜单的折叠与展开
    toggleClick(){
      this.isCollapse = !this.isCollapse
    },
    //保存链接的激活状态
    saveNavState(activePath){
      window.sessionStorage.setItem('activePath',activePath)
      this.activePath = activePath
    }
  },
};
</script>

<style lang="less" scoped>
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #eee;
  font-size: 20px;
  font-weight: 700;
  > div {
    display: flex;
    align-items: center;
    img {
      width: 40px;
      height: 40px;
    }
    span {
      margin-left: 10px;
    }
  }
}
.el-aside {
  background-color: #333744;
  .el-menu{
    border-right: none;
  }
}
.el-main {
  background-color: #eaedf1;
}
.home-container {
  height: 100%;
}
.iconfont{
  margin-right: 10px;
}
.toggle-button{
  background-color: #4A5064;
  font-size: 10px;
  line-height: 25px;
  color: #eee;
  text-align: center;
  letter-spacing: 0.1em;
  cursor: pointer;
}
.left-bar{
  transition: 0.3s;
}
</style>