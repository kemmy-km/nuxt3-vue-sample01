<template>
  <div>
    <div class="overlay">

    </div>
    <div class="detailStack">
      <h2>詳細</h2>

      <div class="buttonWrapper">
        <button @click="executeClose" class="commonButton">
          閉じる
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
      </form>

      <div v-if="isFormEdit" class="buttonWrapper mt-4">
        <button 
          class="commonButton mr-2"
          @click="executeConfirmDialog('delete')"
        >削除する</button>
      </div>

      <teleport to="body">
        <div v-if="getShouldDialog" class="modal">
          <div>
            <p>モーダルウィンドウ・ダイアログ</p>

            <!-- 子コンポーネントに、親コンポーネントのメソッド`deleteConfirmDialog`を使わせてあげる -->
            <StringCheckDialog 
              :event-type="eventType"
              :is-stack="true"
              @parent-event="deleteConfirmDialog"
              @parent-event2="closeDialog"
              @parent-event3="closeStack"
            />
          </div>
        </div>
      </teleport>
    </div>
  </div>
</template>

<script>
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
      eventType: "delete",
    }
  },
  computed: {
    getShouldDialog() {
      return this.shouldDialog
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
    executeClose() {
      this.$emit("parent-event")
    },

    executeIsFormEditFalse() {
      this.$emit("parent-event2", false)
    },

    deleteConfirmDialog(flag) {
      this.shouldDialog = flag
    },

    closeDialog() {
      this.shouldDialog = false
    },

    closeStack() {
      this.$emit("parent-event3", false)
    },

    executeConfirmDialog(eventName) {
      // deleteなどの文字列をセットする処理を入れる
      this.eventName = eventName

      if (eventName === "delete") {
        this.deleteConfirmDialog(true)

      } else if (eventName === "showDetail") {
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
  opacity: 0.95;
  z-index: 2;
  height: 100vh;
  padding: 1rem;
  border-left: 1px solid #000;

  min-height: calc(90vh + 160px);
}
</style>