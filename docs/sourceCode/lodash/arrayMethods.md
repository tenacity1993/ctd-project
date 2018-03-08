# Array Methods

### 						——lodash源码阅读学习（v4.17.4）

####1. chunk

> Creates an array of elements split into groups the length of `size`. If `array` can't be split evenly, the final chunk will be the remaining elements.

看到这个功能解释的时候我写的代码： 

```javascript
/**
 * lodash chunk
 * 
 * @param {Array} array 
 * @param {Array} size 
 * @returns {Array}
 */
function chunk(array, size) {
    let result = []
    let len = array.length
    let num = array.length / size
    let pos = 0
    if (!array || size < 1) return []
    if (size === 1) {
        array.forEach(item => {
            result.push(item)
        })
    } else {
        for (let i = 0; i < num - 1; i++) {
            let tmp = []
            for (let j = 0; j < size; j++) {
                tmp.push(array[i * size + j])
                pos++
            }
            result.push(tmp)
        }
        result.push(array.slice(pos - len))
    }
    
    return result
}

```

其中我原来写的是size == 0， 看了源码之后才改成了size < 0，大致理解了这个函数需要实现的功能。

再来看下源码，源码中依赖了`slice`函数，先来看下`slice`函数。

函数源码中有下面这样一句提示：

**Note:** This method is used instead of [`Array#slice`](https://mdn.io/Array/slice) to ensure dense arrays are returned.（确保返回密集数组）

- 关于稀疏数组和密集数组，参考下文章 [sparse arrays vs. dense arrays](http://2ality.com/2012/06/dense-arrays.html)

  意思是说：

  - 稀疏数组

    ```javascript
    var a = new Array(3) // 这里a是稀疏数组，console.log结果为 empty*3 全部为空
    a.length //3
    a[0]  //undefined
    // 当遍历该数组时，javascript会直接跳过数组
    a.forEach(function (x, i) { console.log(i+". "+x) }); // undefined
    a.map(function (x, i) { return i }) // [empty × 3]  也为空
    ```

  - 密集数组

    ```javascript
    var a = Array.apply(null, Array(3));
    a //[undefined, undefined, undefined]
    //类似于下面这样
    var a = Array(undefined, undefined, undefined)
    a //[undefined, undefined, undefined]

    ```

  - 相同和不同

    某些操作两者是一样的：

    ```javascript
    a[0]  // undefined 
    a.length //3
    ```

    但是，在密集数组中可以实现对元素的遍历，js并不会跳过。

    ```javascript
    a.forEach(function(x, i) {
      console.log(i + '. ' + x)
    })
    // 0. undefined
    // 1. undefined
    // 2. undefined
    a.map(function(x, i) {
      return i
    })
    //[0, 1, 2]
    ```

- _.slice

  - 与原生slice方法相比：
    - 能够确保返回密集数组

    - 效率更高 [性能测试展示](https://jsperf.com/slice-vs-slice)

      效果图

      ![性能对比(Ops/sec)](/Users/ctd/Pictures/Snip20180110_5.png)

  - 源码

    ```javascript
    /**
     * Creates a slice of `array` from `start` up to, but not including, `end`.
     *
     * **Note:** This method is used instead of
     * [`Array#slice`](https://mdn.io/Array/slice) to ensure dense arrays are
     * returned.
     *
     * @since 3.0.0
     * @category Array
     * @param {Array} array The array to slice.
     * @param {number} [start=0] The start position.
     * @param {number} [end=array.length] The end position.
     * @returns {Array} Returns the slice of `array`.
     */
    function slice(array, start, end) {
      #1
      let length = array == null ? 0 : array.length
      if (!length) {
        return []
      }
      start = start == null ? 0 : start
      end = end === undefined ? length : end
      #2
      if (start < 0) {
        start = -start > length ? 0 : (length + start)
      }
      end = end > length ? length : end
      if (end < 0) {
        end += length
      }
      #3
      length = start > end ? 0 : ((end - start) >>> 0)
      start >>>= 0
      #4
      let index = -1
      const result = new Array(length)
      while (++index < length) {
        result[index] = array[index + start]
      }
      return result
    }

    export default slice

    ```

    针对#1的代码，确保length、start、end都取值正确。如果数组为空，将长度置为0，同时为start和end设置默认值。

    针对#2的代码，如果start<0，从后往前取数据。

    针对#3的代码，要理解在js中>>>的含义。  

    ​	`>>>` 表示无符号右移，丢弃被移出的位，并使用 0 在左侧填充。`>>>0`将该变量转化成无符号的32位整数。

    针对#4的代码，我写了个demo，以前没有注意到这种区别，结果可以保证都是密集数组，即便是空，也可以以undefined的形式返回。

    ```javascript
    var a = new Array(3)
    var b = [], index = -1
    while(++index < a.length) {
        b[index] = a[index]
    }
    console.log(b)
    // [undefined, undefined, undefined]
    ```

- _.chunk

  ```javascript
  import slice from './slice.js'

  /**
   * Creates an array of elements split into groups the length of `size`.
   * If `array` can't be split evenly, the final chunk will be the remaining
   * elements.
   *
   * @since 3.0.0
   * @category Array
   * @param {Array} array The array to process.
   * @param {number} [size=1] The length of each chunk
   * @returns {Array} Returns the new array of chunks.
   * @example
   *
   * chunk(['a', 'b', 'c', 'd'], 2)
   * // => [['a', 'b'], ['c', 'd']]
   *
   * chunk(['a', 'b', 'c', 'd'], 3)
   * // => [['a', 'b', 'c'], ['d']]
   */
  function chunk(array, size) {
    size = Math.max(size, 0)
    const length = array == null ? 0 : array.length
    if (!length || size < 1) {
      return []
    }
    let index = 0
    let resIndex = 0
    const result = new Array(Math.ceil(length / size))

    while (index < length) {
      result[resIndex++] = slice(array, index, (index += size))
    }
    return result
  }

  export default chunk

  ```

  - Math.ceil()向上取整，result中存储最终结果数组中元素的个数，就是最终分组的组的个数。
  - 通过对比当前分组起始的元素的index位置与原数组的长度，判断是否分组完成，然后按照size的大小，将元素进行切片