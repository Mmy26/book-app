<template>
  <div>
    <v-row>
      <v-col cols="6">
        <v-text-field v-model="keyword" label="本のタイトルを検索" />
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn color="primary" @click="search(keyword)"> 検索する</v-btn>
      </v-col>
      <v-col>
        <v-btn color="secondary" to="/book">一覧に戻る</v-btn>
      </v-col>
    </v-row>
    <div v-show="!isFound" class="mt-4">検索結果は０件でした。</div>
    <v-row class="mt-4">
      <v-col v-for="(book, i) of searchResults" :key="i" cols="12" md="6">
        <v-card class="mx-auto mb-4">
          <v-row>
            <v-col cols="4">
              <v-img :src="book.image"></v-img>
            </v-col>
            <v-col cols="8">
              <v-card-title>{{ book.title }}</v-card-title>
              {{ book.description }}
              <v-spacer />
              <v-card-actions>
                <v-btn class="mx-2" fab dark color="indigo">
                  <v-icon dark>mdi-plus</v-icon>
                </v-btn>
              </v-card-actions>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 検索キーワード
      keyword: '',
      // 検索結果
      searchResults: [],
      // データが存在するか
      isFound: true,
    }
  },
  methods: {
    async search(keyword) {
      // 初期化しておく
      this.searchResults = []
      // クエリストリングを作成
      // GoogleBooksAPIのURL
      const baseUrl = 'https://www.googleapis.com/books/v1/volumes?'
      // 検索対象のオブジェクト
      const params = {
        q: `intitle:${keyword}`,
        maxResults: 40,
      }
      // 形式を整える
      const queryParams = new URLSearchParams(params)
      //console.log(baseUrl + queryParams)

      // fetchでJSON取得
      const response = await fetch(baseUrl + queryParams)
        // 同時にレスポンスをJSONに変換
        .then((response) => response.json())
      //console.log(response.items)

      // データが存在するかどうかで条件分岐
      if (response.items === undefined) {
        this.isFound = false
      } else {
        // 必要な情報を配列にpush
        for (const book of response.items) {
          // 取得した情報の中で必要なものだけを変数に代入
          const title = book.volumeInfo.title
          const img = book.volumeInfo.imageLinks
          const description = book.volumeInfo.description
          // searchResultsにPushしていく
          this.searchResults.push({
            // 三項演算子 中身があったら追加
            title: title ? title : '', // eslint-disable-line
            image: img ? img.thumbnail : '',
            // 説明文が長いのでsliceで40文字に制限
            description: description ? description.slice(0, 40) : '',
          })
        }
      }
    },
  },
}
</script>

<style></style>
