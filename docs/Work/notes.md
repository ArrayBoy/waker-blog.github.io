## kiss core & kiss middle

## 菜单设置

1. creating

> 添加了组，页面或者按钮的标识，用于添加完成删除数据的判断标志

2. 表格行组件（div 模拟）ModNode

> 列表行采用递归渲染完成树结构多级嵌套

3. level

> 菜单缩进通过 pages 组件传 level 到 page-node 来计算样式缩进

4. 按钮添加 loadding

```javascript
this.newPageLoading = false;
this.$nextTick(() => {
  this.newPageLoading = true;
});
```

5. 让 input 获得焦点

```javascript
this.$nextTick(() => {
  this.$refs["label_name"].focus();
});
```

6. createMod

> 是否创建模块标识

7. create

> 添加小组，页面，按钮已确认，用于判断是否显示下一个输入框

8. 国际化操作

```javascript
//三元符操作
const key = data.id ? 'COMMON_UPDATE_SUCCESSFUL' : 'COMMON_ADDING_SUCCESSFUL'
this.$Message.success(this.$L.get(key))

//过滤器使用
:placeholder="'COMMOON_PLEASE_ENTER_A_SYSTEM_NAME' | L"
```

9. vuex 使用

> 每一个组件下面有一个 store 模块，里面包含 getters.js，mutations.js，index.js

- index.js 引入前 2 个文件 getters.js，mutations.js， 定义所需 store 的状态字段

  ```javascript
  import mutations from "./mutations";
  import getters from "./getters";
  export default {
    namespaced: true,
    state: {
      urlPrefix: "",
      urls: {},
      curMod: null, // 菜单配置 右侧选中
      dragMod: null,
      // 字典表列表
      dictList: [],
      // 当前编辑的字典表的数据
      dictInfo: {},
      // 当前编辑的字典表的属性数据
      dictAttrDefs: [],
      // 当前字典表的状态 type：1为新增，2为编辑, 3为空
      dictStatus: { type: 3 },
      // 当前选中的字典id
      activeDictId: null,
      //公共组件还是中台
      mode: "core",
    },
    mutations,
    getters,
  };
  ```

- getters.js

  ```js
  export default {
    UpdateLogoUrl: (state) => {
      const key = "LOGO_UPDATE";
      return state.urlPrefix + state.urls[key];
    },
  };
  ```

- mutations 定义操作 state 的方法

  ```js
  export default {
    setUrlPrefix(state, urlPrefix) {
      state.urlPrefix = urlPrefix;
    },
    setUrls(state, e) {
      state.urls = e;
    },
    setCurMod(state, e) {
      state.curMod = e;
    },
    setDragMod(state, e) {
      state.dragMod = e;
    },
  };
  ```

- 组件使用

  ```javascript
  //计算属性直接使用state
  computed: {
      ...mapState('SystemStore', ['curMod']),
      lockCreate() {
          let status = false
          if ((this.curMod && this.curMod.moduleUrl) || this.creating) status = true
          return status
      }
  },

  //mutations
   methods: {
      ...mapMutations('SystemStore', ['setCurMod']),
      handleThis() {
          if (!this.datas.id) return
          if (this.curMod && this.curMod.id === this.datas.id) this.setCurMod(null)
          else this.setCurMod(this.datas)
      }
  }
  ```

10. vue2.0 给 data 对象新增属性，并触发视图更新

```javascript
data () {
    return {
        student: {
            name: '',
            sex: ''
        }
    }
}
mounted () {
    this.$set(this.student,"age", 24)
    // this.$set(this.datas, 'edit', false)
}
```

11. 阻止弹窗默认关闭

```js
this.$Message.error(msg)
this.createButtonLoading = false
this.$nextTick(() => {
    this.createButtonLoading = true
})

//或者去掉loadding，添加关闭按钮
<!-- 添加小组、页面、按钮 -->
<Modal
    v-model="createPage"
    :title="modalTitle"
    :transfer="false"
    :mask-closable="false"
    @on-cancel="modalCancel('pageFormValidate')"
>
    <Form ref="pageFormValidate" :model="pageFormDatas" :rules="pageFormRule">
        <FormItem :label="addlabel" prop="moduleName" class="form-item">
            <Input
            v-model.trim="pageFormDatas.moduleName"
            :maxlength="32"
            @input="inputSync(pageFormDatas.moduleName)"
            @on-focus="isInputSync"
            />
        </FormItem>
        <FormItem label="权限标识" prop="moduleSign" class="form-item">
            <Input v-model.trim="pageFormDatas.moduleSign" :maxlength="32" />
        </FormItem>
        <FormItem v-if="addType === 1" label="地址链接" prop="moduleUrl" class="form-item">
            <Input v-model.trim="pageFormDatas.moduleUrl" :maxlength="32" />
        </FormItem>
        <FormItem v-if="addType !== 2" label="显示名称" prop="label" class="form-item labelBlur">
            <Input v-model.trim="pageFormDatas.label" :maxlength="32" />
        </FormItem>
        <FormItem v-if="addType !== 2" label="状态" prop="status" class="form-item">
            <i-switch v-model="pageFormDatas.status" size="large">
            <span slot="open">显示</span>
            <span slot="close">隐藏</span>
            </i-switch>
        </FormItem>
        <Checkbox v-model="pageContinueAdd">&nbsp;点击确定，提交内容并继续添加</Checkbox>
    </Form>
    <div slot="footer">
    <Button type="primary" @click="realCreatePage('pageFormValidate')">确定</Button>
    <Button type="default" @click="modalCancel('pageFormValidate')">取消</Button>
    </div>
</Modal>
```

