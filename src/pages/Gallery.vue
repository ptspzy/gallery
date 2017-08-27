<template>
  <div>
    <image-list :imageList="organizeImg()"></image-list>
  </div>
</template>

<script>
  import ImageList from '@/components/ImageList';
  import axios from 'axios'
  import data from '../../config/data' // 本地照片文件生成的json数据

  export default {
    name: 'gallery',
    methods: {
      countImgsInCol(idx, imgs) {
        const gallery = document.querySelector('#gallery');
//        const gw = gallery.clientWidth;
        const gw = window.innerWidth || 1440;
        const maxEachCol = 4;
        const minH = 120;
        let count = 0;
        for (let i = Math.min(maxEachCol, imgs.length - idx); i > 0; i -= 1) {
          let w = 0;
          for (let j = idx; j < idx + i; j += 1) {
            const minW = (minH * imgs[j].w) / imgs[j].h;
            if (minW >= gw) {
              return 1;
            } else {
              w += minW;
            }
          }
          if (w < gw) {
            count = i;
            break;
          }
        }
        return count;
      },
      getCols(imgs) {
        const cols = [];
//        const rest = imgs.length;
        let count = 1;
        for (let i = 0; i < imgs.length; i += count) {
          count = this.countImgsInCol(i, imgs);
          console.log(i, count);
          cols.push({ start: i, end: (i + count) - 1, total: count });
        }
        return cols;
      },
      organizeImg() {
        const imgs = this.randomImg() // for test
//        const data = await aixos.get('http://127.0.0.1:7001/photo') // get real data
//        const imgs = data.data;
        console.log(imgs)
        const galleryW = window.innerWidth || 1440;
        const margin = 10;
        const cols = this.getCols(imgs);
        for (let i = 0; i < cols.length; i += 1) {
          const col = cols[i];
          let colH = 0;
          let scale = 0;
          for (let j = col.start; j <= col.end; j += 1) {
            scale += imgs[j].w / imgs[j].h;
          }
          colH = (galleryW - (margin * (col.total + 1))) / scale;
          console.log(colH);
          for (let k = col.start; k <= col.end; k += 1) {
            const img = imgs[k];
            img.hh = colH;
            img.ww = (colH * img.w) / img.h;
          }
        }
        return imgs;
      },
      randomImg() {
        const count = 500 + Math.ceil(Math.random() * 10);
        const imgs = [];
        for (let i = count; i > 0; i -= 1) {
          const w = 250 + (Math.ceil(Math.random() * 50) * 10);
          const h = 200 + (Math.ceil(Math.random() * 50) * 10);
          imgs.push({
            src: `https://unsplash.it/${w}/${h}`,
            w,
            h,
          });
        }
        return imgs;
      },
    },
    mounted() {
      window.addEventListener('resize', () => {
        this.organizeImg()
      })
    },
    components: {
      ImageList,
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
