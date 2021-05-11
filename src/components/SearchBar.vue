<template>
  <div class="search-bar">
      <input @keypress.enter="onInputKeyword" type="text" placeholder="Search">
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  methods: {
    onInputKeyword: function (event) {
      // console.log(event.target.value) //event가 발생한 키의 value를 가져옴
      this.$emit('input-search', event.target.value)
    }
  }
}
</script>

<style>
.search-bar {
  transition: margin 0.5s;
}

.search-bar > input {
  width: 100%;      /* 가로 너비 배치를 화면의 끝과 끝으로 배치 */
  padding: 0.5rem;  /* input 안쪽의 margin을 여유롭게 */
  font-size: 2rem;
}
</style>