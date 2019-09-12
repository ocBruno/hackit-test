<template>
  <div class="container" v-if="this.loaded">
<div class="search-wrapper">
        <label>Search title:</label>
    <input type="text" class="ml-2" v-model="search" placeholder="Search title.."/>
  </div>
    <modal name="blog-post"><button @click="hide()" class="btn btn-light btn-sm float-right mr-2">x</button><iframe width="100%" height="100%" :src="activeModal.video" frameborder="0"></iframe></modal>
    <div class="row">
      <div class="col-sm-3"  v-for="(post, i) in filteredTips" :key="i">
        <BlogPost>
          <img class="thumb" slot="thumb" :src="post.thumb">
          <h3 slot="title">{{ post.title }}</h3>
          <p slot="descr">{{post.descr}}</p>
          <button class="btn btn-light" slot="modalButton" v-on:click="show(post);">Ver</button>

        </BlogPost>
        </div>
    </div>
  </div>
</template>

<script>
import BlogPost from './BlogPost'

let grammarTips = {}
let oneMinuteTips = {}
let pronunciationTips = {}

for (let i = 1; i < 8; i++) {
  try {
    grammarTips['gt'+i] = require('json-loader!yaml-loader!../../blog/grammarTips/' + i + '.yaml')
    grammarTips['gt'+i].data = grammarTips['gt'+i].data[0].replace(/^\s+#.*/gm, '')
    grammarTips['gt'+i].thumb = 'https://s3-sa-east-1.amazonaws.com/rhavicarneiro/blog/grammar-tips/img/gt' + i + '-front.jpg'
    grammarTips['gt'+i].video = 'https://s3-sa-east-1.amazonaws.com/rhavicarneiro/blog/grammar-tips/video/gt' + i + '.mp4'
  } catch (e) {
    console.log(e)
  }
}

for (let i = 1; i < 41; i++) {
  try {
    oneMinuteTips['omt'+i] = require('json-loader!yaml-loader!../../blog/oneMinuteTips/' + i + '.yaml')
    oneMinuteTips['omt'+i].data = oneMinuteTips['omt'+i].data[0].replace(/^\s+#.*/gm, '')
    oneMinuteTips['omt'+i].thumb = 'https://s3-sa-east-1.amazonaws.com/rhavicarneiro/blog/one-minute-tips/img/omt' + i + '-front.jpg' 
    oneMinuteTips['omt'+i].video = 'https://s3-sa-east-1.amazonaws.com/rhavicarneiro/blog/one-minute-tips/video/omt' + i + '.mp4'
  } catch (e) {
    // console.log(e)
  }
}

for (let i = 1; i < 20; i++) {
  try {
    pronunciationTips['pt'+i] = require('json-loader!yaml-loader!../../blog/pronunciationTips/' + i + '.yaml')
    pronunciationTips['pt'+i].data = pronunciationTips['pt'+i].data[0].replace(/^\s+#.*/gm, '')
    pronunciationTips['pt'+i].thumb = 'https://s3-sa-east-1.amazonaws.com/rhavicarneiro/blog/pronunciation-tips/img/pt' + i + '-front.jpg'
    // removes videos with broken link format
    pronunciationTips['pt'+i].video = i < 14 ? 'https://s3-sa-east-1.amazonaws.com/rhavicarneiro/blog/pronunciation-tips/video/pt' + i + '.mp4' : delete pronunciationTips['pt'+i]
  } catch (e) {
    // console.log(e)
  }
}


export default {
  name: 'Container',
  components: {
    BlogPost
  },

  data() {
    return {
      search: '',
      loaded: false,
      activeModal: {video: ''},
      lessonTips: [],
      scrolledToBottom: false,
      lessonCount: 20
    }
  },
  methods: {
    show(modalData) {
      this.activeModal.video = modalData.video
      this.$modal.show('blog-post')
    },
    scroll () {
      window.onscroll = () => {
        let bottomOfWindow = Math.max(window.pageYOffset, document.documentElement.scrollTop, document.body.scrollTop) + window.innerHeight === document.documentElement.offsetHeight
        if (bottomOfWindow) {
          this.scrolledToBottom = true 
          this.lessonCount += 5
        }
      }
    },
    hide() {
      this.$modal.hide('blog-post')
    },
    spreadObj(obj) {
      Object.keys(obj).forEach((key)=>{
        this.lessonTips.push(obj[key])
      })
    },
  },
  computed: {
    filteredTips() {
      return this.lessonTips.filter(post => {
        return post.descr.toLowerCase().includes(this.search.toLowerCase())
    }).filter((post, i) => {
      return i < this.lessonCount
    })
  }
  },
  mounted() {
    this.scroll()
    this.spreadObj(grammarTips)
    this.spreadObj(oneMinuteTips)
    this.spreadObj(pronunciationTips)
    this.loaded = true
  },
  created() {
  },
  destroyed () {
  },
  updated() {


  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
.thumb {
  width: 200px;
}
</style>
