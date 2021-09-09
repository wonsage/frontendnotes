# JavaScript 算法：二叉树

## 

二叉树（Binary Tree）是一种具有层级特性的数据结构。

根节点

中间节点

叶子节点

二叉树的高：二叉树节点的层数

排序二叉树：左子节点的值小于父节点的，右子节点的值大于父节点的

## 根的生成和节点插入方法

```js
function BinaryTree() {									// 整个二叉树构造函数
  let Node = function (key) {		// 节点的构造函数
    this.key = key		// 关键值
    this.right = null	// 右子节点
    this.left = null	// 左子节点
  }

  let root = null

  let insertNode = function (node, newNode) {		// 新节点插入到某现有节点旁的方法
    if (newNode.key < node.key) {				// 比较大小
      if (node.left === null) {					// 如果没有左子节点
        node.left = newNode							// 则新节点直接作为左子节点
      } else {													// 如果有左子节点
        insertNode(node.left, newNode)	// 则嵌套自调用，将新节点插入左节点旁
      }
    } else {
      if (node.right === null) {
        node.right = newNode
      } else {
        insertNode(node.right, newNode)
      }
    }
  }

  this.insert = function (key) {		// 二叉树实例的插入方法
    let newNode = new Node(key);
    if (root === null) {
      root = newNode
    } else {
      insertNode(root, newNode)
    }
  }
}
// 下面是示例验证
let nodes = [8, 3, 10, 1, 6, 14, 4, 7, 13]
let binaryTree = new BinaryTree
nodes.forEach(function (key) {
  binaryTree.insert(key)
})
```

## 遍历

### 中序遍历

从根节点开始，对于每一个节点都是遍历其左子节点，碰到叶子节点即可打印。左子节点遍历完毕，打印该节点，之后再遍历右子节点。 （从左至右）

中序遍历二叉树得到的结果是从小到大的顺序排列。

```js
let inOrderTraversNode = function (node, callback) {	// 通用的中序遍历方法
  if (node !== null) {			// 只在节点非空时执行
    inOrderTraverseNode (node.left, callback)
    callback(node.key)
    inOrderTraverseNode (node.right, callback)
  }	// 具体的回调函数在构造函数外定义即可，根据具体需求在遍历时操作，比如是打印或推入新数组
}

this.inOrderTraverse = function (callback) {					// 实例的中序遍历方法
  inOrderTraverseNode (root, callback)								// 调用通用方法，从根节点开始
}
```

### 前序遍历

当前节点 → 左子节点 → 右子节点 （从根到叶 > 先左后右）

前序遍历可以用于复制一棵二叉树，它的效率比重新构造二叉树高。

```js
let preOrderTraverseNode = function (node, callback) {
  if (node !== null) {
    callback(node.key)
    preOrderTraverseNode (node.left, callback)
    preOrderTraverseNode(node.right, callback)
  }
}
this.preOrderTraverse = function (callback) {
  preOrderTraverseNode (root, callback)
}
```

### 后序遍历

左子节点 → 右子节点 → 当前节点 （从叶到根 > 先左后右）

```js
let postOrderTraverseNode = function (node, callback) {
  if (node !== null) {
    postOrderTraverseNode (node.left, callback)
    postOrderTraverseNode(node.right, callback)
    callback(node.key)
  }
}
this.postOrderTraverse = function (callback) {
  postOrderTraverseNode (root, callback)
}
```

三种遍历的区别在于对节点及其左右子节点的先后顺序

## 查找

### 查找最大或最小节点

最小节点：从根节点开始，直至找到没有左子节点的节点

最大节点：从根节点开始，直至找到没有右子节点的节点

```js
let minNode = function (node) {
  if (node) {
    while (node.left !== null) {
      node = node.left
    }
    return node // 这里也可以返回最小节点的值
  } else {
    return null
  }
}
this.min = function () {
  return minNode(root)
}
```

### 查找具体节点

从根节点开始，将待查节点与其比较，小于则与其右子节点比较，大于则与其左子节点比较，等于即找到，或直至叶节点仍未找到即不存在。

```js
let searchNode = function (node, key) {
  if (node === null) {
    return false	// 待查节点不存在，即返回 false
  }
  if (key < node.key) {
    return searchNode(node.left, key)		// 递归查找
  } else if (key > node.key) {
    return searchNode(node.right, key)
  } else {
    return true		// 存在 key === node.key 时，说明找到。
  }
}
this.search = function (key) {
  return searchNode(root, key)
}
```

## 删除节点

```js
let removeNode = function (node, key) {
  if (node === null) {
    return null		// 开门判断，待查节点不存在时，程序直接结束
  }
  if (key < node.key) {
    node.left = removeNode(node.left, key)		// 此处赋值是为了，删除后返回的节点重建与上级节点关系
    return node
  } else if (key > node.key) {								// 同上
    node.right = removeNode(node.right, key)
    return node
  } else {		// 当 key === node.key 时，
    if (node.left === null && node.right === null) {	// 待删节点是叶节点
      node = null																			// 直接将该节点赋 null 并返回
      return node
    }
    if (node.left === null) {					// 同下
      node = node.right
      return node
    } else if (node.right === null) {	// 待删节点只有一个子节点
      node = node.left								// 以其子节点替换它并返回
      return node
    }
    // 以上情况均不是，说明待删节点有左右子节点
    let aux = minNode(node.right)	// 此时需要找出待删节点的右子节点下的最小节点
    node.key = aux.key						// 以该最小节点的值替换待删节点的值，这样节点间关系不变
    node.right = removeNode(node.right, aux.key)	// 删除该最小节点
    return node																		// 最后返回c
  }
}
this.remove = function (key) {
  return removeNode(root, key)
}
```

