<!--
 * @Author: your name
 * @Date: 2021-09-02 15:10:21
 * @LastEditTime: 2023-09-06 15:33:41
 * @LastEditors: yanglilong yanglilong@uino.com
 * @Description: In User Settings Edit
 * @FilePath: /blog/docs/vue/vue3/base.md
-->

## vue3 脚手架

[官网](https://cn.vuejs.org/guide/scaling-up/tooling.html#project-scaffolding)

```js
// 这个命令会安装和执行 create-vue，它是 Vue 提供的官方脚手架工具。跟随命令行的提示继续操作即可。
npm create vue@latest

// 安装完成有一个推荐命令
npm run format  //格式化当前代码
npm run format --all //命令来格式化整个代码库。需要注意的是，这个工具可能会修改代码库中的某些部分，因此在使用前一定要备份代码
```

要学习更多关于 Vite 的知识，请查看 [Vite](https://cn.vitejs.dev/) 官方文档。
若要了解如何为一个 Vite 项目配置 Vue 相关的特殊行为，比如向 Vue 编译器传递相关选项，请查看 [@vitejs/plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue#readme) 的文档。

#### create-vue 与 vue-cli 的区别

- Vue CLI 是官方提供的基于 Webpack 的 Vue 工具链，它现在处于维护模式。我们建议使用 Vite 开始新的项目，除非你依赖特定的 Webpack 的特性。
- create-vue 是基于 vite 的工具链

#### 浏览器内模板编译注意事项

当以无构建步骤方式使用 Vue 时，组件模板要么是写在页面的 HTML 中，要么是内联的 JavaScript 字符串。在这些场景中，为了执行动态模板编译，Vue 需要将模板编译器运行在浏览器中。相对的，如果我们使用了构建步骤，由于提前编译了模板，那么就无须再在浏览器中运行了。为了减小打包出的客户端代码体积，Vue 提供了多种格式的“构建文件”以适配不同场景下的优化需求。

前缀为 vue.runtime.\* 的文件是只包含运行时的版本：不包含编译器，当使用这个版本时，所有的模板都必须由构建步骤预先编译。

名称中不包含 .runtime 的文件则是完全版：即包含了编译器，并支持在浏览器中直接编译模板。然而，体积也会因此增长大约 14kb。

默认的工具链中都会使用仅含运行时的版本，因为所有 SFC 中的模板都已经被预编译了。如果因为某些原因，在有构建步骤时，你仍需要浏览器内的模板编译，你可以更改构建工具配置，将 vue 改为相应的版本 vue/dist/vue.esm-bundler.js。

如果你需要一种更轻量级，不依赖构建步骤的替代方案，也可以看看 petite-vue。

## vue3+vite

#### setup

```js
//父组件
<script setup>
import { ref } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
let text = ref(0);
const foo1 = () => {
  text.value = 1;
};
</script>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    foo() {
      this.text = 2;
    },
  },
};
</script>

<template>
  <HelloWorld msg="yll-test" />
  <p>{{ text }}</p>
  <button @click="foo">click</button>
  <button @click="foo1">setup click</button>
</template>

<style></style>

//子组件
<script setup>
import { ref } from "vue";

defineProps({
  msg: String,
});

const count = ref(0);
</script>

<template>
  <h1>{{ msg }}</h1>
  <button type="button" @click="count++">count is: {{ count }}</button>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
```

## vue3+ts+vite

#### [vue3+ts](https://v3.cn.vuejs.org/guide/typescript-support.html#%E5%AE%9A%E4%B9%89-vue-%E7%BB%84%E4%BB%B6)

- 默认 data

```js
<script lang="ts">
interface ListItem {
  id: number;
  name: string;
}
interface Book {
  title: string;
  year?: number;
}
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    const dataSource: ListItem[] = [];
    const array: Array<any> = [];
    const arrayNum: number[] = [];
    const text: string = "字符串";
    const obj: Book = {
      title: "waker",
    };
    return {
      dataSource,
      array,
      arrayNum,
      text,
      obj,
    };
  },
  created() {
    this.foo();
  },
  methods: {
    foo(a?: any, b?: any) {
      this.text = "waker";
      this.dataSource = [
        {
          id: 12345,
          name: "waker",
        },
      ];
      this.array = [1, 2, true, null, undefined, "waker"];
      this.obj = { title: "cd", year: 18 };
    },
  },
});
</script>
<template>
  <HelloWorld msg="yll-test" />
  <p>{{ text }}</p>
  <button @click="foo">click</button>
  <button @click="foo1">setup click</button>
  <ul>
    <li v-for="i in array" :key="i">{{ i }}</li>
  </ul>
</template>
```

- setup

```js
<script lang="ts">
import {
  defineComponent,
  PropType,
  reactive,
  ref,
  toRefs,
  computed,
  onUnmounted,
  onMounted,
  nextTick
} from 'vue'
import { CloseOutlined } from '@ant-design/icons-vue'
import { find } from 'lodash'
import { BasicUserType, RoleType, StateType } from '@/@types'
import { useStore } from 'vuex'
import { message } from 'ant-design-vue'

export default defineComponent({
  name: 'Selector',
  components: { CloseOutlined },
  props: {
    title: {
      type: String,
      default: '',
      require: true
    },
    list: {
      type: Array as PropType<BasicUserType[]>,
      require: true,
      default: []
    },
    selectedList: {
      type: Array as PropType<BasicUserType[]>,
      require: true,
      default: []
    },
    mode: {
      type: String,
      default: 'all' // all 所有团队成员  group 分组成员
    }
  },
  setup(props, context) {
    const store = useStore<StateType>()

    // 基础数据
    const state = reactive({
      inAdd: false,
      inSearch: false,
      indeterminate: false,
      checkAll: false,
      keyword: '',
      optionsUp: false,
      selectedData: [] as any[]
    })

    const rawList: BasicUserType[] = [...props.selectedList]

    // 搜索结果是否显示
    const optionsVisible = computed(() => state.inAdd || state.inSearch)

    // 搜索结果
    const optionsData = computed(() => {
      if (state.inAdd) {
        return props.list
      } else if (state.inSearch) {
        return props.list.filter((item: any) =>
          item.username.toLowerCase().includes(state.keyword.toLowerCase())
        )
      } else {
        return []
      }
    })

    // refs
    const selectorRef = ref(null)
    const searchRef: any = ref(null)
    const optionsRef = ref(null)

    /**
     * 点击'添加'按钮
     */
    const handleAdd = (e: any) => {
      state.inSearch = false
      state.inAdd = true
      // monitorClick()
      const screenHeight = window.innerHeight
      if (e.y > screenHeight * (2 / 3)) {
        state.optionsUp = true
      } else {
        state.optionsUp = false
      }
    }

    /**
     * 处理搜索
     */
    const handleSearch = () => {
      state.inAdd = false
      if (state.keyword.length !== 0) {
        state.inSearch = true
        // monitorClick()
      } else {
        state.inSearch = false
      }
    }

    /**
     * 点击搜索框
     */
    const handleSearchClick = () => {
      state.inAdd = false
      handleSearch()
    }

    const canChoose = (item: BasicUserType) => {
      // 团队分组成员模式，所有选项可选
      if (props.mode === 'group') {
        return true
      }

      // 是自己的话，不能选
      const user = store.state.user.userDetail
      if (user.userId === item.userId) {
        return false
      }
      if (
        item.roleId == RoleType['超级管理员'] ||
        item.roleId == RoleType['团队超级管理员']
      ) {
        return false
      }
      return true
    }

    const changeSelectAllStatus = () => {
      nextTick(() => {
        state.indeterminate =
          state.selectedData.length > 0 &&
          state.selectedData.length < props.list.length
        state.checkAll = state.selectedData.length === props.list.length
      })
    }

    // 搜索选项是否被选中
    const isChecked = (id: number) => {
      return state.selectedData.filter((item) => item.userId == id).length !== 0
    }

    /**
     * 点击搜索选项
     */
    const checkItem = (data: any) => {
      if (!canChoose(data)) {
        message.warning('不能删除自己或者团队超级管理员')
        return false
      }
      if (!isChecked(data.userId)) {
        state.selectedData = [...state.selectedData, data]
      } else {
        state.selectedData = [...state.selectedData].filter(
          (item) => item.userId !== data.userId
        )
      }

      changeSelectAllStatus()

      state.keyword = ''
      state.inSearch = false
      searchRef.value.focus()

      const deleteList = rawList
        .map((item) => {
          if (!find(state.selectedData, ['userId', item.userId])) {
            return item
          }
        })
        .filter((item: any) => Boolean(item))

      const addList = state.selectedData
        .map((item) => {
          if (!find(rawList, ['userId', item.userId])) {
            return item
          }
        })
        .filter((item: any) => Boolean(item))

      context.emit('list-change', { addList, deleteList })
    }

    /**
     * 点击全选
     */
    const onCheckAllChange = () => {
      state.selectedData =
        props.list.length === state.selectedData.length ? [] : [...props.list]
      state.indeterminate = false
      context.emit('list-change', {
        addList: state.selectedData,
        deleteList: []
      })
    }

    // 事件监听相关

    /**
     * 关闭待选项
     */
    const closeOptions = () => {
      state.inAdd = false
      state.inSearch = false
      ;(optionsRef.value as any).scrollTop = 0
    }

    /**
     * 删除最后一项
     */
    const deleteLastData = () => {
      state.selectedData = [...state.selectedData].slice(
        0,
        state.selectedData.length - 1
      )
      changeSelectAllStatus()
    }

    /**
     * 处理鼠标点击事件监听
     */
    const handleClickListen = (e: any) => {
      if (!(selectorRef.value as any).contains(e.target)) {
        closeOptions()
      }
    }

    /**
     * 处理键盘按下事件监听
     */
    const handleKeydownListen = (e: any) => {
      if (
        (searchRef.value as any).isFocused &&
        e.keyCode === 8 &&
        state.keyword === ''
      ) {
        deleteLastData()
      }
    }

    // 格式化名称
    const getFormatName = (item: BasicUserType) => {
      if (item.nickName) {
        return item.nickName
      }
      if (item.username) {
        return item.username.split('-')[0]
      }
      return '团队超级管理员'
    }

    onMounted(() => {
      state.selectedData = props.selectedList
      document.addEventListener('click', handleClickListen)
      document.addEventListener('keydown', handleKeydownListen)
    })

    onUnmounted(() => {
      document.removeEventListener('click', handleClickListen)
      document.removeEventListener('keydown', handleKeydownListen)
    })

    return {
      ...toRefs(state),
      canChoose,
      optionsData,
      selectorRef,
      searchRef,
      optionsRef,
      optionsVisible,
      handleAdd,
      handleSearchClick,
      handleSearch,
      isChecked,
      checkItem,
      onCheckAllChange,
      getFormatName
    }
  }
})
</script>
```

> BasicUserType from '@/@types'

```js
export interface BasicUserType {
  id: number
  name?: string
  avatar?: string
  role?: string
  department?: string
  code?: string
  createTime?: string
  description?: string
  email?: string
  lastLoginTime?: string
  modifyTime?: string
  modifyUser?: number
  nickName?: string
  phone?: string
  roleId?: number
  roleName?: string
  status?: number
  tenantId?: number
  type?: string
  userId?: number
  username?: string
  cloudRole?: string
}

```

## vuex 与 pinia 的区别

Vuex 与 Pinia 的区别如下：

1. 架构设计：Vuex 采用了集中式的架构，将所有的状态存储在一个单一的全局状态树中，通过 mutations 和 actions 来修改和处理状态。Pinia 采用了去中心化的架构，将状态分布在多个模块中，每个模块拥有自己的状态、mutations 和 actions。

2. 体积和复杂性：Vuex 是 Vue.js 的官方状态管理库，在 Vue.js 项目中广泛使用，并拥有庞大的生态系统。Pinia 是一个相对较小的库，更简单，在一些小型或简单的项目中可能更容易上手。

3. 灵活性：Pinia 提供了更加灵活的状态管理方式，因为它支持多个 store 实例，而 Vuex 只支持一个 store 实例。
