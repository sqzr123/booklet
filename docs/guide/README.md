# **vue**中提供了依赖包

```npm install svg-sprite-loader ```

# **接下来就是环境的配置**
    1·配置vue.config.js
   
```
chainWebpack: (config) => {
    // svg图标加载
    config.module
      .rule("svg")
      .exclude.add(path.join(__dirname, "src/assets/svg"))
      .end();

    config.module
      .rule("icons") // 定义一个名叫 icons 的规则
      .test(/\.svg$/) // 设置 icons 的匹配正则
      .include.add(path.join(__dirname, "src/assets/svg")) // 设置当前规则的作用目录，只在当前目录下才执行当前规则
      .end()
      .use("svg-sprite") // 指定一个名叫 svg-sprite 的 loader 配置
      .loader("svg-sprite-loader") // 该配置使用 svg-sprite-loader 作为处理 loader
      .options({
        // 该 svg-sprite-loader 的配置
        symbolId: "icon-[name]",
      })
      .end();
  },
```
    2·svg矢量图的存储路径
   路径得跟config里自己配置的一致 
   
    3·svg的组件封装
    
```
<template>
  <i v-if="iconFileName.indexOf('el-icon-') === 0" :class="iconFileName" />
  <svg v-else class="svg-icon" aria-hidden="true" v-on="$listeners">
    <use :xlink:href="`#icon-${iconFileName}`" />
  </svg>
</template>

<script>
export default {
  name: 'SvgIcon',
  props: {
    iconFileName: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  overflow: hidden;
  vertical-align: -0.15em;
  fill: currentColor;
}
</style>

```
    4·在svg文件中创建js并引入svg组件
    
```
import Vue from "vue";
import svgIcon from "@/components/svgIcon";
Vue.component("svg-icon", svgIcon); // 全局注册svgIcon

const req = require.context("@/assets/icons/svg", false, /\.svg$/);
const requireAll = (requireContext) => {
  // requireContext.keys()数据：['./404.svg', './agency.svg', './det.svg', './user.svg']
  requireContext.keys().map(requireContext);
};
requireAll(req);

```
    5·在全局中(mian.js)使用svg   （在svg的js中引入了svg组件，这里就是使用svg）
    
```
import '@/assets/icons/index'
```
    6·最后就是使用svg
    
```
<svg-icon   className=“需要修改的样式”  />
```