# 基础知识

## 1.vue 手动更新视图

> 在一个组件实例中，只有在 data 里初始化的数据才是响应的，Vue 不能检测到对象属性的添加或删除，没有在 data 里声明的属性不是响应的。
> Vue 不允许在已经创建的实例上动态添加根级响应式属性，但是可以使用$set 方法将相应属性添加到嵌套的对象上。

#### vue 不能检测以下变动的数组

1. 利用索引直接设置一个项时，vm.items[indexOfItem] = newValue

2. 修改数组的长度时，例如： vm.items.length = newLength

#### 解决方案：

```javascript
//1. Vue.set 或者 vm.$set（该方法是全局方法 Vue.set 的一个别名）
Vue.set(vm.items, indexOfItem, newValue);
vm.$set(vm.items, indexOfItem, newValue);

//2. Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue);
```

#### 由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除

```javascript
var vm = new Vue({
  data: {
    test: {
      a: 1,
    },
  },
});
// `vm.test.a` 是响应式的

vm.test.b = 2;
// `vm.test.b` 不是响应式的
```

#### 解决方案：

```javascript
var vm = new Vue({
  data: {
    test: {
      a: 1,
    },
  },
});

//添加一个新的属性到嵌套的 test 对象：
Vue.set(vm.test, "b", 2);
//或者
vm.$set(vm.test, "b", 2);
```

#### 有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign() 或 \_.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：

```javascript
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: "Vue Green",
});
```

应该这样做：

```javascript
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: "Vue Green",
});
```

## 2.params 传参获取不到的原因

```js
使用params时，不能使用path，应使用name跳转
```

## 3.解决前端 Value should be trueValue or falseValue.

```js
const ps = Object.assign({}, this.pageFormDatas, {
  moduleImg: "ts ts-envamables",
  isInit: false,
  tmpID: randomWord(),
  parentId: isGroup ? this.curMod.id : this.datas.id,
  customColor: this.colorList.length ? this.colorList.shift() : "#B3B3B3",
  moduleType: 0,
  status: this.pageFormDatas ? 1 : 0,
});
替换;
const ps = Object.assign(this.pageFormDatas, {
  moduleImg: "ts ts-envamables",
  isInit: false,
  tmpID: randomWord(),
  parentId: isGroup ? this.curMod.id : this.datas.id,
  customColor: this.colorList.length ? this.colorList.shift() : "#B3B3B3",
  moduleType: 0,
  status: this.pageFormDatas ? 1 : 0,
});
```

## 4.跨组件刷新可通过自定义事件完成

> 各级组件都需要传递

```js
// 顶级组件
<router-view
  v-if="initState"
  @on-refresh-menu="getMenuData"
  @on-refresh-logo="getLogos"
/>
// 父级组件
<template>
  <AuthorityManage @on-refresh-menu="$emit('on-refresh-menu')" />
</template>

<ModNode
  v-for="mod in datas.children"
  :key="mod.id"
  :datas="mod"
  :custom-color="mod.customColor || '#B3B3B3'"
  :level="0"
  @on-refresh-menu="$emit('on-refresh-menu')"
></ModNode>

// 子组件触发
this.$emit('on-refresh-menu')
```

## 5.'Computed property “tableData” was assigned to but it has no setter.'报错

```js
computed: {
    tableData : {
        get: function () {
              return  this.$store.getters.getOrderList
        },
        set: function () {

        }
    }

},
```

## 6.prop

```js
//prop类型指定
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}

//prop验证
props: {
  // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
  propA: Number,
  // 多个可能的类型
  propB: [String, Number],
  // 必填的字符串
  propC: {
    type: String,
    required: true
  },
  // 带有默认值的数字
  propD: {
    type: Number,
    default: 100
  },
  // 带有默认值的对象
  propE: {
    type: Object,
    // 对象或数组默认值必须从一个工厂函数获取
    default: function () {
      return { message: 'hello' }
    }
  },
  // 自定义验证函数
  propF: {
    validator: function (value) {
      // 这个值必须匹配下列字符串中的一个
      return ['success', 'warning', 'danger'].indexOf(value) !== -1
    }
  }
}
```

## 7.计算属性修改 props 传值

```js
props: {
  ruleData: {
    type: Object,
    default: () => {
      return {}
    }
  },
},
computed: {
  initRuleData() {
    if (this.ruleData) {
      this.ruleData.nodes.forEach(node => {
        node.available = node.available ? node.available : true
      })
      this.ruleData.lines.forEach(line => {
        line.available = line.available ? line.available : true
      })
    }
    return this.ruleData
  }
},
data() {
  return {
    hasSaveRuleData:[]
  }
}
created() {
  // 计算属性的执行顺序是在created的之前
  this.hasSaveRuleData = this.initRuleData
},
```

## 8.mixin 执行顺序及使用

1. mixin created

2. 组件 created

3. mixin mounted

4. 组件 mounted

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。
比如，数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。

同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。

