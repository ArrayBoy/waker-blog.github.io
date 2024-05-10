<!--
 * @Author: waker
 * @Date: 2021-06-30 18:28:08
 * @LastEditTime: 2021-11-22 11:28:20
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \blog\docs\Work\index.md
-->

## 孪生服务 DTService > portalPage.vue

- 绑定的事件里面可简写 @click="() => (showModalCreateService = true)"
- 校验提示再明确
- 命名语义化 let b = res.filter(r => r.digitalServiceTypeCode === 0)
- 判断请求结果最好通过 res.code || res.success
-
-
-

## 孪生模型 DTModel > index.vue

以 table 为例：
组件流：
UinoTable  
columns：定义表头
slot 字段需求修改或格式化的列

Page：分页
API： api > interface > 对应自己的模块 > url |——ajax 封装方法提取

## 公共部分

- 侧边栏 menu, 头部 nav ——依赖'@uino/kiss-core-develop'
- 侧边栏的菜单配置与当前用户权限有关，但目前仍然存在重复加载的情况
- 获取当前用户信息：getCurrentUser——在 localstorage 里面做持久化存储
  > src\App.vue

## store

```javascript
//正常vuex module
computed: {
    previewMod(){
        console.log(this.$store.state.ModelStore.previewMod)
        return this.$store.state.ModelStore.previewMod
    }
}

//mapState
import { mapMutations, mapState } from 'vuex'
computed: {
    ...mapState('ModelStore', [
        'previewMod'
    ]),
}
```

## 封装方法：

- this.$L——public>static>js>i8n.js     this.$L.get('COMMON_ALL') ：转换为中文“全部”
- copy

```js
import uiv from '@/plugins/uinv'
copy(str) {
    uiv.copy('.copy', str)
}
```

-
-
-
-
-

## 封装组件：

> 直接调用

- Button

```js
<Button
type="primary"
:class="{ forbid: previewMod }"
@click="showModel(null)"
>创建模型</Button
>
<Button
    type="primary"
    disabled
    class="mar10"
    custom-icon="ts ts-import"
    >批量导入</Button
>
```

- Select

```js
<Select
v-model="params.cdt.dirId"
style="width:120px"
class="ml10"
transfer
@on-change="modelSearch"
>
<Option :value="0">全部领域</Option>
<Option
    v-for="(item, index) in dirList"
    :key="index"
    :value="item.id"
>
    {{ item.dirName }}
</Option>
</Select>
```

- Input

```js
<Input
v-model.trim="params.keyword"
class="ml10"
search
style="width:240px"
:placeholder="
    showTable === true ? '搜索模型名称或简写' : '搜索模型名称'
"
@on-change="modelSearch"
/>
```

- Tabs
  > src\components\DTModel-test\modelDetail.vue

```js
import ModelInfo from './components/modelInfo'
import ModelAttr from './components/modelAttr'
import ModelService from './components/modelService/index'
import RelationBuild from './components/relationBuild'
import EventRules from './kpiOperate/index.vue'
<Tabs v-model="tabValue" size="small">
    <TabPane label="模型信息" name="modelInfo">
    <ModelInfo v-if="tabValue === 'modelInfo'" :data="mData" />
    </TabPane>
    <TabPane label="模型属性" name="modelAttr">
    <ModelAttr
        v-if="tabValue === 'modelAttr'"
        :data="mData"
        :current="current"
    />
    </TabPane>
    <TabPane label="模型服务" name="modelService">
    <ModelService
        v-if="tabValue === 'modelService'"
        :data="mData"
        :current="current"
        :tab-value="tabValue"
    />
    </TabPane>
    <TabPane label="告警规则" name="eventRules">
    <EventRules v-if="tabValue === 'eventRules'" />
    </TabPane>
    <TabPane label="关系构建" name="RelationshipBuilding">
    <RelationBuild
        v-if="tabValue === 'RelationshipBuilding'"
        :prop-data="mData"
        :current="current"
    />
    </TabPane>
</Tabs>
```

- 公共 table 组件

```js
src\components\common\keyValueTable\index.vue
被model详情调用
src\components\DTModel\modelDetail.vue
```

- 3d 模型展示
  > src\components\common\keyValueTable\index.vue

```js
import { threeDimensionalModel } from '@/plugins/3d.js'
threeDShow(data) {
    this.isThreeDModelShow = true
    let path = data
    .split(' ')[1]
    .split('=')[1]
    .split('.jpg')[0]
    .split('8080')[1]
    threeDimensionalModel(path)
}
```

- Dropdown

