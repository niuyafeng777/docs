# 客户端 API

客户端 API 可以通过 `vuepress/client` 来引入。

## 组合式 API

### useClientData

- 详情：

  返回所有客户端数据的 Ref 对象。

  每个属性也可以通过下列的组合式 API 来访问。

- 示例：

```vue
<script setup lang="ts">
import { useClientData } from 'vuepress/client'

const {
  pageData,
  pageFrontmatter,
  pageHead,
  pageHeadTitle,
  pageLang,
  routeLocale,
  siteData,
  siteLocaleData,
} = useClientData()
</script>
```

### usePageData

- 详情：

  返回当前页面数据的 Ref 对象。

- 参考：
  - [Node API > Page 属性 > data](./node-api.md#data)
  - [插件 API > extendsPage](./plugin-api.md#extendspage)

### usePageFrontmatter

- 详情：

  返回当前页面 Frontmatter 的 Ref 对象。

  它的值是页面数据的 `frontmatter` 属性。

### usePageHead

- 详情：

  返回当前页面 Head 配置的 Ref 对象。

  它的值是合并 [head](./frontmatter.md#head) Frontmatter 和 [head](./config.md#head) 配置，并进行去重后得到的。

### usePageHeadTitle

- 详情：

  返回当前页面 Head 中的标题的 Ref 对象。

  它的值是连接页面标题和站点标题后得到的。

### usePageLang

- 详情：

  返回当前页面语言的 Ref 对象。

  它的值是页面数据的 `lang` 属性。

### useRoutes

- 详情：

  返回所有路由的 Ref 对象。

  它的值是站点数据的路由信息。

- 参考：
  - [深入 > Cookbook > 解析路由](../advanced/cookbook/resolving-routes.md)

### useRouteLocale

- 详情：

  返回当前路由对应的 locale path 的 Ref 对象。

  它的值是 [locales](./config.md#locales) 配置的键之一。

### useSiteData

- 详情：

  返回站点数据的 Ref 对象。

### useSiteLocaleData

- 详情：

  返回当前 locale 的站点数据的 Ref 对象。

  当前 locale 中的配置已经合并到顶层配置中。

### onContentUpdated

- 详情：

  当 markdown 文件内容发生变化时，触发回调。

  该函数仅能在组件的 `setup` 阶段被调用。

  ```vue
  <script setup>
  import { onContentUpdated } from 'vuepress/client'

  onContentUpdated((reason) => {
    console.log(`content updated reason: ${reason}`)
  })
  </script>
  ```

## 工具函数

### defineClientConfig

- 详情：

  帮助你创建 [clientConfigFile](./plugin-api.md#clientconfigfile) 的工具函数。

- 参考：
  - [深入 > Cookbook > 客户端配置的使用方法](../advanced/cookbook/usage-of-client-config.md)

### resolveRoute

- 详情：

  解析给定链接对应的路由

- 参考：
  - [深入 > Cookbook > 解析路由](../advanced/cookbook/resolving-routes.md)

### resolveRoutePath

- 详情：

  解析给定链接对应的路由路径

- 参考：
  - [深入 > Cookbook > 解析路由](../advanced/cookbook/resolving-routes.md)

### withBase

- 详情：

  在 URL 前添加站点 [base](./config.md#base) 前缀。

- 参考：
  - [指南 > 静态资源 > Base Helper](../guide/assets.md#base-helper)

## 常量

在客户端代码中有一些常量可以使用。

如果想要把这些常量的类型定义补充到你的代码环境中，请将 `vuepress/client-types` 添加到你的 `tsconfig.json` 里：

```json
{
  "compilerOptions": {
    "types": ["vuepress/client-types"]
  }
}
```

### `__VUEPRESS_VERSION__`

- 类型： `string`

- 详情：

  VuePress Core 的版本号。

### `__VUEPRESS_BASE__`

- 类型： `string`

- 详情：

  配置中的 [base](./config.md#base) 字段。

### `__VUEPRESS_DEV__`

- 类型： `boolean`

- 详情：

  一个环境标记，用于标识当前是否运行在 `dev` 模式下。

### `__VUEPRESS_SSR__`

- 类型： `boolean`

- 详情：

  一个环境标记，用于标识当前是否运行在服务端渲染 (SSR) 环境下。

## 进阶能力

### resolvers <Badge text="实验性能力" />

- 类型： `Record<string, Function>`

- 详情：

  一个响应式对象，其中的方法决定了如何获取全局计算属性。

- 示例：

在客户端配置文件中自定义 `<title>` 的格式：

```ts
import { defineClientConfig, resolvers } from 'vuepress/client'

export default defineClientConfig({
  enhance({ app, router, siteData }) {
    resolvers.resolvePageHeadTitle = (page, siteLocale) =>
      `${siteLocale.title} > ${page.title}`
  },
})
```

::: danger
`resolvers` 会直接影响 VuePress 的基础功能，在修改前请确保你已充分了解其用途。
:::
