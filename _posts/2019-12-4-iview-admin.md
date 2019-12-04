---
layout:     post
title:    iview-admin
subtitle:  
date:       2019-12-4
author:     
header-img: 
catalog: true
tags:
    - < 实习 >
typora-root-url: ..


---



# iview-admin-- 基于Vue的后台管理系统 

官方文档 https://lison16.github.io/iview-admin-doc/#/ 

## 一.目录结构

```
├── config  开发相关配置
├── public  打包所需静态资源
└── src
    ├── api  AJAX请求
    └── assets  项目静态资源
        ├── icons  自定义图标资源
        └── images  图片资源
    ├── components  业务组件
    ├── config  项目运行配置
    ├── directive  自定义指令
    ├── libs  封装工具函数
    ├── locale  多语言文件
    ├── mock  mock模拟数据
    ├── router  路由配置
    ├── store  Vuex配置
    ├── view  页面文件
    └── tests  测试相关
```

## 二.路由配置

```JavaScript
meta: {
  hideInMenu: (default: false) 设为true后在左侧菜单不会显示该页面选项
  showAlways: (default: false) 设为true后如果该路由只有一个子路由，在菜单中也会显示该父级菜单
  notCache: (default: false) 设为true后页面不会缓存
  access: (default: null) 可访问该页面的权限数组，当前路由设置的权限会影响子路由
  icon: (default: -) 该页面在左侧菜单、面包屑和标签导航处显示的图标，如果是自定义图标，需要在图标名称前加下划线'_'
  href: 'https://xxx' (default: null) 用于跳转到外部连接
}
```

## 三.权限控制

#### 1.整个页面访问权限

在路由的meta中添加access字段

```JavaScript
{
  path: '/page1',
  name: 'page1',
  component: Main,
  meta: {
    access: ['super_admin'] /*
                             * 该页面只有权限值为super_admin的用户才能访问
                             * 如果这级路由有子路由，则子路由也只有super_admin才能访问
                             * 如果不设置此字段，则所有用户均可访问
                             */
  }
}
```

#### 2.单个路由组件浏览控制

取access值做判断，控制组件是否显示

```JavaScript
<template>
  <div>
    <component1 v-if="viewAccessAll"></component1>
    <component2 v-if="viewAccessSuper"></component2>
  </div>
</template>
<script>
import { hasOneOf } from '@/libs/tools'
export default {
  name: 'page',
  computed: {
    access () {
      return this.$store.state.user.access
    },
    viewAccessAll () {
      return hasOneOf(['super_admin', 'admin'], this.access)
    },
    viewAccessSuper () {
      return hasOneOf(['super_admin'], this.access)
    }
  }
}
</script>
```

#### 3.axios

##### api中

```JavaScript
import axios from '@/libs/api.request'

export const getTableData = () => {
  return axios.request({  
    // 这里返回的是一个Promise，request方法传入一个配置对象，配置项可参考axios
    url: 'get_table_data',
    method: 'get'
  })
}
```

##### 使用

```JavaScript
import { getTableData } from '@/api/data'

getTableData().then(res => {
  this.tableData = res.data
}).catch(err => {
  console.log(err)
})
```

## 四.登录部分

### 1.token登陆验证

##### 基于 Cookie/Session 的认证方案

-  Cookie 
   `cookie`指的就是在浏览器里面存储的一种数据，仅仅是浏览器实现的一种数据存储功能。

   `cookie`的保存时间，可以自己在程序中设置。如果没有设置保存时间，应该是一关闭浏览器，`cookie`就自动消失。 

-  Session 

   `Session`是另一种记录客户状态的机制，**不同的是**`Cookie`保存在客户端浏览器中，而`Session`保存在服务器上。 

- token

  ```
  1.前端使用用户名跟密码请求首次登录
  
  2.后服务端收到请求，去验证用户名与密码是否正确
  
  3.验证成功后，服务端会根据用户`id`、用户名、定义好的秘钥、过期时间生成一个 `Token`，再把这个 `Token` 发送给前端
  
  4.前端收到 返回的`Token` ，把它存储起来，比如放在 `Cookie` 里或者 `Local Storage` 里
  
  5.前端每次路由跳转，判断 `localStroage` 有无 `token` ，没有则跳转到登录页。有则请求获取用户信息，改变登录状态；
  
  6.前端每次向服务端请求资源的时候需要在**请求头**里携带服务端签发的`Token`
  
  7.服务端收到请求，然后去验证前端请求里面带着的 `Token`。没有或者 `token` 过期，返回`401`。如果验证成功，就向前端返回请求的数据。
  
  8.前端得到 `401` 状态码，重定向到登录页面。
  ```

  然后在全局钩子里判断是否存在token进行路由拦截

2.路由拦截

3.模拟数据

## 五.home

