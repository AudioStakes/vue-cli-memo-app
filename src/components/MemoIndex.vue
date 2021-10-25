<template>
  <div>
    <h2>メモ一覧</h2>

    <button
      class="new"
      @click="newMemo"
    >
      新規メモ
    </button>

    <ul>
      <li
        v-for="memo in memos"
        :key="memo.id"
        class="title"
        :class="{current: memo.id === currentId}"
        @click="editMemo(memo.id)"
      >
        {{ memo.title }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'MemoIndex',
  props: {
    memos: {
      type: Array,
      required: true
    },
    currentId: {
      type: Number,
      default: null
    },
  },
  methods: {
    editMemo (id) {
      this.$emit('edit-memo', id)
    },
    newMemo () {
      this.$emit('new-memo')
    }
  }
}
</script>
