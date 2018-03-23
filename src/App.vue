<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <!-- <router-view/> -->
    <ul v-for="(item, index) in items" :key="index" @click="onClickCallBack(item)">
      <li>{{item.text}}</li>
    </ul>

    <input type="text" @input="onInput" @change="onChange">
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      items: [
        {id: 1, text: '1-abc'},
        {id: 2, text: '2-abc'},
        {id: 3, text: '3-abc'}
      ]
    }
  },
  methods: {
    onClickCallBack (item) {
      alert(item.id)
    },
    // 只要操作内容，不论是增加还是删除，或者有输入法，都会触发事件
    onInput () {
      console.log(1)
    },
    // change事件只有在内容发生改变的情况下才会触发
    // 就算增加字符再删去相应字符，这种操作依然不会触发change事件，可以理解为输入完成之后敲击回车且内容发生改变之后才会触发）
    onChange () {
      console.log(2)
    }
  }
}
</script>

<style>
</style>
