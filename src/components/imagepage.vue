<template>
    <div id="app">
      <div v-if="ready" class="">
        <div class="container">
          <div class="row profile">
            <div>
              <ul class="breadcrumb">
                <li class="completed"><a href="#"><i class="glyphicon glyphicon-home"></i>&nbsp;&nbsp; Home</a></li>
                <router-link to="/albumpage"><li class="completed"><a href="#" class="disabled">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<i class="glyphicon glyphicon-folder-open"></i>&nbsp;&nbsp; {{realalbum}}</a></router-link></li>
                <li class="active"><a href="#" class="disabled"><i class="glyphicon glyphicon-picture"></i>&nbsp;&nbsp; {{this.realname.name}} </a></li>
                <div class="navbar-right">
                  <li v-if="authorized" @click="logout()" class="navbar-right"><a href="javascript:function() { return false; }"><span class="glyphicon glyphicon-user"></span>&nbsp;&nbsp; Sign out</a></li>
                  <li v-else="authorized" @click="login()" class="navbar-right"><a href="javascript:function() { return false; }"><span class="glyphicon glyphicon-user"></span>&nbsp;&nbsp; Sign in</a></li>
                </div>
              </ul>
            </div>
            <div class="profile-content">
              <div v-if="authorized">
                <h1>{{realalbum}}®</h1>
                <h2>● {{this.realname.name}} ●</h2>
              </div>
              <div v-else="authorized">
                <h5>Error (.)(.) : Please Sign in with Facebook</h5>
                <img src="/static/error.gif" alt="" height="300"><br><br>
              </div>

              <router-view :id="id"></router-view>

                <div v-if="authorized" class="">
                  <div class="row">
                    <div class="col-sm-12 col-sm-offset-0">
                      <div  v-for="photo in photos" class="box">
                        <img class = "img-rounded" width = '60%' :src="photo.images[1].source" alt="" /><br>
                      </div>
                    </div>
                  </div>
                </div>
            </div>

	         </div>
         </div>
         <br>
         <br>
       </div>

      <div v-else="ready" class="">
        <center>
          <router-link to="/"><img src="/static/warp.gif" alt="" width="200px"/></router-link>
        </center>
      </div>
    </div>

</template>

<script>
/* global FB */
import Hello from './Hello'

export default {
  name: 'imagepage',
  props: ['id', 'page'],
  components: {
    Hello
  },
  data () {
    return {
      albums: [],
      photos: [],
      pagename: [],
      realname: [],
      realalbum: '',
      profile: {},
      ready: false,
      authorized: false
    }
  },
  methods: {
    getAlbums () {
      let vm = this
      FB.api('/cupamag/albums', {fields: ['cover_photo', 'name']}, function (response) {
        console.log(response)
        vm.$set(vm, 'albums', response.data)
      })
    },
    realName (id) {
      let vm = this
      FB.api('/' + id, function (response) {
        console.log(response)
        vm.$set(vm, 'realname', response)
      })
    },
    realAlbum (page) {
      let vm = this
      FB.api('/' + page, function (response) {
        console.log(response)
        vm.$set(vm, 'realalbum', response.name)
        console.log(response.name)
      })
    },
    getPhotosByAlbumId (albumId) {
      let vm = this
      FB.api('/' + albumId + '/photos', {fields: ['images']}, function (response) {
        console.log(response)
        vm.$set(vm, 'photos', response.data)
      })
    },
    getProfile () {
      let vm = this
      FB.api('/me', function (response) {
        console.log(response)
        vm.$set(vm, 'profile', response)
      })
    },
    login () {
      let vm = this
      FB.login(function (response) {
        vm.statusChangeCallback(response)
      }, {scope: 'publish_actions'})
    },
    logout () {
      let vm = this
      FB.logout(function (response) {
        vm.statusChangeCallback(response)
      })
    },
    statusChangeCallback (response) {
      let vm = this
      vm.ready = true
      console.log('statusChangeCallback')
      console.log(response)
      if (response.status === 'connected') {
        vm.authorized = true
        vm.getProfile()
        vm.getAlbums()
        vm.realName(this.id)
        vm.realAlbum(this.page)
      } else if (response.status === 'not_authorized') {
        vm.authorized = false
      } else {
        vm.authorized = false
      }
    }
  },
  mounted () {
    this.getPhotosByAlbumId(this.id)
    let vm = this
    // window.fbAsyncInit = () => {
    FB.init({
      appId: '365137310495361',
      cookie: true,
      xfbml: true,
      version: 'v2.8'
    })
    FB.getLoginStatus(function (response) {
      vm.statusChangeCallback(response)
    })
    // }
  }
}
</script>

<style>
</style>
