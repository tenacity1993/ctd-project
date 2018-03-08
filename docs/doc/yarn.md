## yarn

2017-12-20

####1. 功能差异

- yarn.lock 文件中记录了添加模块的精确的版本号，这样可以确保另一个位置安装了相同版本的包，并且在package.json中仍然保留着一个允许的版本段
- 在npm里shrinkwrap命令也会产生lock文件，然后npm在读取package.json之前会读取该文件，类似于yarn读取yarn.lock。不同的是**yarn总是会自动更新yarn.lock文件，而npm需要手动维护**
- npm安装包是顺序执行，yarn则为并行安装，速度更快
- yarn输出更加简洁

#### 2. cli差异

- yarn的全局操作需要使用global，不能使用-g或者—global。与npm类似项目相关的依赖不应该被全局安装。`global`前缀只能跟yarn add, yarn bin（displays the location of the yarn `bin` folder）, yarn ls, yarn remove工作，除了yarn add以外，其他命令与npm的对应命令完全一样。

- Yarn install 从package.json中安装依赖，并允许添加新的包。yarn install 只能安装在yarn.lock或者package.json中列举出来的依赖。

- yarn add [-dev] 添加依赖类似与 npm中的`—save`  ，yarn的`—dev`类似于`--save-dev`

- yarn why 该命令可以深入剖析依赖图，找出安装相应包的原因，可能是显示添加，也可能是某个包的依赖。

- yarn upgrade 将包升级到package.json**规则指定的版本范围的最新版**，而不是yarn.lock所定义的完全一致的版本。要想在npm中达成一致的效果，需要运行下面的命令：

  ```shell
  rm -rf node_modules
  npm install
  ```

  不要与npm update混淆，npm update会升级所有的包到最新的版本。

  ​

  ​

### yarn npm homebrew

1. brew: installation of software
2. npm: installation of packages
3. yarn: installation of packages 