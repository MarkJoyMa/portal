---
title: 路由规则
slug: /docs/tutorials/api/route/rule
---

## 概述

在 go-zero 中，我们通过 api 语言来声明 HTTP 服务，然后通过 goctl 生成 HTTP 服务代码，在之前我们系统性的介绍了 <a href="/docs/tutorials" target="_blank">API 规范</a>。

在 api 描述语言中，有特定的路由规则，这些路由规则并非和 HTTP 的路由规则完全引用，接下来我们来看一下 api 描述语言中的路由规则吧。

## 路由规则

在 api 描述语言中，我们需要满足如下正则表达式的路由规则（Golang）：

```go
(?m)(/:{0,1}[\w-]+)+
```

规则细分：

1. 路由规则必须以 `/` 开头
1. 路由节点必须以 `/` 分隔
1. 路由节点中可以包含 `:`，但是 `:` 必须是路由节点的第一个字符，`:` 后面的节点值必须要在结请求体中有 `path` tag 声明，用于接收路由参数，详细规则可参考 <a href="/docs/tutorials/api/parameter" target="_blank">路由参数</a>。
1. 路由节点可以包含字母、数字、下划线、中划线

路由示例：

```go {27,31,35,39,43,47,51}
type DemoPath3Req {
    Id int64 `path:"id"`
}

type DemoPath4Req {
    Id   int64  `path:"id"`
    Name string `path:"name"`
}

type DemoPath5Req {
    Id   int64  `path:"id"`
    Name string `path:"name"`
    Age  int    `path:"age"`
}

type DemoReq {

}

type DemoResp {

}

service Demo {
    // 示例路由 /foo
    @handler demoPath1
    get /foo (DemoReq) returns (DemoResp)

    // 示例路由 /foo/bar
    @handler demoPath2
    get /foo/bar (DemoReq) returns (DemoResp)

    // 示例路由 /foo/bar/:id，其中 id 为请求体中的字段
    @handler demoPath3
    get /foo/bar/:id (DemoPath3Req) returns (DemoResp)

    // 示例路由 /foo/bar/:id/:name，其中 id，name 为请求体中的字段
    @handler demoPath4
    get /foo/bar/:id/:name (DemoPath4Req) returns (DemoResp)

    // 示例路由 /foo/bar/:id/:name/:age，其中 id，name，age 为请求体中的字段
    @handler demoPath5
    get /foo/bar/:id/:name/:age (DemoPath5Req) returns (DemoResp)

    // 示例路由 /foo/bar/baz-qux
    @handler demoPath6
    get /foo/bar/baz-qux (DemoReq) returns (DemoResp)

    // 示例路由 /foo/bar_baz/123
    @handler demoPath7
    get /foo/bar_baz/123 (DemoReq) returns (DemoResp)
}
```
