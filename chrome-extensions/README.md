## 本质

chrome extensions 本质上就是个网页（html + js + css）

## 基本组成

* manifest.json
* background script
* content script
* popup

### manifest.json

每一个chrome extensions都必须包含一个manifest.json，位于根路径。该文件描述了扩展的组成清单。

[完成清单参考链接](https://developer.chrome.com/extensions/manifest)

```json
{
  // 必选
  "manifest_version": 2,
  "name": "插件名称",
  "version": "x.x.x",
  // 可选（一般都需要）
  "default_locale": "en",
  "description": "插件描述",
  "icons": {
    "16": "icon_url", // 扩展程序页面上的图标
    "32": "icon_url", // 防止图标在Windows上失帧
    "48": "icon_url", // 扩展程序管理页面上的图标
    "128": "icon_url" // Chrome webstore中的图标
  },
  // 可选
  "background": {
    "page": "xxxx.html",
    "scripts": ["xxxx.js", "yyyy.js"],
    "persistent": false
  },
  "browser_action": {
    "default_icon": "url", // 工具栏中的图标
    "default_title": "悬浮在工具栏插件图标上时显示的tooltip",
    "default_popup": "xxx.html" // 不可内联js
  },
  // page_action 配置同 browser_action，二选一
  // 区别是 page_action 仅能作用与某些页面，比如 vue-devtool
  // 省略
  "content_scripts": [{
    "js": ["xxx.js"],
    "matches": ["http://*/*"],
    "run_at": "document_start"
  }],
  "permissions": [
    "contentMenus",
    "tabs",
    "xxx"
  ],
  "web_accessible_resources": ["dist/*"]
}
```

### background script

运行在浏览器中的一个后台脚本，与当前页面无关。需要在 manifest.json 中的 `background` 字段中注册。

#### page

相当于后台脚本的管理页面，可在此引入 js 脚本，可省略。

#### scripts

在 page 省略的情况下，可通过 scripts 引入 js 脚本。

#### persistent

后台脚本在 Chrome extensions 中分为两类：运行于后台页面（background page）和事件页面（event page）

background page: 持续运行，生命周期与浏览器相同，此时 persistent = true

event page: 只在需要活动时活动，在完全不活动的情况下Chrome会释放其占用的资源。在再次有事件需要后台脚本处理时将重新载入，此时 persistent = false

> 保持后台脚本持久活动的唯一场合是使用 chrome.webRequest API来阻止或修改网络请求。

### content script

插入到网页中的脚本。有两个特点：独立，共享。

独立：工作空间与命名空间独立，与被插入插件的页面不共享任何函数或变量；

共享：插件将一些脚本插入到符合条件的页面中，因此dom是共享的。这一特点主要作用是允许通过 postMessage 进行消息通信。

#### run_at

指定脚本何时插入到页面，有三个选项：

* document_start: style 样式加载完毕，dom 渲染完成和脚本执行前
* document_end: dom 渲染完成后，DOMContentLoaded 后马上执行
* document_idle: 在 DOMContentLoaded 和 window load 之间，具体时刻取决于页面的复杂程度和加载时间

## 通信机制

通信主要有以下几种情景：

1. content script 与 background 的通信
2. popup 与 background 的通信
3. popup 与 background 的通信
4. 插件 iframe 网站与插入网页的通信（有些插件通过iframe的形式呈现）

### conentent script 与 background 的通信

#### content script 向 background 发送消息

```javascript
// 在 content script中
chrome.runtime.sendMessage(
  message,
  function (resp) {
    // resp 为 background 接受消息后返回的信息
  }
)

// 在 background 中
chrome.runtime.onMessage.addListener(
  function (request, sender, sendResponse) {
    // request: 接受到的消息
    // sendResponse: 响应的回调函数，若不执行，则默认返回 undefined
    // sendResponse 一般为同步执行
  }
)
```

background 同时监听所有页面上的 content script 发送的消息，若有多个页面同时发送了同种消息，则只会处理最先收到的那个

#### background 向 content script 发送消息

1. 在background中

首先需要知道向哪个 conent script 发送消息。假设一个页面一份 content script，而一个页面对应浏览器的一个tab，每个tab有自己的tabId，因此此时获取tabId即可:

```javascript
function getCurrentTabId(callback) {
  chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
    callback && callback(tabs.length ? tabs[0].id : null)
  })
}
```

得到 tabId 后即可发送消息:

```javascript
chrome.tabs.sendMessage(tabId, message, function(response) {
})
// response 为content script响应后传回的内容
```

2. 在 content script 中

```javascript
chrome.runtime.onMessage.addListener(function(request, sender, sendResponse) {})
```

### popup 与 background 的通信

可以利用 chrome.runtime.onMessage 和 chrome.runtime.sendMessage，或者:

```javascript
const bg = chrome.extension.getBackgroundPage()
// 在调用 bg 上的某些方法
```

### popup 与 content script 的通信

同 background 与 content script 的通信

### 插件iframe 与 被插入插件的网页 的通信

同 iframe 与 父窗体 的通信
