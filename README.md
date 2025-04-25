# TradingView 开发指南

获取 DEMO 代码，联系电报 @dev3699 获取

TradingView Charting Library 图标库，是一个独立的K线图表解决方案

可以自定义连接数据源，用于展示 A股、数字货币、外汇期货、美股等等交易标的的 K 线相关图表

图表库包可在GitHub上获得（必须获得授权才能访问GitHub上的这个私有资源）

申请地址： https://cn.tradingview.com/HTML5-stock-forex-bitcoin-charting-library/

个人无法申请，需要以公司名义来申请！

## 文档

- [官方文档](https://www.tradingview.com/charting-library-docs/latest/getting_started/)
- [TradingView 中文开发文档](https://zlq4863947.gitbook.io/tradingview/change-log)

英语好的小伙伴可以直接阅读官方文档

TradingView 中文开发文档版本相对较旧，很久未更新

## 实例

- [图表实例](https://charting-library.tradingview-widget.com/)
- [官方代码实例](https://github.com/tradingview/charting-library-examples)

## 图标库代码介绍

    +/charting_library
        + /bundles
        - charting_library.js
        - charting_library.d.ts
        - charting_library.cjs.js
        - charting_library.esm.js
        - charting_library.standalone.js
        - charting_library.js
        - datafeed-api.d.ts
        - package.json
    + /datafeeds
        + /udf
    - index.html
    - mobile_black.html
    - mobile_white.html
    - test.html

- /charting\_library 包含全部的图表库文件。
- /charting\_library/charting\_library.min.js 包含外部图表库 widget 接口。
- /charting_library/charting_library.min.d.ts 包含 TypeScript 定义的 widget 接口
- /charting_library/datafeed-api.d.ts 包含 TypeScript 定义的 datafeed 接口。
- /charting_library/datafeeds/udf/ 包含 UDF-compatible 的 datafeed 包装类（用于实现 JS API 通过 UDF 传输数据给图表库）
- html 文件为图表库使用示例

## Hello World 入门

将核心库(charting-library)下载到本地后，在项目根目录下执行

> python -m http.server 9090

在浏览器中打开 `http://127.0.0.1:9090/` 即可看到K线图表

## 图表使用

#### 1. 加载核心依赖库

```
<script type="text/javascript" src="charting_library/charting_library.standalone.js"></script>
<script type="text/javascript" src="datafeeds/udf/dist/bundle.js"></script>
```

如果是 VUE 加载组件，加载方式如下

```
import { widget } from '../../../js/charting_library';
```

#### 2. 实例化图表

```
var datafeedUrl = "https://demo-feed-data.tradingview.com";

new TradingView.widget({
    // debug: true, // uncomment this line to see Library errors and warnings in the console
    fullscreen: true,
    symbol: 'AAPL',
    interval: '1D',
    container: "tv_chart_container",

    //  BEWARE: no trailing slash is expected in feed URL
    datafeed: new Datafeeds.UDFCompatibleDatafeed(datafeedUrl, undefined, {
        maxResponseLength: 1000,
        expectedOrder: 'latestFirst',
    }),
    library_path: "charting_library/",

    disabled_features: ["use_localstorage_for_settings"],
    enabled_features: ["study_templates"],
    charts_storage_url: 'https://saveload.tradingview.com',
    charts_storage_api_version: "1.1",
    client_id: 'tradingview.com',
    user_id: 'public_user_id',
});

```

### 连接自定义数据源

连接数据源是最繁琐的步骤，因为图表库并不包含市场数据

官方提供了两种方式来连接自定义数据源，一种是 JS API、一种是 UDF

简单来说 JS API 是通过 Java Script 代码来实现指定的公共接口

需要在前端页面中去封装 `
searchSymbols` `getBars` `resolveSymbol` ... 等等 function，并处理好数据格式来返回给图表库来展示

使用 JS API 需要使用 Websocket

而 UDF 的方式，是使用 datafeedUrl api 来让后端向图表库提供数据
```
new Datafeeds.UDFCompatibleDatafeed(datafeedUrl, undefined, {
    maxResponseLength: 1000,
    expectedOrder: 'latestFirst',
}),
```

使用 http 协议去定时1000(1秒)轮询请求api，来获取数据

具体细节可以参考 [如何连接我的数据](https://zlq4863947.gitbook.io/tradingview/3-shu-ju-bang-ding/how-to-connect-my-data)