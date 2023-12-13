![[Pasted image 20231130095242.png]]



假如现在我们有这么几个项目，并且采用`PolyRepo` 的模式去进行代码管理:

- adminApp – 后台管理应用
- h5App – h5移动端应用
- shared-utils – 公共方法，供adminApp以及h5App 进行调用。

其中`shared-utils`作为一个`npm`包，其它项目通过`npm install`进行安装使用。

如果我们发现`shared-utils`中的一个方法有Bug，我们进行修复的时候大致步骤如下：

1. 在`shared-utils` 代码仓库中进行修复以及提交。
2. 更新`shared-utils`的包版本，发布到`npm`上
3. 在`adminApp`中更新`shared-utils`包版本，并进行构建发布。
4. 在`h5App`中更新`shared-utils`包版本，并进行构建发布。

如此几个步骤下来，修复错误所需的时间会比较长，如果实际的业务应用不止两个，那么这个问题完全修复的时间则更加长。





