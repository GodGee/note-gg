### Vue3 - setup script

## 前言

Vue3已经发布很长一段时间了，相信大多数前端人都已经上手把玩过了，其中比较大的一个特性就是`setup`方法，可以让我们非常直观和方便的组合我们的业务逻辑和hooks。在`setup`里面返回的变量可以直接在`template`里面使用。大多数情况下，我们的大部分逻辑都集中在`setup`方法里面，所以官方提供了一个**实验性的**写法，直接在`script`里面写`setup`的内容，即：`setup script`。

## 使用

我们之前的组件可能是这样的：

```js
<template>
  <div class="flex items-center justify-center h-screen bg-gray-50">
    <Card>{{msg}}</Card>
  </div>
</template>
<script lang="ts">
import { ref, defineComponent } from "vue";
import Card from "./components/Card.vue";

export default defineComponent({
  components: {
    Card,
  },
  setup() {
    const msg = ref("setup script");
    return { msg };
  }
});
</script>
```

这里做了两件事，一个是导入并注册组件，一个是导出一个字符串给`template`使用。

启用`setup script`之后是这样的：

```js
<template>
  <div class="flex items-center justify-center h-screen bg-gray-50">
    <Card>{{msg}}</Card>
  </div>
</template>


<script lang="ts" setup>
import { ref } from "vue";
import Card from "./components/Card.vue";
const msg = ref("setup script");
</script>
```

这里省去了组件的注册步骤，也没有显式的导出变量的动作。

虽然是实验性功能，但还是开箱即用，你只需要在`script`上配置`setup`即可。

### 使用setup中的参数

```XML
<script setup="props, context" lang="ts">
 
<script>
```

像这样，只要在setup处声明即可自动导入，同时也支持解构语法：

```XML
<script setup="props, { emit }" lang="ts">
 
<script>
```

### 导出变量&方法

在`setup script`里面定义的所有变量都会自动导出。非常方便

```js
<script lang="ts" setup>
import { ref } from "vue";
const msg = ref("setup script");
const handlerClick = () => {
  console.log("on click");
};
</script>
```

### 使用组件

所有的组件导入即可自动注册：

```js
<script lang="ts" setup>
import Card from "./components/Card.vue";
import Button from "./components/Button.vue";
</script>
```

### 使用props - defineProps

使用`props`需要用到`defineProps`来定义，具体用法跟之前的`props`写法类似：

```js
<script lang="ts" setup>
import { defineProps } from "vue";
const props = defineProps(['title', 'content']);
</script>
```

给`props`定义类型：

```js
const props = defineProps({
  title: String,
  content: {
      type: Stirng,
      required: true
  }
});
```

使用TS的注解的方式：

```ts
defineProps<{
  title?: string
  content: string
}>();
```

增强的props类型定义：

```
const props = defineProps<{
  foo: string
  bar?: number
}>()

const emit = defineEmit<(e: 'update' | 'delete', id: number) => void>()
```

不过注意，采用这种方法将无法使用props默认值。

### 使用emits - defineEmit

使用`defineEmit`对组件里面使用到的事件进行验证和定义：

```js
const emit = defineEmit(['onHeaderClick'])
emit('onHeaderClick', 'params')

// 对事件进行验证
const emit = defineEmit({
    onHeaderClick: ({title}) => {
        if(!title) {
            console.warn('Invalid title')
            return false
        }
        return true
    }
})
```

具体的用法跟之前的一样。

### 使用context - useContext

使用`useContext`获取上下文：

```js
import { useContext } from 'vue'
const { slots, attrs } = useContext()
```

获取到的`slots` `attrs`跟之前的`setup`里面的是一样的。

### 指令

指令跟组件一样，导入自定注册：

```js
<script setup>
  import {color} from './v-color'
</script>

<template>
  <div v-color />
</template>
```

导入的color自动映射为指令`v-color`

```js
<script setup>
  import {color as superColor} from './v-color'
</script>

<template>
  <div v-super-color />
</template>
```

### await语法支持

在script setup内可以直接使用await语法：

```TypeScript
<script setup>
  const post = await fetch(`/api/post/1`).then((r) => r.json())
</script>
```

### 获取 slots 和 attrs

```
<script setup lang="ts">
  import { useContext } from 'vue'

  const { slots, attrs } = useContext()
</script>
```

