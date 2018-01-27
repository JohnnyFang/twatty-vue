<template>
  <div id="app">
    <ul>
      <li v-for="book in books">
        {{ book.title }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      books: [{
        _id: "1",
        title: "The power of habits"
      },{
        _id: "2",
        title: "Steal the show"
      }]
    }
  }
}
</script>