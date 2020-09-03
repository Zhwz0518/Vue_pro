<template>
<!-- 页面布局  -->
   <el-container class="home-container">
   <!-- 头部区域  -->
  <el-header>
  <div>
  <img src="../assets/horse.png" alt="">
  <span>电商后台管理系统</span>
  </div>
  <el-button type="success" plain @click="logout" >退出</el-button>
  </el-header>
  <el-container>
   <!-- 侧边栏区域  -->
    <el-aside :width="isCollapse ? '64px' : '200px'" >
    <div class="toggle-button" @click="toggleCollapse">|||</div>
    <!-- 侧边栏菜单，:collapse="isCollapse"（设置折叠菜单为绑定的 isCollapse 值），:collapse-transition="false"（关闭默认的折叠动画） -->
    <el-menu
      background-color="#c8ebdf"
      text-color="#728bad"
      active-text-color="#03c2e6" unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
      <!-- 一级菜单 -->
      <!-- 通过v-for双重循环渲染左侧菜单 -->
      <el-submenu :index="item.id+ ''" v-for="item in menulist" :key="item.id">
        <!-- 一级菜单的模板区域 -->
        <template slot="title">
          <!-- 图标 -->
          <i :class="iconsObj[item.id]"></i>
          <!-- 文本 -->
          <span>{{item.authName}}</span>
        </template>
          <!-- 二级菜单 -->
          <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{subItem.authName}}</span>
            </template>
          </el-menu-item>
      </el-submenu>
    </el-menu>
    </el-aside>
    <!-- 主体区域  -->
    <el-main>
      <!-- 路由占位符 -->
      <router-view></router-view>
    </el-main>
  </el-container>
</el-container>
</template>

<script>
    export default {
        data() {
            return {
                menulist:[],
                iconsObj: {
                    '125': 'iconfont icon-user',
                    '103': 'iconfont icon-tijikongjian',
                    '101': 'iconfont icon-shangpin',
                    '102': 'iconfont icon-danju',
                    '145': 'iconfont icon-baobiao'
                },
                 // 是否折叠
                isCollapse: false,
                // 被激活的链接地址
                activePath: ''
            }
        },
        created() {
            this.getMenuList();
            this.activePath = window.sessionStorage.getItem('activePath')
        },
        methods: {
            logout(){
                window.sessionStorage.clear();
                this.$router.push("/login");                      
            },
            //发送请求左侧管理列表请求
            async getMenuList(){
                const { data: res } = await this.$http.get('menus')
                //请求失败时，返回错误信息
                if (res.meta.status !== 200) return this.$message.error(res.meta.msg);
                //请求成功，赋值给menulist
                this.menulist=res.data;
                console.log(res)
            },
            // 点击按钮，切换菜单的折叠与展开
            toggleCollapse() {
                this.isCollapse = !this.isCollapse
            },
            // 保存链接的激活状态
            saveNavState(activePath) {
                window.sessionStorage.setItem('activePath', activePath)
                this.activePath = activePath
            }
        }
    }
</script>

<style lang="less" scoped>
.home-container{
    height: 100%;
    .el-header{
    background-color: #bbe6d6;
    display: flex;
    justify-content:space-between;
    align-items: center;
    color: #2c3e50;
    font-size: 20px;
    padding-left: 0;
    > div{
        display: flex;
        align-items: center;
    }
    span{
        margin-left: 5px;
    }
    }
.el-aside{
    background-color: #c8ebdf;
}
.el-main{
    background-color: #e4f5ef;
}
}
.toggle-button {
  background-color: #ade1cc;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>