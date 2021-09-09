## jQuery 节点操作

- 创建元素节点

  $('<p></p>')

  参数：创建的元素 HTML 代码，字符串。

#### 追加元素

- 将创建的元素节点添加到某一元素（父元素）下：
  - 父元素对象调用方法：

    $parent.append($child)、$parent.prepend($child)

    添加到父元素下最后或最前，参数是 jQuery 对象。

  - 子元素对象调用方法：

    $child.appendTo($parent)、$child.prependTo($parent)

    添加到父元素下最后或最前，参数可以是 jQuery 对象或字符串形式的 CSS 选择器。

- 将新创建元素节点添加到某一元素（兄弟元素）同级：
  - 目标位置元素对象调用方法：

    $brother.after($new)、$brother.before($new)

    在该元素后或前插入新元素

  - 新建元素对象调用方法：

  ​	$new.insertAfter($brother)、$new.insertBefore($brother)

  ​	将新元素插入到目标元素后面或前面，参数可以是 jQuery 对象或字符串形式的 CSS 选择器。

#### 删除元素

remove()

#### 清空元素

- empty() 清空调用者对象内部的所有元素即元素相关的事件。【常用】

- `$obj.html('')` 替换 HTML 内容为空，仅清空内部元素，不清理内存中的相关事件

#### 克隆元素

clone()

参数：布尔值。默认值：false

- true：克隆内容和事件
- false：仅克隆内容

返回值：克隆出来的新元素

## jQuery 操作元素的尺寸

### 元素的宽高

- width()、height()

  content 区域的宽高

  返回不带单位的宽高数值，也可传入参数来设置宽高

- innerWidth()、innerHeight()

  返回 content 和 padding 的宽高

  传参修改时，padding 不变，content 的宽高变化

- outerWidth()、outerHeight()

  返回 content、padding、border 的宽高

  传参修改时，padding、border 不变，content 的宽高变化

## jQuery 操作元素的位置

### 获取元素位置 #5

- offset()

  返回值：元素位置，一个包含 left、top 的对象

  ​	【注意】其参考对象始终是 document，与定位无关。

- position()

  返回值：以最近定位元素为参考的位置，一个包含 left、top 的对象

### 获取滚动距离

scrollTop()

返回滚动条距离顶部的距离

可以传数字修改

## 综合案例

- 固定导航
- 返回顶部
- 楼梯效果