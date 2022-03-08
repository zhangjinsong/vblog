---
title: vue
date: 2022-02-19 01:58:30
permalink: /pages/bf43aa/
categories:
  - 标准
tags:
  - vue
---
# VUE 规范

## 命名分类

- camelCase（驼峰式，也叫小驼峰命名，e.g. userInfo）
- PascalCase（帕斯卡命名式，也叫大驼峰命名，e.g. UserInfo）
- kebab-case（短横线连接式，e.g. user-info）
- snake_case（下划线连接式，e.g. user_info）

## 标签使用

- 除了 div ul>li 标签外，其他 html 标签内不能含有任务组件的标签（包括 element-ui 标签）
- `el-row` 必须包含`el-col` 不要单独使用`el-col`

## 样式

- 使用`scss`语法 每个`.vue`文件下的 style 都必须加上`scope`属性

```javascript
<style rel="stylesheet/scss" lang="scss" scoped />
```

## 组件建议

::: tip
1. 每个 Vue 组件的代码建议不要超出 200 行，如果超出建议拆分组件。
2. 组件一般情况下是可以拆成基础/ui 部分和业务部分，基础组件一般是承载呈现，基础功能，不和业务耦合部分。
3. 业务组件一般包含业务功能业务特殊数据等等。
:::

## 组件命名

组件文件名除 index.vue 之外，一律采用 PascalCase(**大驼峰**)写法。原因是引入组件时，组件的变量通常用 PascalCase 格式，以区别于一般变量。组件文件名与变量名一致，方便对应。

```js
import UserBook from "./user/UserBook";
```

组件名应该始终是多个单词的，根组件 App 除外
html 元素都是单个单词的（如 `<article>`,`<header>`)，为了区分组件和一般 html 元素，组件由多个单词组成，如`BookItem.vue`，单独的`Book.vue`不推荐。

## UI 组件/基础组件

开发的时候注意可拓展性，支持数据传参进行渲染，支持插槽 slot。  
 设置有 mixin，mixin 中放了基础信息和方法。

## 容器组件

和当前业务耦合性比较高，由多个基础组件组成，可承载当前页的业务接口请求和数据(vuex)。

## 组件存放位置

ui 组件存放在 src/components/ 中，包含 xxx.vue 和 xxmixin.js 和 readme.md。

## 组件通讯

避免数据的分发源混乱，不建议使用 eventBus 控制数据，应使用 props 来和 \$emit 来数据分发和传送。
同级组件的通讯一般会有一个中间容器组件作为桥梁。
容器组件作为数据的接受和分发点。

## 组件的挂在和销毁

通过v-if控制组件的挂在和销毁
```html
<testcomponent v-if="componentActive"></testcomponent>
```

通过is控制组件的挂在和销毁
```html
<component is="componentName"></component>
```

## Vue 文件结构

顺序：template -> script -> style。一个组件尽量不要超过 200 行，页面包含独立部分时尽量分离成子组件

```vue
<template>
  <div>...</div>
</template>
<script>
export default {
  components: {},
  data() {
    return {};
  },
  created() {},
  methods: {}
};
</script>
<!-- 声明语言，并且添加scoped -->
<style lang="scss" scoped>
...
</style>
```

组件/实例的选项顺序

```md
- name (全局引用)
- components (模板依赖)
- directives ...
- filters ...
- mixins (组合)
- props (接口)
- data (本地状态属性)
- computed ...
- watch (响应回调)
- created (生命周期函数)
- mounted ...
- methods (实例属性)
```

## Vue Router Path 规范

router path 采用 kebab-case 格式。
::: tip
用下划线（如：/user_info）或 camelCase（如：/userInfo)的单词被当成一个单词，搜索引擎无法区分语义。
:::

```js
// bad
{
  path: '/user_info', // user_info当成一个单词
  name: 'UserInfo',
  component: UserInfo,
  meta: {
    title: ' - 用户',
    desc: ''
  }
},

// good
{
  path: '/user-info', // 能解析成user info
  name: 'UserInfo',
  component: UserInfo, // 确保和组件名一致
  meta: {
    title: ' - 用户',
    desc: ''
  }
},
```

