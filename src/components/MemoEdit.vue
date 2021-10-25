<template>
  <div>
    <h2>メモ編集</h2>

    <input
      v-model="title"
      @input="edited = true"
      type="text"
    >
    <textarea
      v-model="content"
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
        @click="destroyMemo(memo.id)"
      >
        削除
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MemoEdit',
  model: {
    prop: 'memo'
  },
  props: {
    memo: {
      type: Object,
      required: true
    },
  },
  data () {
    return {
      title: this.memo.title,
      content: this.memo.content,
      edited: false
    }
  },
  watch: {
    memo: {
      handler (memo) {
        this.title = memo.title
        this.content = memo.content
        this.edited = false
      }
    }
  },
  methods: {
    updateMemo () {
      if (!this.title || !this.content) { return }

      this.$emit('update-memo', this.memo.id, {
        title: this.title,
        content: this.content
      })

      this.edited = false
    },
    destroyMemo (id) {
      this.$emit('destroy-memo', id)
    },
  }
}
</script>
