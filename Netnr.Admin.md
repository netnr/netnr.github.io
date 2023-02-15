### 权限设计
- 用户对应多个角色，每个角色配置对应的权限
- 角色配置菜单和菜单对应按钮，此为页面权限
- 角色配置模块和接口，此为接口权限
- 数据权限未做通配，可分多个方法或在方法中过滤来实现

---

### 系统模块

#### 快表管理
建表、建列、搞定，单表的简单应用

表操作（新增、修改、删除）
```
[
  {
    "method": "POST",
    "path": "tableName",
    "body": {
      "field1": "a",
      "field2": 2
    }
  },
  {
    "method": "PUT",
    "path": "tableName/dataId",
    "body": {
      "field1": "b",
      "field2": 22
    }
  },
  {
    "method": "DELETE",
    "path": "tableName/dataId"
  }
]
```

#### 运维监测
以主机 HOST 为首，关联 HTTP SSL Port Database 等监测项，加入任务调度，配置报警规则及联系人

---

### 后端
略

### 前端

#### 依赖组件
- @shoelace-style/shoelace 常见组件包
- page 路由
- ag-grid 表格
- ag-charts 图表
- bootstrap 常用、辅助样式、栅格、布局等

#### 项目结构
- assets 资源路径，静态文件
- css 样式目录
- js/index.js 入口
- js/nrChart.js 图表
- js/nrFunction.js 方法
- js/nrGlobal.js 全局
- js/nrGrid.js 表格
- js/nrStorage.js 存储
- js/nrVary.js 变量
- js/page/* 页面
- js/page/shared 公共页面（首页、错误、登录页面）
- js/pack/* 页面分模块打包

#### 命名规则
- nrPage.domXXX 节点对象
- nrPage.gridXXX 表格对象
- nrPage.chartXXX 图表对象
- nrPage.viewXXX 显示、弹出方法
- nrPage.buildXXX 构建方法
- nrPage.bindXXX 绑定事件、数据方法
- nrPage.getXXX 获取方法
- nrPage.reqXXX 请求接口方法
- nrPage.refXXX 请求字典接口方法
- nrPage.event_XXX 事件全局回调方法

刷新、关闭页面（选项卡）清理对象前缀配置 `nrVary.flagTabClearPrefix`

#### 开发事项
- 使用异步 async
- 依赖包加载方法为 `nrGlobal.refPackage`，默认暴露给 window 对象
- 页面脚本加载方法为 `nrGlobal.getPageScript`
- 模块名称以小写、下划线固定格式，如 fast_table
- 设置节点 `data-href` 属性打开链接
- 设置节点 `data-action` 属性触发行为，如切换主题，需在 `nrGlobal.bindAction` 添加响应
- 全局样式前缀 `nrg-` 如 `<div class="nrg-card-one"></div>` 可从 `nrVary.domCardOne` 获取对象
- 页面样式前缀 `nrp-` 需要手动调用 `nrGlobal.buildObjNode(nrVary.domPanel, "nrp", nrPage)`
- 样式格式建议 `nrp-{module}-{class}` 如：`nrp-index-card-one` 表示首页模块的一个卡片
- 表格 Grid 列字段以 `$` 开头为构建变量，以 `#` 开头为虚拟列

#### 页面开发
```js
let nrPage = {
    // 设置路径，唯一，路由匹配则运行该脚本，调用方法 render()
    // admin 表示模块名称 且须放置 page/admin 目录下，模块名称使用小写、下划线
    pathname: '/admin/log',
    title: "可选，标题，默认从菜单提取",
    icon: "可选，图标，默认从菜单提取",

    //（必须）渲染，nrVary.domPanel 指向页面面板，默认刷新或重新打开会清空面板
    render: async () => {
        nrVary.domPanel.innerHTML = `<div class="nrp-card">Hello World</div>`;
        
        // 构建节点对象（在面板渲染后）
        nrGlobal.buildObjNode(nrVary.domPanel, "nrp", nrPage);
        
        //定时任务
        var tn1 = setInterval(() => {
            nrPage.domCard.innerHTML = new Date().toISOString();
        }, 1000);
        //加入标记，用于清理选项卡时清理任务
        nrGlobal.pageTiming(nrPage.pathname, tn1);
    },

    /*（可选）变更主题*/
    event_theme: (theme) => {
        console.debug(theme)
    },
    /*（可选）窗口大小*/
    event_resize: () => {
        console.debug('resize')
    },
}

export { nrPage };
```