<template>
  <div>
    <div class="searchBox">
      <input type="text" v-model="book_input" class="s_ipt" name="book_info" maxlength="100" autocomplete="off"/>
      <input type="submit" class="query_book_btn" value="逐风一下" v-on:click="searchBook"/>
      <div id="books_list">
        <ul id="books">
          <li v-for="book in books">
            <div id="item">
              <div id="left">
                <div id="top">
                  <label><span id="book_name">书籍:</span> {{book.novel_name}}</label>
                  <label><span id="book_author">作者:</span> {{ book.novel_author }}</label>
                </div>
                <div id="bottom">
                  <label><span id="book_details">简介:</span> {{book.novel_details}}</label>
                </div>
              </div>
              <div id="right">
                <a class="download" v-bind:href="book.novel_down_url">下载</a>
              </div>
            </div>
          </li>
        </ul>
      </div>
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
        this.$http.get(queryBook)
          .then(function (data) {
            if (data.data.code === 200) {
              vm.books = data.data.data
            } else {
              alert(data.data.msg)
            }
          }).catch(function (err) {
            alert(err)
          })
      },
      searchBook: function () {
        let bookInfo = this.book_input
        this.query('all', bookInfo)
      }
    }

  }
</script>
