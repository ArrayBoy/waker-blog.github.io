<!--
 * @Author: yanglilong yanglilong@uino.com
 * @Date: 2022-08-09 14:43:48
 * @LastEditors: yanglilong yanglilong@uino.com
 * @LastEditTime: 2022-09-06 15:15:50
 * @FilePath: /blog/docs/pytest/note.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->

#### MacOS 系统下 python 实现自动上传文件操作

> 原理：打开选择文件后，通过文件系统查找对应的文件，依赖 PyKeyboard，PyMouse

```js
// install

pip install PyKeyboard

pip install PyMouse

// 然后需要修改文件

// /Applications/my-project/uino-micro-base-auto-test/venv/lib/python3.10/site-packages/pykeyboard/__init__.py


if sys.platform.startswith('java'):
    from .java_ import PyKeyboard

elif sys.platform == 'darwin':
    from .mac import PyKeyboard, PyKeyboardEvent

elif sys.platform == 'win32':
    from .windows import PyKeyboard, PyKeyboardEvent

else:
    from .x11 import PyKeyboard, PyKeyboardEvent

// /Applications/my-project/uino-micro-base-auto-test/venv/lib/python3.10/site-packages/pymouse/__init__.py


if sys.platform.startswith('java'):
    from .java_ import PyMouse

elif sys.platform == 'darwin':
    from .mac import PyMouse, PyMouseEvent

elif sys.platform == 'win32':
    from .windows import PyMouse, PyMouseEvent

else:
    from .unix import PyMouse, PyMouseEvent

```

```js
// 方法
from time import sleep
from pykeyboard import PyKeyboard
from pymouse import PyMouse
import pyperclip

def upload_file(filePath):
    # fileAbsPath = os.path.abspath(os.path.dirname(os.path.dirname(__file__))) + filePath
    if platform.system() == "Darwin":  # platform.system()判断操作系统
        # 创建鼠标对象
        k = PyKeyboard()
        m = PyMouse()
        k.press_keys(["Command", "Shift", "G"])
        x_dim, y_dim = m.screen_size()
        m.click(x_dim // 2, y_dim // 2, 1)
        # 输入文件全路径进去
        k.type_string(filePath)
        sleep(2)
        k.press_key("Return")
        sleep(1)
        k.press_key("Return")
        sleep(2)

def importFile(self, importFile):
    self.click(*self.ADD_AREA_SELECT)
    importName=(By.XPATH, "//li[contains(text(),'导入')]")
    self.click(*importName)
    sleep(2)
    upload = (By.XPATH, "//span[contains(text(), '点击上传')]")
    sleep(2)
    self.click(*upload)
    util.upload_file(importFile)
```

#### 替代方案

> 直接下载 pyuserinput,包里面保护了 PyKeyboard 和 PyMouse，且不用修改**init**.py 文件

```js
pip install pyuserinput
```

> macos 不能回车解决

```js
def upload_file(filePath):
    fileAbsPath = os.path.abspath(os.path.dirname(os.path.dirname(__file__))) + filePath
    # 创建键盘对象
    pyk = PyKeyboard()
    if platform.system() == "Darwin":  # mac系统
        pyperclip.copy('/' + fileAbsPath)
        pyk.press_keys(["Command", "Shift", "G"])
        pyk.press_keys(["Command", "V"])
        # sleep(2)
        pynput_keyboard = Controller()
        pynput_keyboard.press(Key.enter)
        pynput_keyboard.release(Key.enter)
        sleep(1)
        pynput_keyboard.press(Key.enter)
        pynput_keyboard.release(Key.enter)
    if platform.system() == "Windows":  # windows系统

        pyperclip.copy(fileAbsPath)
        # 输入文件全路径进去（使用键盘操作ctrl+v）
        # pyk.type_string(fileAbsPath)
        sleep(1)
        pyk.press_key(pyk.control_key)
        pyk.tap_key('v')
        pyk.release_key(pyk.control_key)
        sleep(1)
        pyk.press_key(pyk.enter_key)

```

#### 如果打不开需要设置安全性与隐私-》完全磁盘访问权限-》添加 python 执行程序-》保存

## 语法相关

[xpath](https://www.byhy.net/tut/auto/selenium/xpath_1/#%E7%BB%84%E9%80%89%E6%8B%A9%E7%88%B6%E8%8A%82%E7%82%B9%E5%85%84%E5%BC%9F%E8%8A%82%E7%82%B9)

```python
1. util.get_yaml_value 返回单值

2. util.get_yaml_values 返回list

3. 常用
DELETE_LAYER = (By.XPATH, " ")
self.click(*self.DELETE_LAYER)

SEARCH_LAYER_STR = "//div[@class='list-wrapper']//span[contains(text(), '{}')]"
self.assert_exist_ele(*(By.XPATH, self.SEARCH_LAYER_STR.format(modify_name)))
self.assert_not_exist_ele(*(By.XPATH, self.SEARCH_LAYER_STR.format(modify_name)))

STOP_PLUGIN_BTN_STR = "//div[@class='table-wrap']//div[@class='ivu-table-body']//span[contains(text(),'{}')]/../../..//span[contains(text(),'停用')]"
self.click(By.XPATH, self.STOP_PLUGIN_BTN_STR.format(plugin_name))
self.move_mouse_to_element_click(By.XPATH, self.DELETE_PLUGIN_BTN_STR.format(modify_plugin_name))

START_PLUGIN_BTN_STR = "//div[@class='table-wrap']//div[@class='ivu-table-body']//span[contains(text(),'{}')]/../../..//span[contains(text(),'启用')]"

SIBLINGS = "//div[@class='uino-tree-node'][last()]//span[contains(text(), '树状图管理层级2')]/following-sibling::span"

SELECTED_LAYER_STR = "//div[@class='uino-tree-node'][last()]//span[contains(text(), '树状图管理层级2')]/.."
```
