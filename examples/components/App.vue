<template>
  <div class="app">
    <div class="nav-header">
      <img src="../assets/logo.png">
      <IMenu class="pull-right" activeName="key1" v-on:on-select="onMenuSelect">
        <iMenuItem name="key1"><icon name="user">menu2</icon></iMenuItem>
        <iMenuItem name="key2"><icon name="user">menu3</icon></iMenuItem>
        <iMenuItem name="key3"><icon name="user">menu4</icon></iMenuItem>
        <iMenuItem name="key4"><icon name="user">menu5</icon></iMenuItem>
      </IMenu>
    </div>
    <div class="nav-left">
      <ul class="nav nav-stack">
        <router-link tag="li" active-class="active" to="/"><a>首页</a></router-link>
        <router-link tag="li" active-class="active" to="/Login"><a>登录</a></router-link>
        <router-link tag="li" active-class="active" to="/demo"><a>示例</a></router-link>
       <!-- <li><router-link to="/" :class="{active: pathName === 'home'}">首页</router-link> </li>
        <li><router-link to="/Login" :class="{active: pathName === 'login'}">登录</router-link> </li>
        <li><router-link to="/demo" :class="{active: pathName === 'demo'}">Demo</router-link> </li>-->
      </ul>
    </div>
    <transition :name="transName">
      <router-view class="content-view"></router-view>
    </transition>
  </div>
</template>

<script>
  import Mixin from '../mixins'
  export default {
    name: 'app',
    mixins: [Mixin],
    data () {
      return {
        transName: 'slide-left'
      }
    },
    watch: {
      $route (to, from) {
        this.toPath(to.path, from.path)
      }
    },
    methods: {
      toPath (toPath, fromPath) {
        const toDepth = toPath.split('/').length
        const fromDepth = fromPath.split('/').length
        this.transName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
      },
      onMenuSelect (name) {
        console.log('select the menuName is ' + name)
      }
    },
    created () {
      if (this.path) {
        this.jump(this.path)
      }
    }
  }
</script>

<style scoped>
  .nav-header {
    position: fixed;
    display: block;
    height: 55px;
    top: 0;
    left: 0;
    right: 0;
  }
  .nav-header img {
    width: 55px;
    height:55px;
  }
  .nav-left {
    position: fixed;
    left: 0;
    width: 220px;
  }
  .app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin-top: 55px;
  }

  .content-view{
    position: absolute;
    left: 220px;
    right: 0;
    top: 55px;
    bottom: 0;
    transition: all .5s ease;
  }

  .slide-left-enter, .slide-right-leave-active{
    opacity: 0;
    -webkit-transform: translate(900px, 0);
    transform: translate(900px, 0);
  }
  .slide-left-leave-active, .slide-right-enter{
    opacity: 0;
    -webkit-transform: translate(-900px, 0);
    transform: translate(-900px, 0);
  }
</style>
