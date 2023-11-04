# Next.js（今天被折磨了么

## Routing

### Loading UI and Streaming

The special file `loading.js` helps you create meaningful Loading UI with [React Suspense](https://react.dev/reference/react/Suspense).

在组件加载时显示 `loading.js` 加载页面。

![loading.js special file](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Floading-special-file.png&w=3840&q=75&dpl=dpl_6gnezwfspXcNrWr6SUrFhAXCtq3z)

```typescript
export default function Loading() {
  // You can add any UI inside Loading, including a Skeleton.
  return <LoadingSkeleton />
}
```

`loading.js` 将会被嵌套于 `layout.js` 中，且会自动包裹 `page.js` 在  `<Suspense> ` 中

![loading.js overview](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Floading-overview.png&w=3840&q=75&dpl=dpl_6gnezwfspXcNrWr6SUrFhAXCtq3z)

+ 导航是可中断的，意味着改变路由不需要等当前页面加载完成

#### [Streaming with Suspense](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#streaming-with-suspense)

#### [What is Streaming?](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#what-is-streaming)

#### Server-Side Rendering(SSR)

在用户看到页面前，有 5 个步骤

1. 在服务器端获取到需要展示的数据

2. 服务器端渲染 HTML

3. 发送 HTML, CSS, Javascript 到客户端

4. 使用 HTML, CSS 渲染非交互页面

5. Finally, React [hydrates](https://react.dev/reference/react-dom/client/hydrateRoot#hydrating-server-rendered-html) the user interface to make it interactive.

   使可交互

SSR with React and Next.js helps improve the perceived loading performance by showing a non-interactive page to the user as soon as possible.

However, it can still be slow as all data fetching on server needs to be completed before the page can be shown to the user.

**Streaming** allows you to break down the page's HTML into smaller chunks and progressively send those chunks from the server to the client.

拆分成组件渲染，避免等待数据延缓部分组件的渲染，优化渲染效率

Streaming works well with React's component model because each component can be considered a chunk. 

![Chart showing Server Rendering with Streaming](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming-chart.png&w=3840&q=75&dpl=dpl_6gnezwfspXcNrWr6SUrFhAXCtq3z)

Streaming 在处理长数据的渲染时有奇效。 It can reduce the [Time To First Byte (TTFB)](https://web.dev/ttfb/) and [First Contentful Paint (FCP)](https://web.dev/first-contentful-paint/). It also helps improve [Time to Interactive (TTI)](https://developer.chrome.com/en/docs/lighthouse/performance/interactive/), especially on slower devices.

#### Example

用 `<Suspense>` 包裹异步组件，在加载时显示fallback UI。

```typescript
import { Suspense } from 'react'
import { PostFeed, Weather } from './Components'
 
export default function Posts() {
  return (
    <section>
      <Suspense fallback={<p>Loading feed...</p>}>
        <PostFeed />
      </Suspense>
      <Suspense fallback={<p>Loading weather...</p>}>
        <Weather />
      </Suspense>
    </section>
  )
}
```

使用 `Suspense` 的好处：

1. SSR
2. 选择性渲染组块

For more Suspense examples and use cases, please see the [React Documentation](https://react.dev/reference/react/Suspense).

#### SEO

Next.js 会等待 [`generateMetadata`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata) 中数据获取完成后再向客户端传递 streaming UI ，这确保了第一部分的 streamed response 包含 `<head>` 的 tag。

Since streaming is server-rendered, it does not impact SEO. You can use the [Mobile Friendly Test](https://search.google.com/test/mobile-friendly) tool from Google to see how your page appears to Google's web crawlers and view the serialized HTML ([source](https://web.dev/rendering-on-the-web/#seo-considerations)).

#### [Status Codes](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#status-codes)

When streaming, a status code will be returned to signal that the request was successful.`200`

The server can still communicate errors or issues to the client within the streamed content itself, for example, when using [`redirect`](https://nextjs.org/docs/app/api-reference/functions/redirect) or [`notFound`](https://nextjs.org/docs/app/api-reference/functions/not-found). Since the response headers have already been sent to the client, the status code of the response cannot be updated. This does not affect SEO.

### Error Handling

The `error.js` 可以帮助你优雅地处理运行时出现的错误。

+ 自动包裹路由段和嵌套的子组件
+ Create error UI tailored to specific segments using the file-system hierarchy to adjust granularity.
+ 将 error 隔离，保持其余部分功能正常运行
+ 可以添加功能尝试修复 error

```typescript
'use client' // Error components must be Client Components
 
import { useEffect } from 'react'
 
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error)
  }, [error])
 
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  )
}
```

### [How Works`error.js`](https://nextjs.org/docs/app/building-your-application/routing/error-handling#how-errorjs-works)

error.js 会自动创建一个包含嵌套子组件或 `page.js` 的 [React Error Boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

从 `error.js` 导出的组件会被作为 fallback component 使用

error 更新，组件将会重新渲染

When the fallback error component is active, layouts **above** the error boundary **maintain** their state and **remain** interactive, and the error component can display functionality to recover from the error.

### [Recovering From Errors](https://nextjs.org/docs/app/building-your-application/routing/error-handling#recovering-from-errors)

The cause of an error can sometimes be temporary. In these cases, simply trying again might resolve the issue.

An error component can use the function to prompt the user to attempt to recover from the error. When executed, the function will try to re-render the Error boundary's contents. If successful, the fallback error component is replaced with the result of the re-render.`reset()`

```typescript
'use client'
 
export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  )
}
```

#### [Nested Routes](https://nextjs.org/docs/app/building-your-application/routing/error-handling#nested-routes)