```js
<Dropdown placement="bottom" @on-click="onOperateTopDrop">
    <Button
        class="drop-down"
        size="small"
        custom-icon="uino-iconfont iconicon_more"
    ></Button>
    <DropdownMenu slot="list">
        <DropdownItem name="import">{{
        'COMMON_IMPORT' | L
        }}</DropdownItem>
        <DropdownItem name="export">{{
        'COMMON_EXPORT' | L
        }}</DropdownItem>
        <DropdownItem v-if="!showTable" name="queryTemplate">
        查看模版
        </DropdownItem>
        <DropdownItem v-if="!showTable" name="saveAsTemplate">
        另存为模版
        </DropdownItem>
    </DropdownMenu>
</Dropdown>
```

- 右侧面板<Panel></Panel>
  > src\components\DTModel-test\modelGraph\index.vue

## 公共代码

```js
//删除及确认弹窗
deleteModel(value) {
    this.$Modal.confirm({
    title: this.$L.get('COMMON_DELETE'),
    content: this.$L.get('COMMON_PLEASE_CONFIRM_WHETHER_TO_DELETE2', {
        name: value.classCode
    }),
    loading: true,
    onOk: async () => {
        const res = await this.ajax({
        URL: 'DT_DELETE_MODEL',
        data: { id: value.id }
        })
        if (res) {
        this.getModelList()
        this.Snackbar.success('删除成功')
        }
        this.$Modal.remove()
    }
    })
},
```

> src\components\DTModel\index.vue

```js
<!-- 一键导入 上传 -->
    <Import
      v-model="importBox"
      :title="'COMMON_BULK_IMPORT' | L"
      :import-url="importUrl"
      :percent="uploadPercent"
      :status="importRes"
      :contain-template="false"
      :failed-result-text="'COMMON_DOWNLOAD_THE_IMPORT_DETAIL_FILE' | L"
      :tip-text="'请选择JSON文件或ZIP文件'"
      :upload-file-type="uploadFileType"
      upload-file-accept=".json, .zip"
      @upload-success="uploadFileSuccess"
      @upload-complate="importBoxChange"
    />
    <!-- 一键导入 选择 json -->
    <Shuttle
      v-model="showImport"
      :title="importShuttleTitle"
      :flag-word="'DCV_MODEL' | L"
      :placeholder="'COMMON_SEARCH_MODEL' | L"
      :data="fileJsons"
      :default-key="defSelectFileJsons"
      :default-disabled="false"
      @on-ok="importData"
    />
    <!-- 导出 -->
    <Shuttle
      v-model="showExport"
      :title="['COMMON_EXPORT'] | L"
      :tile="false"
      :flag-word="'DCV_MODEL' | L"
      :placeholder="
        'COMMON_SEARCH_ITEM'
          | L({
            item: `${$L.get('DCV_MODEL')}`.toLowerCase()
          })
      "
      :data="exportTree"
      :default-key="defSelectExportIds"
      :default-disabled="false"
      :has-all="true"
      :empty-prompt="'COMMON_PLEASE_CHOOSE' | L"
      @on-ok="exportData"
    />
```

```js
//ajax tpl
async getUserDefaultPwd() {
    try {
        const res = await this.ajax({
            URL: 'GET_USER_DEFAULT_PASSWORD',
            // data: 'uino.permission.user.initPassword'
            data: {
                pageSize: 100
            }
        })
        if (res) {
            const decodePwd = uinv.decode(res)
            this.userDefaultPwd = decodePwd
        }
    } catch (e) {
        this.orgsTree = []
    }
},
```

#### 关系模型

> src\components\DTModel-test\modelGraph\index.vue

## other

1. 告警处理——canvas

> src\components\alarmProcess\Range.vue

2. 图形图标——echarts

服务运营

> src\components\serviceOpt\index.vue

## dev 流程

1. 路由：src\router\dtmp.js

2. 入口文件：src\views\dtmp\model-test.vue

3. 组件：src\components\DTModel-test\index.vue

4. 请求地址配置：src\api\interface\DTModel.js

5. 权限配置： 系统设置添加路由————权限设置勾选新加路由

## 图片选择器

> core

```js
// new

src / components / CI / components / iconMap / index.vue;
eg: 对象管理-对象定义
src/components/CI/index.vue 里面inject到src/components/CI/components/attrs.vue
eg: 可视化建模抽屉面板左上角--solt插槽
src/components/VisualModel/panel/ciClassPanel/panelImage.vue
eg: 资源中心-图标管理映射
src/components/IconManage/components/firstList.vue
// old
src / components / modules / iconMap / index.vue;
```

## 默认图片

```js
//2d
// http://192.168.21.72:8080/rsm/122/defaultIcon/default_ci_picture_attr_icon.png
// DEF_ICON_PEOPLE: "/cmdb/ciClass/getDefaultPictureIcon",
// defIconPeopleLocal: `this.src="${require('/public/default_ci_picture_attr_icon.png')}"`,
// defIconPeople: '',
this.ajax({ URL: "DEF_ICON_PEOPLE" }).then((res) => {
  this.defIconPeople = res;
});

//3d
//http://192.168.21.72:8080/rsm/122/default_3d.jpg
//  GET_DEFAULT_3D_ICON: '/cmdb/ciClass/isShow3dAttribute',
//  default3D_icon: '',
async queryDefault3DIicon() {
    const res = await this.ajax({
    URL: 'GET_DEFAULT_3D_ICON'
    })
    this.default3D_icon = res.defaultIcon || ''
},
```

