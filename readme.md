# 性能优化

## 首屏加载优化

### 思路

> 首屏加载慢的原因？
- 网络层面：网络延迟
- 渲染层面：资源太大， 3M js

1、网络延迟
- cdn 用户节点就近
- preload 预加载
- prerender 预渲染

2、资源太大
- 分包 chunk
- 静态资源懒加载
- 提取公共资源 CSS
- 缓存方面（强制缓存 协商缓存 策略缓存（service-worker））
- 服务端渲染 
- 局部 SSR（落地页、广告页、营销活动页）


### 衡量指标
- FP（First Paint）首次绘制
- FCP （First Contentful Paint）首次内容渲染
> 可借助浏览器提供的 API（Performance） 计算 

- FMP （First Meaning Paint）
- LCP （Largest Contentful Paint）最大内容渲染
> FMP 一般通过 Mutation Observer（监听 DOM 变化）


> 这些性能监听或者上报的代码，我们通常只会写一次，web-tracker
- 性能采集
  - Performance
  - Mutation Observer
- 用户行为采集
  - 无痕埋点
  - 手动埋点
  - 可视化埋点
- 异常采集
  - react ErrorBondary
  - 异常捕获 window.onError

SSR 可交互时间（TTI）
- TBT（Total Blocking Time） 
- TTI（从 FCP -> 用户可交互，中间的时间）


### 优化基础细节
- 1、优化图片:推荐 WebP 格式，不需要太大的图片（头像上传控制 size 200*200）
- 2、组件按需加载：React Suspense + React.lazy + import
  - 分析工具：vite-bundle-analyze
- 3、延迟加载：滚动加载 可视区内容加载 虚拟列表
- 4、tree-shaking：通过代码编写去约定，tree-shaking需要的前提「ESM」
- 5、CDN：oss + cdn
- 6、精简第三方库：
  - 同类别的库足够精简，
  - 库要支持「按需导入」babel-plugin-import（antd）
  - 针对一些国际化的文件，不必要的语言要移除
- 7、HTTP 缓存
- 8、字体压缩
  - font-spider 移除无用字体
  - webfont 处理字体加载
- 9、SSR（server side render） 、SSG（server side generate）
  - vue Nust.js
  - react Next.js


### 具体实现进阶

- 预加载 preload

```html
<link rel = 'preload' href='xxx.js' as='script'>
```

- 预渲染 prerender（webpack plugin ——prerender）

- 加载关键 CSS
  - webpack 插件：miniCssExtraPlugin（提取CSS到单独文件）
  - webpack 插件：webpack-prerender-plugin（预渲染）

- 开启 HTTP/2 Server Push

- 脚本延迟加载 defer async

