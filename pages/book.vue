<template>
  <div>
    <NuxtChild :books="books" @add-book-list="addBook" />
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
  // mountedだとeditなど子コンポーネントの処理の方が早く、間に合わないため
  created() {
    // if文でLocalStorageが設置されているか確認
    if (localStorage.getItem(STORAGE_KEY)) {
      // 例外処理でデータがあれば追加する処理
      try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY))
      } catch (e) {
        // エラーが発生したらデータを削除する
        // localStorage.removeItem(STORAGE_KEY)
      }
    }
  },
  methods: {
    addBook(e) {
      // 実際に何かしたことを入力する
      // if (!this.newBook) {
      //   return
      // }

      // 配列のbooksに追加する本の情報をPush
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

      // console.log(this.books)

      // saveCatsメソッドを呼び出し
      this.saveBooks()

      // 最後に追加したidの取得
      // slice(-1[0]):後ろから1つ目
      // console.log(this.books.slice(-1)[0].id)

      // 画面遷移のメソッドを呼び出す
      this.goToEditPage(this.books.slice(-1)[0].id)
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
    goToEditPage(id) {
      this.$router.push(`/book/edit/${id}`)
    },
  },
}
</script>

<style></style>
