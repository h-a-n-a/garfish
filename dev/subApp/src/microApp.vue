<template>
  <div id="app">
      <el-button plain @click="loadApp">
        加载子应用
      </el-button>
      <el-button plain @click="loadAppReact">
        加载 React 子应用
      </el-button>
    <div ref="vueApp" id="vueApp"></div>
  </div>
</template>

<script>
// 嵌套场景中
import GarfishInstance from 'garfish';

let hasInit = false;
export default {
  name: 'App',
  props: ['basename'],
  methods: {
    async loadApp () {
      console.log(GarfishInstance);
      // let app = await GarfishInstance.loadApp('vueApp',{
      //   entry: 'http://localhost:8000',
      //   basename: this.basename,
      //   domGetter: ()=> this.$refs.vueApp
      // });
      // await app.mount();
      // console.log(app);
      Garfish.router.push({ path: '/vueApp', basename: this.basename })
      // console.log(this.basename, GarfishInstance);
    },
    async loadAppReact () {
      console.log(GarfishInstance);
      // let app = await GarfishInstance.loadApp('vueApp',{
      //   entry: 'http://localhost:8000',
      //   basename: this.basename,
      //   domGetter: ()=> this.$refs.vueApp
      // });
      // await app.mount();
      // console.log(app);
      Garfish.router.push({ path: '/reactApp', basename: this.basename })
      // console.log(this.basename, GarfishInstance);
    }
  },
  mounted () {
    if (hasInit) return;
    hasInit = true;
    GarfishInstance.run({
      basename: '/garfish_master/vue',
      nested: !!window.__GARFISH_PARENT__,
      apps: [
        {
          name: 'vueApp',
          entry: 'http://localhost:8000',
          basename: '/garfish_master/vue',
          activeWhen: '/vueApp',
          cache: true,
          domGetter: ()=> document.querySelector('#vueApp')
        },
        {
          name: 'reactApp',
          entry: 'http://localhost:3000',
          basename: '/garfish_master/vue',
          activeWhen: '/reactApp',
          cache: true,
          domGetter: ()=> document.querySelector('#vueApp')
        }
      ],
      async beforeLoad(appInfo) {
        console.log('开始加载了', appInfo);
        // return Promise.resolve();
      },
    });
    // Can only be run once
  },
  components: {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