12. 选项卡实时加载
    > TabPane 添加 name, 并加 v-if 判断

```js
<Tabs v-model="showTab" :left-range="20">
    <TabPane v-if="showDict" label="字典表" name="dict">
        <dict-tab v-if="showTab === 'dict'" />
    </TabPane>
    <TabPane label="LDAP集成" name="ldap">
        <ladp v-if="showTab === 'ldap'" :urls="defaultUrl" />
    </TabPane>
    <TabPane label="通知服务" name="message">
        <MessageService v-if="showTab === 'message'" :urls="defaultUrl" />
    </TabPane>
</Tabs>
```

```js
this.$Message.error(msg);
this.createButtonLoading = false;
this.$nextTick(() => {
  this.createButtonLoading = true;
});
```

13. 表格自适应

```js
<div ref="tableMain" class="table-main" style="height: calc(100% - 58px)">
    <UinoTable :columns="columns" :data="datas" :max-height="tableHeight" column-control border>
    <template v-slot:enable="{ row }">
        <i-switch v-model="row.enable" size="small" @on-change="resetEnable(row)"> </i-switch>
    </template>
    <template v-slot:acts="{ row }">
        <Button v-if="!isChildClass" type="text" :disabled="row.enable" @click="edit(row)">
        {{ 'COMMON_EDIT' | L }}
        </Button>
        <Button type="text" @click="remove(row)">{{ 'COMMON_DELETE' | L }}</Button>
    </template>
    </UinoTable>
</div>

data() {
    return {
      tableHeight: 500
    }
}
mounted() {
    this.resizeTableHeight()
    window.addEventListener('resize', this.resizeTableHeight)
},
beforeDestroy() {
    window.removeEventListener('resize', this.resizeTableHeight)
},
methods: {
    resizeTableHeight() {
        this.tableHeight = this.$refs['tableMain'] ? this.$refs['tableMain'].offsetHeight : 500
    }
}
```

14. select 默认选项

> 本地静态数据 ID 小的默认选中，动态数据需要手动设置选中项目（在没有 id 的情况下）

18. eslintrc 自动修复

```js
npm run lint --fix
```

19. 单元测试

```js
npm run unit
```

## 字体图标

```js
// 项目里面有2种方式引入

// 1.组件包的形式

// 2.项目里直接引入在assets下或者public下

// 注意：页面引入字体图标class时候一定要加入对应包的class，在iconfont.css里面
```

## 文件上传

```js
<Upload
    v-if="ignore3d === false"
    :action="import3DImage"
    :format="['doc', 'docx', 'xls', 'xlsx', 'ppt', 'pptx', 'pdf']"
    :accept="uploadAcceptFormat"
    :max-size="1024 * 100"
    :show-upload-list="false"
    :before-upload="uploadFileBefore"
    :on-progress="uploadFileProgress"
    :on-success="event => uploadDataFileSuccess(0, event)"
    :on-error="uploadFileError"
    :on-format-error="uploadFileFormatError"
    :headers="httpHeaders"
    :on-exceeded-size="onExceededSize"
>
</Upload>

uploadAcceptFormat: 'application/x-zip-compressed',
uploadAcceptFormat: '.doc, .docx, .xls, .xlsx, .ppt, .pptx, .pdf',
// 文件大小限制
onExceededSize(error) {
    error && this.$Message.error('文件大小不能大于100M')
    this.showUploadPercent = false
},
```

[input file accept 指定文件类型](https://blog.csdn.net/Missying55/article/details/113342253)

## 范围筛选

```js
  drawLine(ctx, startX, startY, endX, endY) {
      ctx.beginPath()
      ctx.moveTo(startX, startY)
      ctx.lineTo(endX, endY)
      ctx.stroke()
    },

  cardList.forEach((self, index) => {
          const x = self.offsetLeft
          const y = self.offsetTop
          const curItemHeight = self.clientHeight
          if (index === 0 || index === cardList.length - 1) {
            this.drawLine(ctx, x2, y2, x2, y + curItemHeight / 2)
          }
          this.drawLine(ctx, x2, y + curItemHeight / 2, x - 5, y + curItemHeight / 2)
        })

    //横线的位置由首尾第一个块的y2和所有的y1,y2决定
```
