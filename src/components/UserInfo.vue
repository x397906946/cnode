<template>
  <div class="UserInfo">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" >
    </div>
    <div class="userInformation" v-else>
      <section>
        <img :src="userInfo.avatar_url">
        <span>{{userInfo.loginname}}</span>
        <p>
          {{userInfo.score}} 积分
        </p>
        <p>
          注册时间：{{userInfo.create_at | formatDate}}
        </p>
      </section>
      <div class="replies">
        <p>回复的主题</p>
        <ul>
          <li v-for="item in userInfo.recent_replies">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
      <div class="topics">
        <p>创建的主题</p>
        <ul>
          <li v-for="item in userInfo.recent_topics">
            <router-link :to="{
              name: 'post_content',
              params: {
                id: item.id
              }
            }">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserInfo",
  data(){
    return {
      isLoading: false,
      userInfo: {}
    }
  },
  methods: {
    getData(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
          .then(res=>{
            this.isLoading = false
            this.userInfo = res.data.data
          })
          .catch(function(err){
            console.log(err)
          })
    }
  },
  beforeMount(){
    this.isLoading = true
    this.getData()
  }
}
</script>

<style scoped>
  .userInformation {
    background-color: white;
    width: 75%;
    margin: 10px auto;
  }
  .userInformation section {
    padding: 12px;
  }
  .userInformation img {
    width: 30px;
  }
  .userInformation li {
    list-style: none;
  }
  .userInformation .replies,
  .userInformation .topics {
    font-size: 0.72rem;
    border-top: 10px solid #DDDDDD;
  }
  .userInformation > div > p {
    padding: 12px 0 12px 12px;
    background-color: rgba(212, 205, 205, 0.17);
    font-size: 0.75rem;
    margin: 0;
  }
  .userInformation > div > ul > li {
    padding: 4px 0 4px 12px;
    white-space: nowrap;
    font-size: 0.72rem;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 30px;
    vertical-align: middle;
  }
  .userInformation > div > ul > li > a {
    color: #094E99;
    text-decoration: none;
  }
</style>

