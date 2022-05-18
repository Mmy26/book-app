<template>
  <div>
    <NuxtChild @add-book-list="addBook" />
  </div>
</template>

<script>
const STORAGE_KEY = 'books'
export default {
  data() {
    return {
      // 情報を入れる配列
      books: [],
      // 新たく追加するデータ
      newBook: null,
    }
  },
  mounted() {
    // if文でLocalStorageが設置されているか確認
    if (localStorage.getItem(STORAGE_KEY)) {
      // 例外処理でデータがあれば追加する処理
      try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY))
      } catch (e) {
        localStorage.removeItem(STORAGE_KEY)
      }
    }
  },
  methods: {
    addBook(e) {
      // 実際に何かしたことを入力する
      // if (!this.newBook) {
      //   return
      // }

      // 配列のbooksにnewBookをPush
      this.books.push({
        id: this.books.length,
        title: e.title,
        image: e.image,
        description: e.description,
        readDate: '',
        memo: '',
      })
      // newBookを空にしておく
      // this.newBook = ''

      console.log(this.books)
      // saveCatsメソッドを呼び出し
      this.saveBooks()
    },
    removeBook(x) {
      this.books.splice(x, 1)
      this.saveBooks()
    },
    saveBooks() {
      const parsed = JSON.stringify(this.books)
      // LocalStorageにデータを保存
      localStorage.setItem(STORAGE_KEY, parsed)
    },
  },
}
</script>

<style></style>
