3

<template>
  <div class="searchBox">
    <input type="text" v-model="book_input" class="s_ipt" name="book_info" maxlength="100" autocomplete="off"/>
    <input type="submit" class="btn" value="百度一下" v-on:click="searchBook"/>
    <div id="books_list">
      <ul id="books">
        <li v-for="book in books">
          <div id="item">
            <p>{{ book.name }}</p>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'searchBox',
    data () {
      return {
        book_input: '',
        books: []
      }
    },
    methods: {
      query: function (category, info) {
        let vm = this
        let queryBook = '/api/query_book?novel_category=' + category + '&book_info=' + info
        this.$http.get(queryBook, this.$data)
          .then(function (data) {
            if (data.data.code === 200) {
              vm.books = data.data.data
            } else {
              alert(data.data.msg)
            }
          })
      },
      searchBook: function () {
        let bookInfo = this.book_input
        this.query('all', bookInfo)
      }
    }

  }
</script>
