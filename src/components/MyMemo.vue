<template>
  <article>
    <h1>Memo</h1>
    <div class="memo-body">
      <ul class="memo-list">
        <li
          v-for="memo in memos"
          :key="memo.content"
        >
          <button
            class="list-button"
            @click="setEditingMode(memo)"
          >{{ memo.content.split('\n')[0] }}</button><br>
        </li>
        <li>
          <button
            class="list-button"
            @click="mode = 'add'"
          >＋</button>
        </li>
      </ul>
      <div class="memo-content" v-if="mode === 'add'">
        <textarea v-model="newMemo"></textarea><br>
        <button @click="addNewMemo">追加</button>
      </div>
      <div class="memo-content" v-else-if="mode === 'edit'">
        <textarea v-model="editingMemo"></textarea><br>
        <button @click="editMemo">編集</button>
        <button @click="remove">削除</button>
      </div>
    </div>
  </article>
</template>

<script>
const STORAGE_KEY = 'memos';
const memoStorage = {
  uid: 0,
  fetch() {
    const memos = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || '[]'
    );
    memos.forEach(function (memo, index) {
      memo.id = index;
    });
    memoStorage.uid = memos.length;
    return memos;
  },
  save(memos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(memos));
  }
};

export default{
  data() {
    return {
      memos: [],
      newMemo: null,
      editingMemo: null,
      editIndex: null,
      mode: null,
    }
  },
  created() {
    this.memos = memoStorage.fetch();
  },
  methods: {
    addNewMemo() {
      if (this.newMemo) {
        this.memos.push({
          id: memoStorage.uid++,
          content: this.newMemo
        });
        this.newMemo = '';
        memoStorage.save(this.memos);
        this.mode = null;
      }
    },
    setEditingMode(memo) {
      this.mode = 'edit';
      this.editIndex = this.memos.indexOf(memo);
      const text = this.memos[this.editIndex].content;
      this.editingMemo = text;
    },
    resetEditingMode() {
      this.mode = null;
      this.editIndex = null;
      this.editingMemo = null;
    },
    remove() {
      this.memos.splice(this.editIndex, 1);
      memoStorage.save(this.memos);
      this.resetEditingMode();
    },
    editMemo() {
      if (this.editingMemo) {
        this.memos[this.editIndex].content = this.editingMemo
        memoStorage.save(this.memos);
        this.resetEditingMode();
      }
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 30px;
  margin: 3px auto;
}

.memo-body {
  display:flex;
}

.memo-list {
  width: 40%;
}

.memo-content {
  width: 80%;
  text-align:center;
  margin-left:10px;
}

li {
  margin: 5px auto;
  text-align: left;
  font-size: 18px;
  list-style: none;
}

button {
  margin: 5px;
  padding: 5px 40px;
  font-size: 18px;
}

.list-button {
  font-size: 24px;
  cursor: pointer;
  border: none;
  background: none;
  color: #0033cc;
}
.list-button:hover{
  text-decoration: underline;
  color: #5250fd;
}

textarea {
  width: 90%;
  font-size: 24px;
  margin-top: 20px;
  height: calc( 2em * 11.5 );
  line-height: 1.3;
  padding: 20px;
}
</style>
