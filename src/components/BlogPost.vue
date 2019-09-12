<template>
  <div>
    <img :src="this.thumb" :alt="this.title" />

    <div><slot name="thumb"></slot></div>
    <div><slot name="title"></slot></div>
    <div><slot name="descr"></slot></div>
    <div><slot name="modalButton"></slot></div>
  </div>
</template>
<script>
export default {
  name: 'BlogPost',
  data() {
    return {
      category: '',
      date: '',
      title: '',
      video: '',
      thumb: '',
      image: '',
      descr: '',
      data: ''
    }
  },
  mounted() {console.log('mounted')},
  created() {console.log('created')},
  updated() {console.log('updated')},
}
</script>
<style lang="scss"></style>
