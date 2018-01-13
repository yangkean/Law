# Vue 风格指南

> 此指南为参考 [风格指南<sup>beta</sup>](https://cn.vuejs.org/v2/style-guide/) 整理得出的较常使用到的部分.

> 下面的例子会直接指出好例子(good) 和反例(bad).

## [组件名为多个单词](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E5%90%8D%E4%B8%BA%E5%A4%9A%E4%B8%AA%E5%8D%95%E8%AF%8D-%E5%BF%85%E8%A6%81)

```js
// bad
export default {
  name: 'Todo',
  // ...
}

// good
export default {
  name: 'TodoItem',
  // ...
}
```

## [组件的 data 必须是一个函数](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E6%95%B0%E6%8D%AE-%E5%BF%85%E8%A6%81)

```js
// bad
export default {
  data: {
    foo: 'bar',
  }
}

// good
export default {
  data () {
    return {
      foo: 'bar',
    }
  }
}
```

## [Prop 定义应该尽量详细](https://cn.vuejs.org/v2/style-guide/#Prop-%E5%AE%9A%E4%B9%89-%E5%BF%85%E8%A6%81)

```js
// bad
props: ['status']

// good
props: {
  status: String,
}
// better
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error',
      ].indexOf(value) !== -1
    },
  }
}
```

## [为 v-for 设置键值(key)](https://cn.vuejs.org/v2/style-guide/#%E4%B8%BA-v-for-%E8%AE%BE%E7%BD%AE%E9%94%AE%E5%80%BC-%E5%BF%85%E8%A6%81)

```html
<!-- bad -->
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>

<!-- good -->
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

## [避免 v-if 和 v-for 用在一起](https://cn.vuejs.org/v2/style-guide/#%E9%81%BF%E5%85%8D-v-if-%E5%92%8C-v-for-%E7%94%A8%E5%9C%A8%E4%B8%80%E8%B5%B7-%E5%BF%85%E8%A6%81)

```html
<!-- bad -->
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  <li>
</ul>

<!-- good -->
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  <li>
</ul>
```

## [为组件样式设置作用域](https://cn.vuejs.org/v2/style-guide/#%E4%B8%BA%E7%BB%84%E4%BB%B6%E6%A0%B7%E5%BC%8F%E8%AE%BE%E7%BD%AE%E4%BD%9C%E7%94%A8%E5%9F%9F-%E5%BF%85%E8%A6%81)

> 例外: 顶级 App 组件和布局组件中的样式可以是全局的

```html
<!-- bad -->
<template>
  <button class="btn btn-close">X</button>
</template>

<style>
.btn-close {
  background-color: red;
}
</style>

<!-- good -->

<!-- 使用 `scoped` 特性 start -->
<template>
  <button class="button button-close">X</button>
</template>

<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>

<!-- 使用 `scoped` 特性 end -->

<!-- 使用 CSS Modules start -->

<template>
  <button :class="[$style.button, $style.buttonClose]">X</button>
</template>

<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>

<!-- 使用 CSS Modules end -->
```

## [把每个组件单独分成文件](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E6%96%87%E4%BB%B6-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
Vue.component('TodoList', {
  // ...
})
Vue.component('TodoItem', {
  // ...
})

// good
components/
|- TodoList.vue
|- TodoItem.vue
```

## [单文件组件的文件名以横线连接](https://cn.vuejs.org/v2/style-guide/#%E5%8D%95%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6%E6%96%87%E4%BB%B6%E7%9A%84%E5%A4%A7%E5%B0%8F%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
components/
|- mycomponent.vue

components/
|- myComponent.vue

// good
components/
|- my-component.vue
```

## [基础组件全部以一个特定前缀开头](https://cn.vuejs.org/v2/style-guide/#%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue

// good
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue

components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue
```

## [和父组件紧密耦合的子组件应该以父组件名作为前缀命名](https://cn.vuejs.org/v2/style-guide/#%E7%B4%A7%E5%AF%86%E8%80%A6%E5%90%88%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue

// good
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
```

## [以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E5%90%8D%E4%B8%AD%E7%9A%84%E5%8D%95%E8%AF%8D%E9%A1%BA%E5%BA%8F-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue

// good
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

## [自闭合组件](https://cn.vuejs.org/v2/style-guide/#%E8%87%AA%E9%97%AD%E5%90%88%E7%BB%84%E4%BB%B6-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

在单文件组件、字符串模板和 JSX 中没有内容的组件应该自闭合, 但 [DOM 模板](https://cn.vuejs.org/v2/guide/syntax.html) 里永远不要自闭合标签。

但事实是, 我觉得你最好避免在 Vue.js 中使用 DOM 模板, 见 [Why You Should Avoid Vue.js DOM Templates](https://vuejsdevelopers.com/2017/09/17/vue-js-avoid-dom-templates/).

> 后面的规范及代码示例也将不考虑 DOM 模板的情况

```html
<!-- bad -->
<my-component></my-component>

<!-- good -->
<my-component/>
```

## [模板中的组件名始终使用 kebab-case 形式](https://cn.vuejs.org/v2/style-guide/#%E6%A8%A1%E6%9D%BF%E4%B8%AD%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D%E5%A4%A7%E5%B0%8F%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```html
<!-- bad -->
<mycomponent/>
<myComponentOne/>


<!-- good -->
<my-component/>
```

## [JS/JSX 中的组件名始终是 PascalCase 的](https://cn.vuejs.org/v2/style-guide/#JS-JSX-%E4%B8%AD%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D%E5%A4%A7%E5%B0%8F%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
import myComponent from './MyComponent.vue'
export default {
  name: 'myComponent',
  // ...
}
export default {
  name: 'my-component',
  // ...
}

// good
import MyComponent from './MyComponent.vue'
export default {
  name: 'MyComponent',
  // ...
}
```

## [组件名应该倾向于完整单词而不是缩写](https://cn.vuejs.org/v2/style-guide/#%E5%AE%8C%E6%95%B4%E5%8D%95%E8%AF%8D%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
components/
|- SdSettings.vue
|- UProfOpts.vue

// good
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```

## [声明 prop 时命名使用 camelCase 而在模板和 JSX 中使用 kebab-case](https://cn.vuejs.org/v2/style-guide/#Prop-%E5%90%8D%E5%A4%A7%E5%B0%8F%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```js
// bad
props: {
  'greeting-text': String
}
<welcome-message greetingText="hi"/>

// good
props: {
  greetingText: String
}
<welcome-message greeting-text="hi"/>
```

## [多个特性的元素应该分多行撰写，每个特性一行](https://cn.vuejs.org/v2/style-guide/#%E5%A4%9A%E4%B8%AA%E7%89%B9%E6%80%A7%E7%9A%84%E5%85%83%E7%B4%A0-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```html
<!-- bad -->
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<my-component foo="a" bar="b" baz="c"/>

<!-- good -->
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
<my-component
  foo="a"
  bar="b"
  baz="c"
/>
```

## [非空 HTML 特性值应该始终带引号](https://cn.vuejs.org/v2/style-guide/#%E5%B8%A6%E5%BC%95%E5%8F%B7%E7%9A%84%E7%89%B9%E6%80%A7%E5%80%BC-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```html
<!-- bad -->
<input type=text>
<app-sidebar :style={width:sidebarWidth+'px'}>

<!-- good -->
<input type="text">
<app-sidebar :style="{ width: sidebarWidth + 'px' }">
```

## [指令都采用缩写](https://cn.vuejs.org/v2/style-guide/#%E6%8C%87%E4%BB%A4%E7%BC%A9%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)

```html
<!-- bad -->
<input
  v-bind:value="newTodoText"
  :placeholder="newTodoInstructions"
>
<input
  v-on:input="onInput"
  @focus="onFocus"
>

<!-- good -->
<input
  :value="newTodoText"
  :placeholder="newTodoInstructions"
>
<input
  @input="onInput"
  @focus="onFocus"
>
```

## [单文件组件保持 `<template>`、`<script>` 和 `<style>` 的标签顺序](https://cn.vuejs.org/v2/style-guide/#%E5%8D%95%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6%E7%9A%84%E9%A1%B6%E7%BA%A7%E5%85%83%E7%B4%A0%E7%9A%84%E9%A1%BA%E5%BA%8F-%E6%8E%A8%E8%8D%90)

```html
<!-- bad -->
<style>/* ... */</style>
<script>/* ... */</script>
<template>...</template>

<!-- good -->
<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```