```js
// auto-table-height.js
export default {
  data() {
    return {
      tableHeight: 500
    }
  },
  mounted() {
    this.resizeTableHeight()
    window.addEventListener('resize', this.resizeTableHeight)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.resizeTableHeight)
  },
  methods: {
    resizeTableHeight() {
      this.tableHeight = this.$refs['table-wrap'] ? this.$refs['table-wrap'].offsetHeight : this.tableHeight
    }
  }
}

// 调用
import autoTableHeight from '@/mixin/auto-table-height'
export default {
  mixins: [autoTableHeight]
}
```

## 9.自定义指令

```js
// 局部注册
directives: {
    focus: {
      // 指令的定义
      inserted: function(el) {
        console.info("focus-inserted", el);
        el.getElementsByTagName("input")[0].focus();
      }
    },
    swatchColor: {
      // 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
      bind: function(el, binding) {
        console.info("swatchColor-bind:1", el, binding.value);
        el.style.backgroundColor = binding.value.bgColor;
      },
      // 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
      inserted: function(el, binding) {
        console.info("swatchColor-inserted:2", el, binding.value);
        el.style.backgroundColor = binding.value.bgColor;
      },
      // 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。
      update: function(el, binding) {
        console.info("swatchColor-update:3", el, binding.value);
        el.style.color = binding.value.color;
      },
      // 指令所在组件的 VNode 及其子 VNode 全部更新后调用。
      componentUpdated: function() {
        console.info("swatchColor-componentUpdated:4");
      },
      // 只调用一次，指令与元素解绑时调用。
      unbind: function() {
        console.info("swatchColor-unbind:5");
      }
    }
  }

// 全局注册-main.js
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})

// 或者customDirective.js
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
// main.js引入
import customDirective from './plugins/customDirective'
Vue.use(customDirective)
```

## 10.插槽

- 基础插槽

```js
// 父组件
<slot-tpl>
  <h4>插槽内容</h4>
  <span>{{ string }}</span>
</slot-tpl>

// 子组件
<template>
  <div class="page">
    <a v-bind:href="url" class="nav-link">
      <span style="color:#999;">子组件内容</span>
      <slot>默认显示内容</slot>
    </a>
  </div>
</template>
```

- 具名插槽

> template 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 v-slot 的中的内容都会被视为默认插槽的内容,一个不带 name 的 slot 出口会带有隐含的名字“default”。

```js
// 父组件
<slot-tpl>
  <template v-slot:header>
    <h2>标题</h2>
  </template>

  <p>主要内容</p>
  <p>其他内容</p>

  <template v-slot:footer>
    <p>页尾</p>
  </template>
</slot-tpl>

// 子组件
<template>
  <div class="page">
    <div class="container">
      <header>
        <slot name="header"></slot>
      </header>
      <main>
        <slot></slot>
      </main>
      <footer>
        <slot name="footer"></slot>
      </footer>
    </div>
  </div>
</template>
```

- 作用域插槽

> 使用带值的 v-slot 让插槽内容能够访问子组件中才有的数据,父组件能够直接访问子组件的数据并且修改默认展示

在父级作用域中，我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字 ,将 user 作为 <slot> 元素的一个 attribute 绑定上去：绑定在 <slot> 元素上的 attribute 被称为插槽 prop

```js
// 父组件
<slot-tpl>
  <template v-slot="slotProps">
        {{ slotProps.user.firstName }}
  </template>
</slot-tpl>

// 子组件
<template>
  <div class="page">
    <span>
      <slot :user="user">
        <span>{{ user.lastName }}</span>
      </slot>
    </span>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class SlotTpl extends Vue {
  user = {
    firstName: "waker",
    lastName: "yang",
    other: "amumu",
  };
}
</script>

```

多个作用域插槽和具名插槽一起使用

```js
// 父组件
<slot-tpl>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
  <template v-slot:other="otherSlotProps">
    {{ otherSlotProps.other.test1 }}
  </template>
</slot-tpl>

// 子组件
<template>
  <div class="page">
    <span>
      <slot :user="user" name="default">
        <span>{{ user.lastName }}</span>
      </slot>
      <slot :other="other" name="other">
        <span>{{ other.test }}</span>
      </slot>
    </span>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class SlotTpl extends Vue {
  user = {
    firstName: "waker",
    lastName: "yang",
    other: "amumu",
  };
  other = {
    test: 21354,
    test1: 98765,
  };
}
</script>

```

- 动态插槽名

> 动态指令参数也可以用在 v-slot 上，来定义动态的插槽名：

```js
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

## 11.作用全局样式

#### 让全局样式覆盖组建 scope 样式

- 配置单独样式文件，然后在 main.js 引入

- 在 app.vue 里面添加不带 scope 的 style 样式

## 12.关于组件字体文件使用问题

- 如果项目作为包供第三方组建使用，则静态资源不要放在 public 下面，要添加到 src/assets 下面，并且第三方在 main.js 里面 引入对应的包路径
