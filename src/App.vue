<template>
  <div
    v-cloak
    id="app"
  >
    <h1>Memo App</h1>

    <div class="container">
      <div class="side">
        <MemoList
          :memos="memos"
          :current-id="memo.id"
          @edit-memo="editMemo"
          @new-memo="newMemo"
        />
      </div>
      <div class="main">
        <MemoNew
          v-if="action === 'new'"
          @create-memo="createMemo"
        />
        <MemoEdit
          v-else-if="action === 'edit'"
          :id="memo.id"
          :title="memo.title"
          :content="memo.content"
          @update-memo="updateMemo"
          @destroy-memo="destroyMemo"
        />
      </div>
    </div>
  </div>
</template>

<script>
import MemoList from './components/MemoList.vue'
import MemoNew from './components/MemoNew.vue'
import MemoEdit from './components/MemoEdit.vue'

const STORAGE_KEY = 'memos'
const STORAGE = window.localStorage

export default {
  name: 'App',
  components: {
    MemoList, MemoNew, MemoEdit
  },
  data () {
    return {
      memos: [],
      memo: {},
      newId: null,
      action: null
    }
  },
  mounted () {
    if (STORAGE.getItem(STORAGE_KEY)) {
      try {
        this.memos = JSON.parse(STORAGE.getItem(STORAGE_KEY))
      } catch (e) {
        STORAGE.removeItem(STORAGE_KEY)
      }
    }

    this.newId = this.memos.length ? Math.max(...this.memos.map(memo => memo.id)) + 1 : 1
  },
  methods: {
    newMemo () {
      this.memo = {}
      this.action = 'new'
    },
    createMemo (memoProperties) {
      this.memos.push({
        id: this.newId,
        ...memoProperties
      })
      this.saveMemos()

      this.editMemo(this.newId)
      this.newId++
    },
    editMemo (id) {
      this.memo = this.memos.find(memo => memo.id === id)
      this.action = 'edit'
    },
    updateMemo (id, memoProperties) {
      const index = this.memos.findIndex(memo => memo.id === id)

      this.memos.splice(index, 1, { id, ...memoProperties})
      this.saveMemos()
    },
    destroyMemo (id) {
      const index = this.memos.findIndex(memo => memo.id === id)

      this.memos.splice(index, 1)
      this.saveMemos()

      this.action = null
    },
    saveMemos () {
      const parsed = JSON.stringify(this.memos)

      STORAGE.setItem(STORAGE_KEY, parsed)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  font-size: 50px;
}

ul {
  list-style: none;
  padding: 0;
}

input[type='text'], textarea {
  margin: 5px 5px 5px 5px;
  padding: 0.375rem 0.75rem;
  font-size: 1.2rem;
  border-radius: 5px;
  line-height: 1.7;
  width: 70%;
}

button {
  display: inline-block;
  margin: 15px 5px 15px 5px;
  line-height: 2;
  border-radius: 8px;
  box-shadow: 0 2px #666;
  width: 150px;
  color: #4d4d4d;
  border-color: gray;
  background-color: white;
}

button.create,
button.update {
  color: #fff;
  border-color: #0d6dfd80;
  background-color: #0d6efd;
}

button.destroy {
  color: #fff;
  border-color: #dc35468c;
  background-color: #dc3545;
}

button:hover:not([disabled]) {
  filter: brightness(90%);
}

button:disabled {
    background-image: none;
    background-color: #ccc;
    border-color: #ccc;
}

.container {
  width: 80%;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 3fr;
  height: 100vh;
  border: 1px dotted gray;
}

.side {
  border-right: 1px dotted gray;
}

.title {
  border: 1px solid gray;
  border-left:none;
  border-right:none;
  color: gray;
  margin-top:-1px;
  padding: 10px;
  overflow-wrap: anywhere;
}

.title:hover {
  background-color: lightgray;
  color: black;
}

.current {
  background-color: darkgray;
  color: black;
}

[v-cloak] {
  display: none;
}
</style>
