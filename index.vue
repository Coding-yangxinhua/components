<template>
  <div class="home-container">
    <van-field class="inputTest" v-model="inputValue" placeholder="这是一个输入框" @blur="resetPos"/>
    <van-button color="#7232dd" plain @click="showEmojiBox = !showEmojiBox">emoji</van-button>
    <emoji-tool-bar :showEmojiBox="this.showEmojiBox"
                    @selectEmoji="selectEmoji"
                    @deleteEmoji="deleteEmoji">
    </emoji-tool-bar>
  </div>
</template>

<script>

import EmojiToolBar from '../../components/EmojiToolBar'
export default {
  components: {
    EmojiToolBar
  },
  data () {
    return {
      inputValue: '',
      showEmojiBox: false,
      inputPos: 0
    }
  },
  methods: {
    // 当输入框失去焦点时，获取输入框光标位置
    resetPos (e) {
      this.inputPos = e.srcElement.selectionStart
    },
    selectEmoji (value) {
      const tempInput = this.inputValue
      const tempPos = this.inputPos
      if (this.inputPos == null) {
        this.inputValue += value
        return
      }
      this.inputValue = tempInput.slice(0, tempPos) + value + tempInput.slice(tempPos, tempInput.length)
      this.inputPos += value.length
    },
    deleteEmoji () {
      const tempInput = this.inputValue
      const tempPos = this.inputPos === 1 ? 2 : this.inputPos
      // 若位置为0，则不执行删除
      if (this.inputPos === 0) {
        return
      }
      // 避免切割时造成表情变成?
      const preElementLength = tempInput[tempPos - 2].codePointAt(0) === 55357 ? 2 : 1
      // 分割并拼接字符串
      this.inputValue = tempInput.slice(0, this.inputPos - preElementLength) + tempInput.slice(this.inputPos)
      // 改变指向位置
      this.inputPos -= preElementLength
    }
  }
}
</script>

<style scoped lang="scss">
.inputTest {
  font-size: 22px;
  line-height: 45px;
}
</style>
