## 删除选中模型或者线

```js
this.graph.diagram.selection.all((nodeOrLink) => {
  window.graph.remove(nodeOrLink);
});

// 对选中对象回调处理
this.graph.diagram.selection.all((nodeOrLink) => {
  // 当是线时
  if (nodeOrLink.Tg === "linkLayer") {
    _t.previewShow = false;
    return true;
  } else {
    if (
      (_t.hasSaveRuleData &&
        _t.hasSaveRuleData.pageNodeId === nodeOrLink.key) ||
      nodeOrLink.key === 0
    ) {
      this.$Message.error(_t.$L.get("CMV_NOT_ALLOW_DELETE_ROOT_NODE"));
      return false;
    } else {
      _t.previewShow = false;
      return true;
    }
  }
});
```
