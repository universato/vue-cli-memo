<template>
  <div class="memos-app">
    <div>
      <h1 class="app-title">{{ app_title }}</h1>
    </div>
    <div class="app-main">
      <div class="memos-main">
        <div class="summary"> 
          {{ numberOfCompletedMemos }} / {{ numberOfMemos }} 件、完了しました。
        </div>
        <div class="memos" v-for="(memo, i) in memos" :key="memo">
          <div class="memo">
            <input class="toggle" type="checkbox" v-model="memo.completed" />
            <a class="edit-button" @click="editMemo(i)"> {{ titleOf(memo) }} </a>
          </div>
        </div>
        <a class="new-button" @click="newMemo()">+ メモを新規作成する</a>
      </div>
      <div class="memo-under-edit" v-if="memoUnderEdit">
        <textarea class="memo-textarea" v-model="memoUnderEdit.body"></textarea><br>
        <div class="memo-buttons">
          <button type="submit" class="save-button" @click="saveMemo()">保存する</button>
          <button class="delete-button" @click="deleteMemo(memoUnderEdit)">削除する</button><br>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const STORAGE_KEY = 'memos-vuejs-3.0';

const memoStorage = {
  fetch() {
    let memos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    memos.forEach((memo, index) => { memo.id = index; });
    memoStorage.uid = memos.length;
    return memos;
  },
  save(memos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(memos));
  }
};

export default {
  name: 'Memos',
  data () {
    return {
      memos: memoStorage.fetch(),
      memoUnderEdit: null,
    }
  }, 
  methods: {
    titleOf(memo){
      return memo.body.split("\n")[0];
    },
    editMemo(index) {
      this.memoUnderEdit = Object.assign({}, this.memos[index]);
    },
    newMemo() {
      this.memoUnderEdit = {id: null, completed: false, body: ''};
    },
    saveMemo() {
      let value = this.memoUnderEdit.body
      let id = this.memoUnderEdit.id
      if (id !== null) {
        let memo = this.memos.find((memo) => { memo.id == this.memoUnderEdit.id; })
        memo = this.memoUnderEdit;
        this.memos[id] = memo;
        memoStorage.save(this.memos);
        this.memoUnderEdit = null;
        return;
      }
      if (!value) { return; }
      this.memos.push({
        id: memoStorage.uid++,
        body: value,
        completed: false
      });
      memoStorage.save(this.memos);
      this.memoUnderEdit = null;
    },
    deleteMemo() {
      let id = this.memoUnderEdit.id;
      if (id === null) {
        memoStorage.save(this.memos);
        this.memoUnderEdit = null;
        return;
      }
      this.memos.splice(id, 1);
      memoStorage.save(this.memos);
      this.memos = memoStorage.fetch();
      this.memoUnderEdit = null;
    }
  },
  computed: {
    numberOfMemos() {
      return this.memos.length;
    },
    numberOfCompletedMemos() {
      return this.memos.filter((memo) => memo.completed).length
    },
  },
  props: {
    app_title: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.memos-app {
  margin: 40px;
  height: 560px;
  width: 800px;
  background-color: #fff;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
}

.app-title {
  color: #666;
  text-shadow: 1px 1px 1px #999;
}

.app-main {
  display: grid;
  grid-template: 1fr / 1fr 1fr;
}

.memos-main {
  text-align: left;
  margin: 8px 40px;
}

.summary {
  margin: 4px 32px;
  text-align: left;
}

.memos {
  text-align: left;
  display: flex;
  flex-direction: column;
}

.new-button{
  margin: 32px 16px;
  padding: 8px 8px;
}

a {
  transition: 0.2s;
}
a:hover {
  transition: 0.2s;
  color: #666;
  /* letter-spacing: 0.1em; */
}
a:hover::before {
  animation: bg_slide 0.2s ease-in;
}

.memo-under-edit {
  margin: 0 28px;
}

.memo-textarea {
  margin: 8px 40px;
  height: 400px;
  width: 320px;
}

.memo-buttons {
  display: grid;
  grid-template: 1fr / 2fr 1fr;
}

.save-button {
  margin: 4px 8px;
  border-color: #ddd;
  border-radius: 6%;
}

.delete-button {
  margin: 4px 8px;
  border-color: #ddd;
  border-radius: 6%;
}

</style>