## methods 命名规范

驼峰式命名，统一使用动词或者动词+名词形式

```js
//bad
go、nextPage、show、login、get_code

// good
jumpPage、openCarInfoDialog
```

请求数据方法，以 data 结尾

```js
//bad
takeData、confirmData、getList、postForm

// good
getListData、postFormData
```

::: tip
尽量使用常用单词开头（set、get、go、can、has、is）
:::

**附：** 函数方法常用的动词

```md
get 获取/set 设置,
add 增加/remove 删除
create 创建/destory 移除
start 启动/stop 停止
open 打开/close 关闭,
read 读取/write 写入
load 载入/save 保存,
create 创建/destroy 销毁
begin 开始/end 结束,
backup 备份/restore 恢复
import 导入/export 导出,
split 分割/merge 合并
inject 注入/extract 提取,
attach 附着/detach 脱离
bind 绑定/separate 分离,
view 查看/browse 浏览
edit 编辑/modify 修改,
select 选取/mark 标记
copy 复制/paste 粘贴,
undo 撤销/redo 重做
insert 插入/delete 移除,
add 加入/append 添加
clean 清理/clear 清除,
index 索引/sort 排序
find 查找/search 搜索,
increase 增加/decrease 减少
play 播放/pause 暂停,
launch 启动/run 运行
compile 编译/execute 执行,
debug 调试/trace 跟踪
observe 观察/listen 监听,
build 构建/publish 发布
input 输入/output 输出,
encode 编码/decode 解码
encrypt 加密/decrypt 解密,
compress 压缩/decompress 解压缩
pack 打包/unpack 解包,
parse 解析/emit 生成
connect 连接/disconnect 断开,
send 发送/receive 接收
download 下载/upload 上传,
refresh 刷新/synchronize 同步
update 更新/revert 复原,
lock 锁定/unlock 解锁
check out 签出/check in 签入,
submit 提交/commit 交付
push 推/pull 拉,
expand 展开/collapse 折叠
begin 起始/end 结束,
start 开始/finish 完成
enter 进入/exit 退出,
abort 放弃/quit 离开
obsolete 废弃/depreciate 废旧,
collect 收集/aggregate 聚集
```

## 多个属性的 html 元素规范

多个特性的元素，占据一行过长时，应该分多行撰写，每个特性一行。(增强更易读)

```html
<!-- bad -->
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<my-component foo="fooattribute" bar="barattribute" baz="bazattribute"></my-component>

<!-- good -->
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo">
<my-component
  foo="fooattribute"
  bar="barattribute"
  baz="bazattribute"/>
```

## 指令规范

- v-for 循环必须加上 key 属性，在整个 for 循环中 key 需要唯一
- 避免 v-if 和 v-for 同时用在一个元素上（性能问题）  
   出现这样的需求，有两种解决方案：

  1. 将数据替换为一个计算属性，让其返回过滤后的列表

  ```html
  <!-- bad -->
  <ul>
    <li v-for="user in users" v-if="user.isActive" :key="user.id">
      {{ user.name }}
    </li>
  </ul>

  <!-- good -->
  <ul>
    <li v-for="user in activeUsers" :key="user.id">
      {{ user.name }}
    </li>
  </ul>

  <script>
    computed: {
      activeUsers: function () {
        return this.users.filter(function (user) {
          return user.isActive
        })
      }
    }
  </script>
  ```

  2. 将 v-if 移动至容器元素上 (比如 ul, ol)

  ```html
  <!-- bad -->
  <ul>
    <li v-for="user in users" v-if="shouldShowUsers" :key="user.id">
      {{ user.name }}
    </li>
  </ul>

  <!-- good -->
  <ul v-if="shouldShowUsers">
    <li v-for="user in users" :key="user.id">
      {{ user.name }}
    </li>
  </ul>
  ```
