<template>
  <div id="listTest">
    
    <div class="buttonWrapper">
      <button 
        @click="openStack(0)"
        class="commonButton"
      >
        新規登録
      </button>
    </div>
    <ul class="flex mt-4">
      <li v-for="(item, index) in items" :key="index" class="card">
        <div class="card-top">
          <div class="imgWrapper">
            <img :src="`${item.imgSrc}`" alt="">
          </div>
        </div>
        <div class="card-bottom">
          {{ item.name }}
          {{ item.age }}

          <button 
            @click="openStack(item.id)" 
            class="commonButton"
          >
            詳細を見る
          </button>
        </div>
      </li>
    </ul>

    <DetailStack
      :is-form-edit="isFormEdit"
      :item="item"
      @parent-event="setIsFormEdit"
      @parent-event2="setIsShowStack"
      :class="giveClassNameIsShow"
      ref="child"
    />
  </div>
</template>

<script lang="ts">
import items from "@/assets/json/items.json"
import { Item } from "~~/types/common"

export default {
  data() {
    return {
      // items: [
      //   {
      //     "id": 1,
      //     "name": "aaa",
      //     "age": 12,
      //   },
      //   {
      //     "id": 2,
      //     "name": "bbb",
      //     "age": 22,
      //   },
      //   {
      //     "id": 3,
      //     "name": "bbb",
      //     "age": 32,
      //   },
      // ],
      items: items,
      isShow: false,
      itemId: 0,
      isFormEdit: false,
      item: {
        id: 0,
        name: "",
        age: 0,
        imgSrc: "",
      },
    }
  },
  computed: {
    giveClassNameIsShow() {
      if (!this.isShow) {  
        return "displayNone"
      }
      return ""
    },

    getItem(): Item {
      return this.item
    },

  },
  methods: {
    openStack(itemId: number | undefined) {
      if (itemId !== 0 && !itemId) return

      window.scroll({ top: 0, behavior: 'smooth' })
      // this.$nextTick(() => {
      //   const destinationDom = this.$refs.child as HTMLElement
      //   destinationDom.scrollIntoView({ behavior: "smooth" })
      // })

      if (itemId !== 0) {
        this.isFormEdit = true

      } else {
        itemId = 0
        this.isFormEdit = false
      }

      // itemIdから、IDが一致するオブジェクトを探す
      for (let item of this.items) {
        if (item.id === itemId) {
          // itemを子要素に渡す
          this.item = item

          // 一致した要素が見つかったら、その時点で処理終了
          break
        }
      }

      this.isShow = true
      this.itemId = itemId
    },

    /** TOOD 孫からも呼べるようにしたい */
    closeStack() {
      this.isShow = false
    },

    setIsShowStack(flag: boolean) {
      this.isShow = flag
    },

    setIsFormEdit(flag: boolean) {
      this.isFormEdit = flag
    },
  },
}
</script>

<style lang="scss">
.card {
  min-width: 20%;
  max-width: 45%;
  min-height: 10rem;

  border: 1px solid #000;
  margin-bottom: 2rem;

  &:nth-child(2n) {
    margin-left: 2rem;
  }
}

.card-bottom {
  padding: 1rem;
}
</style>