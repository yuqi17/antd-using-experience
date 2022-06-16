js 部分:
```jsx
import axios from "axios";

import React, { Component } from "react";

import ReactDOM from "react-dom";

import { message, Spin } from "antd";

const Axios = axios.create({
  // baseURL: process.env.BASE_URL, // 设置请求的base url
  timeout: 20000, // 设置超时时长
});

// 设置post请求头
Axios.defaults.headers.post["Content-Type"] =
  "application/x-www-form-urlencoded;charset=UTF-8";

// 当前正在请求的数量
let requestCount = 0;

// 显示loading
function showLoading() {
  if (requestCount === 0) {
    console.log('hello')
    var dom = document.createElement("div");
    dom.setAttribute("id", "loading");
    document.body.appendChild(dom);
    ReactDOM.render(<Spin tip="加载中..." size="large" />, dom);
  }
  requestCount++;
}

// 隐藏loading
function hideLoading() {
  requestCount--;
  if (requestCount === 0) {
    document.body.removeChild(document.getElementById("loading"));
  }
}

// 请求前拦截
Axios.interceptors.request.use(
  (config) => {
    // requestCount为0，才创建loading, 避免重复创建
    if (config.headers.isLoading !== false) {
      showLoading();
    }
    return config;
  },
  (err) => {
    // 判断当前请求是否设置了不显示Loading
    if (err.config.headers.isLoading !== false) {
      hideLoading();
    }
    return Promise.reject(err);
  }
);

// 返回后拦截
Axios.interceptors.response.use(
  (res) => {
    // 判断当前请求是否设置了不显示Loading

    if (res.config.headers.isLoading !== false) {
      hideLoading();
    }
    return res;
  },
  (err) => {
    if (err.config.headers.isLoading !== false) {
      hideLoading();
    }

    if (err.message === "Network Error") {
      message.warning("网络连接异常！");
    }

    if (err.code === "ECONNABORTED") {
      message.warning("请求超时，请重试");
    }

    return Promise.reject(err);
  }
);

// 把组件引入，并定义成原型属性方便使用  这种写法有点Vue 的味道,只能用于类组件
Component.prototype.$axios = Axios;

export default Axios;

```

loading 层 css 样式:
```css
#loading {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.75);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
    font-size: 20px;
}

```

使用: 要么用 Promise.all, 要么用 promise.then 如果多个请求都用 aysnc/await 则仍然会出现多次loading; 如果多个请求之间有强的依赖顺序,则最好手动去控制loading
```js
import React, { useEffect } from 'react';
import axios from './request';

const Page = () => {
  const query = async ()=>{
    const [{data:{data:a}}, {data:{data:b}}] = await Promise.all([
      axios.get('https://demo-api.apipost.cn/api/demo/news_list?mobile=18289454846&theme_news=国际新闻&page=1&pageSize=20'),
      axios.get('https://demo-api.apipost.cn/api/demo/news_details?id=20&status=1')
    ])
   
    console.log(a, b)
  }
  useEffect(() => {
  //  query()
      axios.get('https://demo-api.apipost.cn/api/demo/news_list?mobile=18289454846&theme_news=国际新闻&page=1&pageSize=20').then(console.log)
      axios.get('https://demo-api.apipost.cn/api/demo/news_details?id=20&status=1').then(console.log)
  }, []);
  return <div>click me</div>
};

export default Page;

```
