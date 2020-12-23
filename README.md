# mytable

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
实现功能
/**
* 列表组件:
1.固定表头,固定列
2.按列排序,筛选过滤
3.树形数据(支持异步)
4.拖拽列宽,拖拽列顺序(下次打开时记忆)
5.选择显示列(只看部分列)
*/
采用elementui + sortablejs 实现
