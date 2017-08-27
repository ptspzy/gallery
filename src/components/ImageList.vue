<template>
  <div id="gallery">
    <div v-for="item in imgs" class="img" v-bind:style="{width:item.ww + 'px',height:item.hh+'px'}">
      <a data-fancybox="gallery" :href="item.src">
        <img class="img-item"  :src="item.src" />
      </a>
    </div>
  </div>
</template>

<script>
  export default { // 'http://127.0.0.1:8080' +
    name: 'ImageList',
    props: ['imageList'],
    data() {
      return {
        imgs: this.imageList || [],
      };
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #gallery {
    position: relative;
    padding: 5px;
  }
  .img {
    display: block;
    margin: 5px;
    float: left;
    background: #ececec;
  }
  .img-item {
    width: 100%;
    height: 100%;
  }
</style>
