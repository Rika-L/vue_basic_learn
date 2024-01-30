<script>
export default {
  name:'School',
  data() {
    return {
      name: 'WYU',
      address: 'Jiangmen',
    }
  },
  methods: {
    showName() {
      alert(this.name)
    }
  }
}
</script>

<template>
  <div class="demo">
    <h2>学校名称：{{ name }}</h2>
    <h2>学校地址：{{ address }}</h2>
    <button @click="showName">点我提示学校名</button>
  </div>
</template>

<style scoped>
.demo {
  background-color: orange;
}
</style>