## 表格 table

1. slot 方式和 render 方式

```js
<UinoTable
border
:columns="columns"
:data="tableList"
:loading="loading"
:max-height="tableHeight"
@on-selection-change="selectRow"
>
    <template slot="3D模型" slot-scope="{ row }">
        <img :src="row['3D模型']" class="show-img-table" />
    </template>
</UinoTable>

columns = [
    {
        title: this.$L.get('EMV_STATUS'),
        key: 'active',
        slot: '3D模型',
        width: 50
    },
    {
        type: 'selection',
        width: 40,
        align: 'center'
    },
    {
        title: 'CI Code',
        key: '_ciCode',
        width: 130,
        renderHeader: (h, { column }) => {
            return h('div', [
                h(
                    'span',
                    {
                        props: {
                        title: column.title
                        }
                    },
                    column.title
                ),
                h(
                    'Tooltip',
                    {
                        props: {
                        content: this.$L.get('COMM_CI_CODE_EXPLAIN'), //'缺省由系统根据“主键”生成， 一个代理键值确定了一个对象（CI实例）。',
                        maxWidth: 200,
                        transfer: true
                        }
                    },
                    [
                        h('Icon', {
                        props: { custom: 'ts ts-remarl', size: 12 },
                        style: {
                            color: '#999',
                            marginLeft: '4px',
                            marginTop: '-2px'
                        }
                        })
                    ]
                )
            ])
        },
        tooltip: true
    },
    {
        title: 'render-test',
        key: 'render',
        render: (h, params) => {
            return h('img', {
                attrs: {
                    src: el.proStdName ? params.row[el.proStdName] : '',
                    class: 'show-img-table'
                    // onerror: this.defIconPeopleLocal
                },
                style: {
                    width: '40px'
                }
            })
        },
        render: (h, params) => {
            if (this.showEditAttrBtn) {
                return h('div', params.row.proName)
            } else {
                return h('div', [
                    h('Input', {
                        props: {
                            value: params.row.proName,
                            disabled: params.row.isParent
                        },
                        on: {
                            'on-change': e => {
                                const rowItem = this.attrDefs.find((item, index) => {
                                    return index === params.index
                                })
                                if (rowItem) {
                                    rowItem.proName = e.target.value
                                }
                            }
                        }
                    })
                ])
            }
        }
    }
]
```

## 点击空白关闭弹窗

> this.$el 指的是当前组件的的 root 元素, this.$ref 指当前具有 ref 属性的元素

```js
//关闭选择框
document.addEventListener("click", (e) => {
  if (!this.$el.contains(e.target)) this.isShowGrid = false;
});
```

## v-modal 及 props

```js
<GridSelector
    v-model="shieldForm.days"
    width="226"
    :grid-type="gridType"
></GridSelector>

//子组件
model: {
    prop: 'days',
    event: 'updateGridFn'
},
props: {
    days: {
      type: Array,
      default: () => []
    },
    width: {
      type: String,
      default: '200'
    },
    gridType: {
      type: String,
      default: ''
    }
},
methods: {
    update() {
        const newDays = [1,2,3]
        this.$emit('updateGridFn', newDays)
    }
}
```

> more props

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

## axios 的封装

```js
// 项目引入及axios全局设置：src/plugins/vue-axios.js

// ajax方法封装：src/config/install.js
import '@/plugins/index' // 引入插件（axios,i18n,cookieTool）
Vue.mixin({
    methods: {
        async ajax(e, method) {
          let url = this.interface[e.URL]
          ...
        }
    }
})
```

## select 组件 option slot

```js
<Select
    v-model="item.targetClassId"
    placeholder="请选择模型"
    :transfer="false"
    class="select-dropdown-retop"
    @on-change="modelChange"
>
    <Option v-for="(itm, idx) in modelList" :key="idx" :value="itm.id" :label="itm.classCode">
    <Tooltip :content="itm.classCode" max-width="162" transfer :delay="50">
        <div class="model-ellipsis">{{ itm.classCode }}</div>
    </Tooltip>
    </Option>
</Select>
<style>
.select-dropdown-retop {
  /deep/ .ivu-select-dropdown,
  /deep/ .ivu-select-dropdown-container {
    top: 31px !important;
  }
  /deep/ .ivu-select-dropdown-container {
    .ivu-select-dropdown {
      top: 0 !important;
    }
  }
}
.model-ellipsis {
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  vertical-align: middle;
  max-width: 130px;
}
</style>
```
