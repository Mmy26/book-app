<template>
  <div>
    <v-row>
      <v-col cols="6">
        <v-btn color="primary" to="/book/search">検索する</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="book in books" :key="book.id" cols="12" md="6" class="mt-4">
        <v-card>
          <v-row>
            <v-col cols="4"><v-img :src="book.image"></v-img></v-col>
            <v-col cols="8">
              <v-card-title>{{ book.title }}</v-card-title
              >読んだ日：{{ book.readDate }} 感想：{{ book.memo }}
              <v-spacer />
              <v-card-actions>
                <!-- nameはrouterで確認できる, paramsで動的なidを渡すことができるようになる -->
                <v-btn
                  :to="{ name: `book-edit-id`, params: { id: book.id } }"
                  color="indigo"
                  fab
                  small
                  dark
                  ><v-icon>mdi-pencil</v-icon></v-btn
                >
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
  // localStorageのデータをBook.vue経由で受け取る
  props: {
    books: { type: Array, default: () => {} },
  },
}
</script>

<style></style>
