<template>
  <div>
    <div class="overlay">

    </div>
    <div class="detailStack">
      <h2>詳細</h2>

      <div class="buttonWrapper">
        <button @click="closeStack" class="commonButton">
          閉じる
        </button>

        <button 
          v-if="isFormEdit" 
          @click="executeConfirmDialog('save')" 
          class="commonButton"
        >
          更新する
        </button>
        <button 
          v-else 
          @click="executeConfirmDialog('save')" 
          class="commonButton"
        >
          登録する
        </button>
      </div>

      <div v-if="isFormEdit">
        <p> {{ item.id }} </p>
        <p> {{ item }} </p>
      </div>

      <form class="mt-4" action="">
        <div>
          名前：
          <input type="text" v-model="item.name">
        </div>
        <div class="mt-4">
          年齢：
          <input type="text" v-model="item.age">
        </div>
        <div v-if="isFormEdit" class="mt-4">
          プロフィール画像
          <div class="imgWrapper">
            <img :src="`${item.imgSrc}`" alt="">
          </div>
        </div>

      </form>

      <div v-if="isFormEdit" class="buttonWrapper mt-4">
        <button 
          class="commonButton mr-2"
          @click="executeConfirmDialog('delete')"
        >削除する</button>
      </div>

      <teleport to="body">
        <div v-if="getShouldDialog" class="modal boxShadow">
          <div>
            <p>モーダルウィンドウ・ダイアログ</p>

            <!-- 子コンポーネントに、親コンポーネントのメソッド`confirmDialog`を使わせてあげる -->
            <StringCheckDialog 
              v-if="eventType === 'delete'"
              :event-type="eventType"
              :is-stack="true"
              @parent-event="closeDialog"
              @parent-event2="closeStack"
            />

            <NormalDialog 
              v-if="eventType === 'save'"
              :event-type="eventType"
              :is-stack-edit="isStackEdit"
              :is-stack="true"
              @parent-event="closeDialog"
              @parent-event2="closeStack"
            />
          </div>
        </div>
      </teleport>
    </div>
  </div>
</template>

<script>
// import { Item } from "~~/types/common"

export default {
  props: {
    isFormEdit: Boolean,
    item: Object,
  },
  data() {
    return {
      dataOptionData: "",
      shouldDialog: false,
      eventName: "default",
      eventType: "",
    }
  },
  computed: {
    getShouldDialog() {
      return this.shouldDialog
    },

    isStackEdit() {
      return this.isFormEdit
    },

  },

  // mounted() {
  //   console.log("item: ", this.item)
  // },
  watch: {
    itemId: function(newVal, oldVal) {
      // データの値が変化した時にコンソールに新しい値と古い値を出力
      this.dataOptionData = newVal
    },
  },
  methods: {
    executeIsFormEditFalse() {
      this.$emit("parent-event", false)
    },

    confirmDialog() {
      this.shouldDialog = true
    },

    closeDialog() {
      this.shouldDialog = false
    },

    closeStack() {
      this.$emit("parent-event2", false)
    },

    executeConfirmDialog(eventType) {
      // deleteなどの文字列をセットする処理を入れる
      this.eventType = eventType
      this.confirmDialog(true)

      // 入力内容からRequestを作成
      const data = {

      }
    },
  }
  
}
</script>

<style lang="scss">
.detailStack {
  position: absolute;
  top: 0;
  right: 0;

  width: 70%;
  background-color: #fff;
  // opacity: 0.95;
  z-index: 2;
  height: 100vh;
  padding: 1rem;
  border-left: 1px solid #000;

  min-height: calc(90vh + 160px);

  .imgWrapper {
    width: 50%;
  }
}
</style>