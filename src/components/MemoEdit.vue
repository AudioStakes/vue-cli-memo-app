<template>
  <div>
    <h2>メモ編集</h2>

    <input
      v-model="titleValue"
      @input="edited = true"
      type="text"
    >
    <textarea
      v-model="contentValue"
      @input="edited = true"
      rows="5"
    />
    <div>
      <button
        class="update"
        @click="updateMemo"
        :disabled="!edited"
      >
        更新
      </button>
      <button
        class="destroy"
        @click="destroyMemo(id)"
      >
        削除
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MemoEdit',
  props: {
    id: {
      type: Number,
      required: true
    },
    title: {
      type: String,
      required: true
    },
    content: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      titleValue: this.title,
      contentValue: this.content,
      edited: false
    }
  },
  watch: {
    id: {
      handler () {
        this.titleValue = this.title
        this.contentValue = this.content
        this.edited = false
      }
    }
  },
  methods: {
    updateMemo () {
      if (!this.titleValue || !this.contentValue) { return }

      this.$emit('update-memo', this.id, {
        title: this.titleValue,
        content: this.contentValue
      })

      this.edited = false
    },
    destroyMemo (id) {
      this.$emit('destroy-memo', id)
    },
  }
}
</script>
