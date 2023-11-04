# Key Concept

## Auto-imports

可以自动导入组件、自定义函数以及 `Vue.js` 的 APIs 。

可以自动导入  [`components/`](https://nuxt.com/docs/guide/directory-structure/components), [`composables/`](https://nuxt.com/docs/guide/directory-structure/components) and [`utils/`](https://nuxt.com/docs/guide/directory-structure/components) 中的文件。

### components

#### 引用

+ 默认以 `components/` 路径下的路径并以驼峰命名法命名加以引用。

+ 如果只想用组件的名称进行引用，将 `pathPrefix` 的选项定义为 false

  ```javascript
  export default defineNuxtConfig({
    components: [
      {
        path: '~/components',
  +     pathPrefix: false,
      },
    ],
  });
  ```

#### 动态组件

+ 如果你想使用 `Vue` 中的 `<component :is="someComputedComponent">` 的写法，那么这个组件需要被你手动导入，或者使用 `Vue` 提供的 `resolveComponent` 

+ for example:

  ```typescript
  <script setup lang="ts">
  import { SomeComponent } from '#components'
  
  const MyButton = resolveComponent('MyButton')
  </script>
  
  <template>
    <component :is="clickable ? MyButton : 'div'" />
    <component :is="SomeComponent" />
  </template>
  
  ```

  ```
  当你使用 resolveComponent 来实现动态组件时，请确认没有注入除组件名字外的其它字符串，且不能时一个变量。
  ```

+ 另外，即使我们不推荐这么做，但你也可以选择全局注册你的组件。这将创建为你的组件创建异步的组块并使得它们全局可用。

  ```typescript
    export default defineNuxtConfig({
      components: {
  +     global: true,
  +     dirs: ['~/components']
      },
    })
  
  ```

+ 你也可以选择性地注册一些全局组件通过把它们放在 `~/component/global` 的目录中。

#### 动态引入

如果你想**动态导入组件**（或者称其为**懒加载**），那么只需要在引用组件的名字前加上一个`lazy` 。当你的组件并非常驻时相当有用。

通过动态导入组件，你的组件可以延缓加载直到需要它，这可以有效优化 Javascript 包的内存。

```vue
<script setup>
const show = ref(false)
</script>

<template>
  <div>
    <h1>Mountains</h1>
    <LazyMountainsList v-if="show" />
    <button v-if="!show" @click="show = true">Show List</button>
  </div>
</template>

```

#### 直接导入

如果你想要或需要避免 Nuxt 的自动导入功能，你也可以从`#components`明确导入需要的组件。

```vue
<script setup lang="ts">
import { NuxtLink, LazyMountainsList } from '#components'

const show = ref(false)
</script>

<template>
  <div>
    <h1>Mountains</h1>
    <LazyMountainsList v-if="show" />
    <button v-if="!show" @click="show = true">Show List</button>
    <NuxtLink to="/">Home</NuxtLink>
  </div>
</template>

```

#### 默认地址

默认情况下，只有`~/components`地址下的组件会被检索。如果你想加入其它需要检索的地址，或者更改在此目录的子文件夹中扫描组件的方式，则可以将其他目录添加到配置`nuxt.config.ts`中。

```typescript
export default defineNuxtConfig({
  components: [
    // ~/calendar-module/components/event/Update.vue => <EventUpdate />
    { path: '~/calendar-module/components' },

    // ~/user-module/components/account/UserDeleteDialog.vue => <UserDeleteDialog />
    { path: '~/user-module/components', pathPrefix: false },

    // ~/components/special-components/Btn.vue => <SpecialBtn />
    { path: '~/components/special-components', prefix: 'Special' },

    // It's important that this comes last if you have overrides you wish to apply
    // to sub-directories of `~/components`.
    //
    // ~/components/Btn.vue => <Btn />
    // ~/components/base/Btn.vue => <BaseBtn />
    '~/components'
  ]
})

```

#### npm Packages

如果你想自动导入npm packages中的组件，你可以使用在本地的**module**中使用`addComponent`来注册。

~/module/register-component.ts

```typescript
import { addComponent, defineNuxtModule } from '@nuxt/kit'

export default defineNuxtModule({
  setup() {
    // import { MyComponent as MyAutoImportedComponent } from 'my-npm-package'
    addComponent({
      name: 'MyAutoImportedComponent',
      export: 'MyComponent',
      filePath: 'my-npm-package',
    })
  },
})

```

```vue
<template>
  <div>
    <!--  the component uses the name we specified and is auto-imported  -->
    <MyAutoImportedComponent />
  </div>
</template>

```

+ Any nested directories need to be added first as they are scanned in order.

