<template>
  <div id="home">
    <div
      id="carousel-example-generic"
      class="carousel slide"
      data-ride="carousel"
      data-interval="4000"
    >
      <ol class="carousel-indicators">
        <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
        <li data-target="#carousel-example-generic" data-slide-to="1"></li>
        <li data-target="#carousel-example-generic" data-slide-to="2"></li>
      </ol>
      <!-- Wrapper for slides -->
      <div class="carousel-inner" role="listbox">
        <div class="item active">
          <img src="../assets/1.jpg" alt class="carousel-img" />
        </div>
        <div class="item">
          <img src="../assets/2.jpg" alt class="carousel-img" />
        </div>
        <div class="item">
          <img src="../assets/3.jpg" alt class="carousel-img" />
        </div>
      </div>

      <!-- Controls -->
      <a
        class="left carousel-control"
        href="#carousel-example-generic"
        role="button"
        data-slide="prev"
      >
        <!-- 左按钮 -->
        <!-- <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
        <span class="sr-only">Previous</span>-->
      </a>
      <a
        class="right carousel-control"
        href="#carousel-example-generic"
        role="button"
        data-slide="next"
      >
        <!-- 右按钮 -->
        <!-- <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
        <span class="sr-only">Next</span>-->
      </a>
    </div>
    <div class="container main">
      <div class="row">
        <div class="col-md-3" id="sideBar">
          <!-- 幽默笑话 -->
          <div class="joke">
            <div class="joke-title">
              <h3>每日一笑</h3>
              <img src="../../src/assets/joke.jpg" alt />
            </div>
            <div class="joke-list">{{joke}}</div>
          </div>
          <button class="btn btn-primary form-control joke-btn" @click="getJoke">换一个</button>
          <!-- ********* -->
          <!-- 日历 -->
          <div style="overflow:hidden;" class="date">
            <div class="form-group">
              <div id="datetimepicker13"></div>
            </div>
          </div>
          <!-- 打赏博主 -->
          <div class="money">
            <img src="../assets/my-money.png" alt />
          </div>
        </div>
        <div class="col-md-9" id="content">
          <!-- 文章列表 -->
          <div class="day" v-for="(item, index) in articleList" :key="index">
            <div class="dayTitle">
              <a href="javascript:;">{{item.article_date}}</a>
            </div>
            <div class="postTitle">
              <a href="javascript:;" class="postTitle2">{{item.article_title}}</a>
            </div>
            <div class="postCon">
              <div class="c_b_p_desc">
                {{item.article_content.slice(0,30)}}
                <router-link
                  :to="'/singleblog/'+item.article_title"
                  class="c_b_p_desc_readmore"
                >阅读原文</router-link>
              </div>
            </div>
            <div class="postDesc">ayong 阅读 (2) 评论 (0)</div>
          </div>
        </div>
      </div>
    </div>
    <Footer></Footer>
  </div>
</template>
<script>
import Footer from './Footer'
export default {
  // name:'home',
  data() {
    return {
      joke: '',
      articleList: []
    }
  },

  components: {
    Footer
  },
  methods: {
    getJoke() {
      this.$http.get('https://autumnfish.cn/api/joke/list?num=1').then(data => {
        // console.log(data.data.jokes[0]);
        this.joke = data.data.jokes[0]
        // console.log(this.joke);
      })
    }
  },
  created() {
    this.getJoke()
    this.$http
      .get('http://dayongge.xyz/phpcrud/app.php?action=read')
      .then(res => {
        // console.log(res.data.articles);
        this.articleList = res.data.articles
      })
  },
  mounted() {
    $('#datetimepicker13').datetimepicker({
      inline: true
    })
  }
}
</script>

<style scoped>
.row {
  display: flex;
}
.main {
  margin-top: 10px;
}
.carousel-img {
  width: 100%;
}
#sideBar,
#content {
  background: #f6f8fa;
  box-shadow: 0 0 12px #aaa;
}
#content {
  margin-left: 1em;
  padding: 10px 5px;
}

.joke-list {
  padding: 10px;
  height: 14em;
  overflow-y: scroll;
  line-height: 1.7em;
  font-size: 15px;
}
.joke-list::-webkit-scrollbar {
  display: none;
}
.joke-list li {
  margin-top: 5px;
  font-size: 14px;
}

.joke h3 {
  font-style: italic;
  font-family: '隶书';
}
.joke .joke-title {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.joke img {
  height: 3.5em;
  border-radius: 5px;
}
.joke-btn {
  margin: 5px 0;
}
.day {
  background: rgba(255, 255, 255, 0.5);
  /* min-height: 80px; */
  border-radius: 7px;
  box-shadow: 1px 1px 2px #a7a8ad;
  color: #666;
  margin: 0 5px 20px 0;
  padding: 5px 20px 10px;
}
.dayTitle {
  width: 100%;
  color: #666;
  line-height: 2.2em;
  font-size: 22px;
  /* clear: both; */
  border-bottom: 1px solid #ccc;
  text-align: center;
}
.postTitle {
  border-left: 8px solid rgba(33, 160, 139, 0.68);
  margin-left: 10px;
  margin-bottom: 10px;
  font-size: 20px;
  float: right;
  width: 100%;
  font-weight: bold;
  border-bottom: 1px dashed #ccc;
  line-height: 2.5em;
  /* clear: both; */
}
.day .postTitle a {
  padding-left: 10px;
}
.postCon {
  line-height: 1.5em;
  width: 100%;
  clear: both;
  padding: 10px 0;
  margin-bottom: 1.5em;
}
.c_b_p_desc {
  padding-left: 10px;
}
.c_b_p_desc_readmore {
  padding-left: 5px;
}
.postDesc {
  /* background: url(images/posted_time.png) no-repeat 0 1px; */
  color: #757575;
  /* float: left; */
  width: 100%;
  /* clear: both; */
  text-align: left;
  font-family: '微软雅黑', '宋体', '黑体', Arial;
  font-size: 13px;
  padding-right: 20px;
  margin-top: 20px;
  line-height: 1.8;
  padding-bottom: 2em;
  margin-left: 2em;
}
.date {
  margin-top: 2em;
  border: 2px dotted #ccc;
  border-radius: 12px;
}
.money img {
  margin-top: 2em;
  width: 100%;
}
</style>
