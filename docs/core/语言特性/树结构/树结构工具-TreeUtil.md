## 介绍

考虑到菜单等需求的普遍性，有用户提交了一个扩展性极好的树状结构实现。这种树状结构可以根据配置文件灵活的定义节点之间的关系，也能很好的兼容关系数据库中数据。实现

```
关系型数据库数据  <->  Tree  <->  JSON
```

树状结构中最大的问题就是关系问题，在数据库中，每条数据通过某个字段关联自己的父节点，每个业务中这个字段的名字都不同，如何解决这个问题呢？

PR的提供者提供了一种解决思路：自定义字段名，节点不再是一个bean，而是一个map，实现灵活的字段名定义。

## 使用

### 定义结构

我们假设要构建一个菜单，可以实现系统管理和店铺管理，菜单的样子如下：

```
系统管理
    |- 用户管理
    |- 添加用户

店铺管理
    |- 商品管理
    |- 添加商品
```

那这种结构如何保存在数据库中呢？一般是这样的：

| id  | parentId | name    | weight |
| --- | -------- | ------- | ------ |
| 1   | 0        | 系统管理 | 5      |
| 11  | 1        | 用户管理 | 10     |
| 111 | 1        | 用户添加 | 11     |
| 2   | 0        | 店铺管理 | 5      |
| 21  | 2        | 商品管理 | 10     |
| 221 | 2        | 添加添加 | 11     |

我们看到，每条数据根据`parentId`相互关联并表示层级关系，`parentId`在这里也叫外键。

### 构建Tree

```java
// 构建node列表
List<TreeNode<String>> nodeList = CollUtil.newArrayList();

nodeList.add(new TreeNode<>("1", "0", "系统管理", 5));
nodeList.add(new TreeNode<>("11", "1", "用户管理", 222222));
nodeList.add(new TreeNode<>("111", "11", "用户添加", 0));
nodeList.add(new TreeNode<>("2", "0", "店铺管理", 1));
nodeList.add(new TreeNode<>("21", "2", "商品管理", 44));
nodeList.add(new TreeNode<>("221", "2", "商品管理2", 2));
```

> TreeNode表示一个抽象的节点，也表示数据库中一行数据。
> 如果有其它数据，可以调用`setExtra`添加扩展字段。

```java
// 0表示最顶层的id是0
List<Tree<String>> treeList = TreeUtil.build(nodeList, "0");
```

因为两个Tree是平级的，再没有上层节点，因此为List。

### 自定义字段名

```java
//配置
TreeNodeConfig treeNodeConfig = new TreeNodeConfig();
// 自定义属性名 都要默认值的
treeNodeConfig.setWeightKey("order");
treeNodeConfig.setIdKey("rid");
// 最大递归深度
treeNodeConfig.setDeep(3);

//转换器
List<Tree<String>> treeNodes = TreeUtil.build(nodeList, "0", treeNodeConfig,
		(treeNode, tree) -> {
			tree.setId(treeNode.getId());
			tree.setParentId(treeNode.getParentId());
			tree.setWeight(treeNode.getWeight());
			tree.setName(treeNode.getName());
			// 扩展属性 ...
			tree.putExtra("extraField", 666);
			tree.putExtra("other", new Object());
		});
```

通过TreeNodeConfig我们可以自定义节点的名称、关系节点id名称，这样就可以和不同的数据库做对应。