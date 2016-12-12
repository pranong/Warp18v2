้<template>
    <div id="app">
      <div v-if="ready" class="">
        <div class="container">
          <div class="row profile">
            <div class="Fixed">
		          <div class="col-md-5">
			        <div class="profile-sidebar">
				      <!-- รูป user -->
              <div class="profile-userpic">
                <img v-if="authorized" :src="'https://graph.facebook.com/' + this.profile.id + '/picture?width=500'" alt="" class="img-responsive"></img>
                <img v-else="authorized" src="/static/warp.gif" class="img-responsive"></img>
              </div>
				      <!--ชื่อ-->
				      <div class="profile-usertitle" >
					      <div class="profile-usertitle-name" v-if="authorized">
						      <a :href="'https://www.facebook.com/' + this.profile.id" STYLE="text-decoration: none"><h3>{{ profile.name }}</h3></a>
					      </div>
                <div class="profile-usertitle-name" v-else="authorized">
						      <h3>Unknow</h3>
					      </div>
					      <div class="profile-usertitle-job" v-if="authorized">
						      <h4>Facebook User</h4>
					      </div>
				      </div>
				      <!-- ปุ่ม Login-->
				      <div class="profile-userbuttons">
					      <button v-if="authorized" @click="logout()" class="btn btn-danger"><span class="glyphicon glyphicon-off"></span> Logout</button>
                <button v-else="authorized" @click="login()" class="btn btn-primary"><i class="fa fa-facebook"></i> Sign in with Facebook</button>
				      </div>
				      <!-- MENU -->
				      <div class="profile-usermenu">
					      <ul class="nav">
					      <li><a href="#"><i class="glyphicon glyphicon-home"></i>Home </a></li>
					        <li class="active"><a href="#" class="disabled"><i class="glyphicon glyphicon-folder-open"></i>Album of {{realalbum}}® </a></li>
					        <li><a href="#" class="disabled"><i class="glyphicon glyphicon-picture"></i>Photos </a></li>
                  <h4>Contact us : </h4><a href="https://www.facebook.com/PranongOi" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
                  <a href="https://www.facebook.com/neativit.keawthong" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
                  <a href="https://www.facebook.com/Phanurut.Chamaree" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
                  <h4><a href="https://www.facebook.com/PranongOi" STYLE="text-decoration: none">Nut</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.facebook.com/neativit.keawthong" STYLE="text-decoration: none">Nae</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.facebook.com/Phanurut.Chamaree" STYLE="text-decoration: none">Petch</a></h4>
					      </ul>
				      </div>
            </div>
			    </div>
		    </div>
        <div class="col-md-3">
        </div>
		    <div class="col-md-9">
          <div class="profile-content">
            <div v-if="authorized">
              <div>
                <ul class="breadcrumb">
                  <li class="completed"><a href="#" class="disabled"><i class="glyphicon glyphicon-home"></i>&nbsp;&nbsp;&nbsp;&nbsp; Home </a></li>
                  <li class="active"><a href="#" class="disabled"><i class="glyphicon glyphicon-folder-open"></i>&nbsp;&nbsp;&nbsp;&nbsp; Album of {{realalbum}}</a></li>
                </ul>
              </div>
              <h1>{{realalbum}}®</h1>
            </div>
            <div v-else="authorized">
              <h5>Error (.)(.) : Please Sign in with&nbsp;&nbsp;&nbsp;&nbsp; Facebook</h5>
              <img src="/static/error.gif" alt="" height="300"><br><br>
              <img src="/static/fb_login.png" alt="" @click="login()" height="50"></img>
            </div>

            <router-view :id="id"></router-view>

            <div v-if="authorized" class="">
              <div class="row">
                <div class="col-sm-12 col-sm-offset-0">
                  <div v-for="album in albums" v-if="album.cover_photo" class="box">
                    <router-link to="/imagepage" STYLE="text-decoration: none"><img class = "img-rounded" width = '50%' @click="setIdAlbum(album.id)" :src="'https://graph.facebook.com/' + album.cover_photo.id + '/picture'" alt=""></img><h2>● {{ album.name }} ●</h2></router-link>
                  </div>
                  <div class="btn-group demoPadder" role="group" aria-label="Basic example">
                    <button type="button" class="btn btn-default" @click="goPre();"><span class="glyphicon glyphicon-menu-left"></span>{{this.pre}}</button>
                    <button type="button" class="btn btn-default" @click="goNext();"><span class="glyphicon glyphicon-menu-right"></span>{{this.next}}</button>
                  </div>
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
      next: '',
      realalbum: '',
      pre: '',
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
    realAlbum (page) {
      let vm = this
      FB.api('/' + page, function (response) {
        console.log(response)
        vm.$set(vm, 'realalbum', response.name)
        console.log(response.name)
      })
    },
    getnewAlbums () {
      let vm = this
      FB.api('/' + this.page + '/albums', {fields: ['cover_photo', 'name']}, function (response) {
        console.log(response)
        vm.$set(vm, 'albums', response.data)
      })
    },
    getNext () {
      let vm = this
      FB.api('/' + this.page + '/albums', function (response) {
        console.log(response)
        vm.$set(vm, 'next', response.paging.cursors.after)
        console.log(response.paging.cursors.after)
      })
    },
    getPre () {
      let vm = this
      FB.api('/' + this.page + '/albums', function (response) {
        console.log(response)
        vm.$set(vm, 'pre', response.paging.cursors.before)
        console.log(response.paging.cursors.before)
      })
    },
    goNext () {
      let vm = this
      FB.api('/' + this.page + '/albums?pretty=0&limit=25&after=' + this.next, function (response) {
        console.log(response)
        vm.$set(vm, 'goNext', response)
        console.log(response)
      })
    },
    goPre () {
      let vm = this
      FB.api('/' + this.page + '/albums?pretty=0&limit=25&before=' + this.next, function (response) {
        console.log(response)
        vm.$set(vm, 'goPre', response)
        console.log(response)
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
