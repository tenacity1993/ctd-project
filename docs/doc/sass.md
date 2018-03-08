## sass

- 存在scss和sass两种后缀的文件，其中`.sass`文件使用缩排语法，可以通过`sass-convert`命令行工具进行两种语法之间的转换

  ```shell
  sass-convert style.sass style.scss
  sass-convert style.scss style.sass
  ```

- 变量

  1. sass引入了变量，可以将反复使用的css属性值定义成变量然后通过变量名来引用。
  2. sass使用`$`来标识变量。中划线和下划线都支持，但是`$link-color` 和`link_color`指向的是同一个变量。
  3. *在sass中的大多数地方，中划线命名的内容和下划线命名的内容是互通的，出了变量，也包括对混合器和sass函数的命名，但是在sass中纯css部分不互通，比如类名、ID或属性名*

- css嵌套  

  1. sass父选择器  & ，当包含父选择器标识符的嵌套规则被打开时，它不会像后代选择器那样进行拼接，而是&被父选择器直接替换。

  2. 群组选择器的嵌套

  3. 使用>  选择直接子元素  + 同层相邻组合选择器 ～同层全体组合选择器

  4. 嵌套属性

     ```scss
     // 这种写法和下面效果一样
     nav {
       border: {
         style: solid;
         width: 1px;
         color: #ccc
       }
     }

     nav {
       border-style: solid;
       border-width: 1px;
       border-color: #ccc
     }

     // 对于属性的缩写形式，可以使用下面这种方式，指明例外规则
     nav {
       border: 1px solid #ccc {
         left: 0px;
         right: 0px;
         }
     }
     ```

- 导入sass文件

  1. css中存在@import规则，允许在一个css文件中导入其他的css文件。但是只有执行到@import时，浏览器才会去下载其他css文件，这导致页面加载起来特别慢

  2. sass的@import规则不同，它在生成css文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个css文件中，而无需发起额外的下载请求。另外，所有在被导入文件中定义的变量和混合器均可在导入文件中使用。

  3. 导入时可以省略sass或者scss后缀，可以随意修改导入样式文件的语法

  4. **局部文件**：当通过@import把css分散到多个文件时，某些文件并不需要生成对应的独立的css文件，这样的文件称为局部文件。约定：*局部文件的文件名以下划线开头*。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。使用@import时可以不写文件的全名（省略文件名开头的下划线）

  5. **变量默认值**  !default 如果变量被声明赋值了那么就用生命的值，否则就用默认值

  6. 嵌套导入  与原生css不同，sass允许@import命令写在css规则内，这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方。被导入的局部文件中定义的所有变量和混合器，也会在这个规则范围内生效。这些变量和混合器不会全局有效，这样可以通过嵌套导入只对站点中某一特定区域运用某种颜色主题或通过其他变量配置呃样式。

  7. 原生css的导入，尽管通常在sass中使用@import时，sass会尝试找到对应的sass文件导入进来，但是在下列三种情况下会生成原生的css@import，尽管这时会造成浏览器解析css时的额外下载。

     - 被导入文件的名字以.css结尾
     - 被导入文件的名字是一个url地址
     - 被导入文件的名字是css的url值

     这就是说，不能用sass的@import直接导入一个原始的css文件，因为sass会认为你想用css原生的@import。但是因为sass语法完全兼容css，所以可以把原始的css文件改名为.scss后缀，即可直接导入。

  8. 静默注释 //  单行注释  内容直到行末，其内容不会出现在生成的css文件中。

- 混合器

  1. 可以通过sass的混合器实现大段样式的重用

  2. 混合器使用@mixin标识符定义，然后在样式表中通过@include来使用这个混合器，放在需要使用的地方。

     ```scss
     @mixin rounded-corners {
       -moz-border-radius: 5px;
       -webkit-border-radius: 5px;
       border-radius: 5px;
     }

     .notice {
       background-color: green;
       border: 2px solid #00aa00;
       @include rounded-corners;
     }

     // sass最终生成

     .notice {
       background-color: green;
       border: 2px solid #00aa00;
       -moz-border-radius: 5px;
       -webkit-border-radius: 5px;
       border-radius: 5px;
     }
     ```

  3. 给混合器传参

     - 可以通过在@include混合器时给混合器传参来制定混合器生成的精确样式。

       ```scss
       @minxin link-colors($normal, $hover, $visited) {
         color: $normal;
         &:hover {color: $hover};
         &:visited {color: $visited};
       }

       a {
         @include link-colors(blue, red, green)
       }
       ```

     - sass允许通过语法$name:value的形式指定每个参数的值

       ```scss
       a {
         @iniclude link-colors {
           $normal: blue;
           $visited: green;
           $hover: red;
         }
       }
       ```

     - 默认参数值

       为了在@include混合器时不必传入所有的参数，可以给参数指定一个默认值。参数默认值使用`$name: default-value`的声明形式，默认值可以时任何有效的css属性值，甚至是其他参数的引用。

- 使用选择器继承来精简css

  选择器继承是说一个选择器可以继承另一个选择器定义的所有样式

  - @extend：

    ```scss
    .error {
      border: 1px red;
      background-color: #fdd;
    }

    .seriousError {
      @extend .error;
      border-width: 3px;
    }
    ```

    `.seriousError`不仅会继承`.error`自身所有样式，任何跟`.error`有关的组合选择器样式也会被以组合选择器的形式继承。

  - 继承和混合器并不一样，混合器主要是用于样式重用，继承则表明有类的归属的关系。

  - 关于@extend有两点：

    - 跟混合器相比，继承生成的css代码相对更少。因为继承仅仅是重复选择器，而不会重复属性，所以使用继承往往比混合器生成的css体积更小。
    - 继承遵从css层叠的规则，当两个不同的css规则应用到同一个html元素上时，并且这两个不同的css规则对统一属性的修饰存在不同的值，css层叠规则会决定应用哪个样式。**通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出。**
    - 注意：通常使用继承会让css美观整洁，因为继承只会在生成css时复制选择器，而不会复制大段的css属性。注意**不要在css规则中使用后代选择器去继承css规则**，这会使得css中选择器的数量失控，逻辑更加复杂，而且，sass并不总是会生成所有可能的选择器组合。

