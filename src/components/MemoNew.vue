<template>
  <div>
    <h2>メモ作成</h2>

    <input
      v-model="title"
      type="text"
    >
    <textarea
      v-model="content"
      rows="5"
    />
    <div>
      <button
        class="create"
        @click="createMemo"
      >
        登録
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MemoNew',
  data () {
    return {
      title: '',
      content: ''
    }
  },
  methods: {
    createMemo () {
      if (!this.title || !this.content) { return }

      this.$emit('create-memo', {
        title: this.title,
        content: this.content
      })
    },
  }
}
</script>
