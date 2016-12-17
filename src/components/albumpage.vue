้<template>
    <div id="app">
      <div v-if="ready" class="">
        <div class="container">
          <div class="row profile">

        <div class="mynav">
          <ul class="breadcrumb">
            <li class="completed"><a href="#"><i class="glyphicon glyphicon-home"></i>&nbsp;&nbsp; Home </a></li>
            <li class="active"><a href="#" class="disabled"><i class="glyphicon glyphicon-folder-open"></i>&nbsp;&nbsp; {{realalbum}}</a></li>
            <div class="navbar-right">
              <li v-if="authorized" @click="logout()" class="navbar-right"><a href="javascript:function() { return false; }"><span class="glyphicon glyphicon-user"></span>&nbsp;&nbsp; Sign out</a></li>
              <li v-else="authorized" @click="login()" class="navbar-right"><a href="javascript:function() { return false; }"><span class="glyphicon glyphicon-user"></span>&nbsp;&nbsp; Sign in</a></li>
            </div>
          </ul>
        </div>

          <div class="profile-content">
            <div v-if="authorized">

              <h1>{{realalbum}}®</h1>
            </div>
            <div v-else="authorized">
              <h5>Error (.)(.) : Please Sign in with Facebook</h5>
              <img src="/static/error.gif" alt="" height="300"><br><br>
              <img src="/static/fb_login.png" alt="" @click="login()" height="50"></img>
            </div>

            <router-view :id="id"></router-view>

            <div v-if="authorized" class="">
              <div class="row">
                <div class="col-sm-12 col-sm-offset-0">

                  <div v-if="nextJson">
                    <div v-if="changen">
                      <div v-for="album in nextJson" v-if="album.cover_photo" class="box">
                        <router-link to="/imagepage" STYLE="text-decoration: none"><img class = "img-rounded" width = '50%' @click="setIdAlbum(album.id)" :src="'https://graph.facebook.com/' + album.cover_photo.id + '/picture'" alt=""></img><h2>● {{ album.name }} ●</h2></router-link>
                      </div>
                    </div>
                    <div v-if="changep">
                      <div v-for="album in preJson" v-if="album.cover_photo" class="box">
                        <router-link to="/imagepage" STYLE="text-decoration: none"><img class = "img-rounded" width = '50%' @click="setIdAlbum(album.id)" :src="'https://graph.facebook.com/' + album.cover_photo.id + '/picture'" alt=""></img><h2>● {{ album.name }} ●</h2></router-link>
                      </div>
                    </div>
                  </div>

                  <div v-else="nextJson">
                  <div v-for="album in albums" v-if="album.cover_photo" class="box">
                    <router-link to="/imagepage" STYLE="text-decoration: none"><img class = "img-rounded" width = '50%' @click="setIdAlbum(album.id)" :src="'https://graph.facebook.com/' + album.cover_photo.id + '/picture'" alt=""></img><h2>● {{ album.name }} ●</h2></router-link>
                  </div>
                  </div>

                  <div class="btn-group demoPadder" role="group" aria-label="Basic example"v-if="check">
                    <button type="button" class="btn btn-default" @click="goPre()"><span class="glyphicon glyphicon-menu-left"></span></button>
                    <button type="button" class="btn btn-default" @click="goNext()"><span class="glyphicon glyphicon-menu-right"></span></button>
                  </div>
                  <div class="btn-group demoPadder" role="group" aria-label="Basic example"v-else="check">
                    <button type="button" class="btn btn-default" @click="goPre();"><span class="glyphicon glyphicon-menu-left">{{now}}</span></button>
                    <button type="button" class="btn btn-default" @click="goNext();"><span class="glyphicon glyphicon-menu-right">{{now}}</span></button>
                  </div>
                </div>
              </div>
            </div>
          </div>
	    </div>
    </div><br><br>
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
  name: 'albumpage',
  props: ['id', 'setIdAlbum', 'page', 'setIdPage'],
  components: {
    Hello
  },
  data () {
    return {
      albums: [],
      photos: [],
      pagename: [],
      nextJson: null,
      preJson: null,
      newnext: null,
      newpre: null,
      now: '',
      next: '',
      pre: '',
      paging: '',
      realalbum: '',
      check: false,
      changep: false,
      changen: false,
      profile: {},
      ready: false,
      re: true,
      authorized: false
    }
  },
  methods: {
    getAlbums () {
      let vm = this
      FB.api('/' + this.page + '/albums', {fields: ['cover_photo', 'name']}, function (response) {
        console.log(response)
        vm.$set(vm, 'albums', response.data)
      })
    },
    getNewAlbums () {
      let vm = this
      FB.api('/' + this.page + '/albums', {fields: ['cover_photo', 'name']}, function (response) {
        console.log(response)
        vm.$set(vm, 'newalbums', response.data)
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
    getNext () {
      let vm = this
      FB.api('/' + this.page + '/albums', function (response) {
        console.log(response.paging.cursors.after)
        vm.$set(vm, 'next', response.paging.cursors.after)
      })
    },
    getPre () {
      let vm = this
      FB.api('/' + this.page + '/albums', function (response) {
        console.log(response.paging.cursors.before)
        vm.$set(vm, 'pre', response.paging.cursors.before)
      })
    },
    newgetNext () {
      let vm = this
      FB.api(vm.now, function (response) {
        console.log(response.paging.cursors.after)
        vm.$set(vm, 'newnext', response.paging.cursors.after)
        vm.next = response.paging.cursors.after
        console.log(vm.next)
      })
    },
    newgetPre () {
      let vm = this
      FB.api(vm.now, function (response) {
        console.log(response.paging.cursors.before)
        vm.$set(vm, 'newpre', response.paging.cursors.before)
        vm.pre = response.paging.cursors.before
        console.log(vm.pre)
      })
    },
    goNext () {
      let vm = this
      FB.api('/' + this.page + '/albums?pretty=0&fields=cover_photo%2Cname&limit=25&after=' + this.next, function (response) {
        console.log(response)
        vm.check = true
        vm.$set(vm, 'nextJson', response.data)
        vm.now = '/' + vm.page + '/albums?pretty=0&fields=cover_photo%2Cname&limit=25&after=' + vm.next
        console.log(vm.now)
        vm.newgetNext()
        vm.newgetPre()
        vm.changen = true
        vm.changep = false
      })
    },
    goPre () {
      let vm = this
      FB.api('/' + this.page + '/albums?pretty=0&fields=cover_photo%2Cname&limit=25&before=' + this.pre, function (response) {
        console.log(response)
        vm.check = true
        vm.$set(vm, 'preJson', response.data)
        vm.now = '/' + vm.page + '/albums?pretty=0&fields=cover_photo%2Cname&limit=25&before=' + vm.pre
        console.log(vm.now)
        vm.newgetNext()
        vm.newgetPre()
        vm.changen = false
        vm.changep = true
      })
    },
    getPhotosByAlbumId (albumId) {
      let vm = this
      FB.api('/' + albumId + '/photos', {fields: ['images']}, function (response) {
        console.log(response)
        vm.$set(vm, 'photos', response.data)
        console.log(albumId)
        return albumId
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
      vm.re = false
      console.log('statusChangeCallback')
      console.log(response)
      console.log(vm.re)
      if (response.status === 'connected') {
        vm.authorized = true
        vm.getProfile()
        vm.getAlbums()
        vm.getNext()
        vm.getPre()
        vm.realAlbum(this.page)
      } else if (response.status === 'not_authorized') {
        vm.authorized = false
      } else {
        vm.authorized = false
      }
    }
  },
  mounted () {
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
