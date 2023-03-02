<template>
  <transition name="fade">
    <div>
      <h2>情報を{{ getSaveLabel }}しますか？</h2>
      
      <div class="mt-4 flex buttonWrapper">
        <button @click="executeClose" class="commonButton">キャンセル</button>
        <button 
          class="commonButton"
          @click="executeSave"
        >
          {{ getSaveLabel }}
        </button>
      </div>

    </div>
  </transition>
</template>

<script lang="ts">
export default {
  props: {
    eventType: String,
    isStack: Boolean,
    isStackEdit: Boolean,
  },
  data() {
    return {
      saveLabel: {
        post: "登録",
        put: "更新",
      }
    }
  },

  computed: {
    isShowDetail() {
      return false
    },

    getSaveLabel(): string {
      if (this.isStackEdit) {
        return this.saveLabel.put
      } else {
        return this.saveLabel.post
      }
    },

  },

  methods: {
    executeSave() {
      // emitで親の`deleteConfirmDialog`を呼び出す
      alert(`${this.getSaveLabel}しました`)

      // 「削除しました」のメッセージ表示後、ダイアログを閉じる
      this.executeClose()

      // スタック・フォームを閉じる処理
      if (this.isStack) {
        this.$emit("parent-event2")
      }
    },

    executeClose() {
      // 親側のメソッドを呼び出す。ここの引数でfalseを指定する場合は、親側の`this.shouldDialog`をfalseにする
      // `parent-event2`は、親側で子を呼び出す箇所で使用する`@parent-event2="closeDialog"`で 記述されている
      this.$emit("parent-event", false)
    },
  },
}
</script>

<style lang="scss">

</style>
