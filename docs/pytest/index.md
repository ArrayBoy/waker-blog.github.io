## macos 安装

#### python3 安装

> 语言

[下载地址](https://www.python.org/downloads/release/python-3105/)

#### pip 安装[参考](https://blog.csdn.net/weixin_42397937/article/details/123325716)

```js
sudo curl https://bootstrap.pypa.io/get-pip.py | python3
```

#### pip 更新

```js
pip install --upgrade pip
```

#### PyCharm 安装

> ide

[下载地址](https://www.jetbrains.com.cn/pycharm/download/#section=mac)

#### 克隆项目

```javascript
git clone git@git.uino.com:uino-base/uino-micro-base-auto-test.git
```

#### 导入项目

```javascript
next;
```

### 安装 jdk

[下载地址](https://www.oracle.com/java/technologies/downloads/#jdk18-mac)

#### 安装依赖(requirements.txt)

```javascript
//安装pytest
pip install pytest

//安装requests
pip install requests

//安装selenium
pip install selenium

//安装pytest
pip install pytest

//安装xlrd
pip install xlrd

//安装pyperclip
pip install pyperclip

//pytest-html
pip install pytest-html

//yaml 安装--pip install yaml不行
pip install PyYAML

pytest==7.1.2
requests==2.27.1
selenium==4.3.0
xlrd==2.0.1
pyperclip==1.8.2
pywin32==304
allure-pytest==2.9.45
allure-python-commons==2.9.45
PyYAML~=6.0
pytest-sugar==0.9.5
pytest-tldr==0.2.4
pytest-html==3.1.1

```

#### Allure 报告插件

1. [下载 allure](https://github.com/allure-framework/allure2/releases)
2. 解压下载的 allure 包放在任意目录下
3. 获取 allure 文件夹的 bin 文件夹路径。把 bin 拖到 terminal，即可获取 path
4. 配置 macos 系统的环境变量
   > 需要修改.zprofile 文件，在/Users/用户名字/.zprofile 路径下。使用 common+shift+.显示隐藏文件,把文件拖到编辑器中，进行添加 allure 的 bin 文件夹的 path

```js
PATH="/Applications/my-project/allure-2.18.12/bin:${PATH}"
export PATH

vi ~/.zprofile  //打开文件添加上面path

```

#### vim 常用命令

> Vim 是 Unix 及类 Unix 系统文本编辑器

```js
1. :wq——保存并退出Vim编辑器

2. :wq!——保存并强制退出Vim编辑器

3. :q——不保存就退出Vim编辑器

4. :qa!——不保存就强制退出Vim编辑器

5. :w!——强制保存文本

6. :w filename——另存到filename文件

7. x!——保存文本，并退出Vim编辑器

8. ZZ——直接退出Vim编辑器
```

#### 下载浏览器驱动

![](../media/chrome-v.jpg)

[下载地址](https://chromedriver.chromium.org/home)

1. 把解压文件(.nulix)放到项目 driver 目录下
2. 修改文件

   ```python
    /uino-micro-base-auto-test/util/util.py

   if driver is None and platform.system() == "Darwin":
       driver_path = os.path.abspath(os.path.dirname(os.path.dirname(__file__))) + '/driver/' + readConfig.read(
           'base-conf', 'driver_macos_name')
       driver = webdriver.Chrome(driver_path, options=chrome_options)
       driver.get(getconfigvalue('url'))
       driver.maximize_window()
       sleep(3)
       gl.set_value("driver", driver)
       log.info(">>> 获取driver成功")

   ```

   ```python
   /uino-micro-base-auto-test/config/CommTest.ini

   增加
   driver_macos_name=chromedriver
   ```

## 常用语法

```python
# 取值--获取某个元素的属性值
# eg 获取input的value值
ele.get_attribute("value")

# 断言
CHECK_SEND_RESULT_STR = "//span[contains(text(), '{}')]"
self.assert_exist_ele(*(By.XPATH, self.CHECK_SEND_RESULT_STR.format('请选择文件')))

def date_selector_abnormal_by_input(self, label_name, time_str):
    """
    告警时间异常校验
    @param label_name: 标签名称
    @param time_str: 输入内容
    @return:
    """
    with allure.step("输入"+str(time_str)):
        blank_xpath = "//label[contains(text(), '模拟内容')]"
        self.input_text_by_clear(time_str, *(By.XPATH, self.FORM_DATE_TEMPLATE_STR.format(label_name)))
        sleep(1)
        input_new_val = self.get_element(*(By.XPATH, self.FORM_DATE_TEMPLATE_STR.format(label_name))).get_attribute("value")
        self.click(*(By.XPATH, blank_xpath))
        sleep(1)
        input_old_val = self.get_element(*(By.XPATH, self.FORM_DATE_TEMPLATE_STR.format(label_name))).get_attribute("value")
        pytest.assume(input_new_val != input_old_val)

# 判断
if driver is None and platform.system() == "Linux":
    chrome_capabilities = {
        "browserName": "chrome",
        "version": "",
        "platform": "windows",
        "javascriptEnabled": True
    }
    driver = webdriver.Remote('http://10.100.31.218:4444/wd/hub', desired_capabilities=chrome_capabilities)
    driver.maximize_window()
    driver.get(getconfigvalue('url'))
    sleep(3)
    gl.set_value("driver", driver)

# 遍历
for item in items:
    for cls_name in appoint_classes:
        if item.parent.name == cls_name:
            item.name = item.name.encode("utf-8").decode("unicode_escape")
            item._nodeid = item.nodeid.encode("utf-8").decode("unicode_escape")
            appoint_classes[cls_name].append(item)
items.clear()
for cases in appoint_classes.values():
    items.extend(cases)

# 异常捕获
def pytest_runtest_makereport(item, call):
    try:
        outcome = yield
        rep = outcome.get_result()
        driver = gl.get_value("driver")
        if rep.when == "call" and rep.failed:
            allure.attach(driver.get_screenshot_as_png(), 'error-png', allure.attachment_type.PNG)
    except Exception as err:
        log.error(err)

# 事件
LDAP_CREATE_BUTTON = (By.XPATH, LDAP_MENU_PREFIX + "//div[@class='btn-add']")
self.click(*self.LDAP_CREATE_BUTTON)

DEL_BATCH_UPLOAD_FILE = (By.XPATH, "//ul[@class='ivu-upload-list']//li//i[contains(@class,'remove')]")
self.move_mouse_to_element_click(*self.DEL_BATCH_UPLOAD_FILE)
```
