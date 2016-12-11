้<template>

  <div class="hello">
    <div id="app">
      <div v-if="ready" class="">

        <div class="container">
          <div class="row profile">
		        <div class="col-md-3">
			        <div class="profile-sidebar">
				    <!-- SIDEBAR USERPIC -->
				        <div class="profile-userpic">
					        <img v-if="authorized" :src="'https://graph.facebook.com/' + this.profile.id + '/picture?width=500'" alt="" class="img-responsive"></img>
				        </div>
				<!-- END SIDEBAR USERPIC -->
				<!-- SIDEBAR USER TITLE -->
				        <div class="profile-usertitle" v-if="authorized">
					        <div class="profile-usertitle-name">
						              {{ profile.name }}
					        </div>
					        <div class="profile-usertitle-job">
						              Facebook User
					        </div>
				        </div>
                <div class="profile-usertitle" v-else="authorized">
                  Newbie
				        </div>
				<!-- END SIDEBAR USER TITLE -->
				<!-- SIDEBAR BUTTONS -->
				<div class="profile-userbuttons">
					<button v-if="authorized" @click="logout()" class="btn btn-danger">Logout</button>
          <button v-else="authorized" @click="login()" class="btn btn-primary">Login with Facebook</button>
				</div>
				<!-- END SIDEBAR BUTTONS -->
				<!-- SIDEBAR MENU -->
				<div class="profile-usermenu">
					<ul class="nav">
						<li>
							<a href="#">
							<i class="glyphicon glyphicon-home"></i>
							Home </a>
						</li>
						<li class="active">
							<a href="#" class="disabled">
							<i class="glyphicon glyphicon-folder-open"></i>
							Album of {{realalbum}} </a>
						</li>
						<li>
							<a href="#" class="disabled">
							<i class="glyphicon glyphicon-picture"></i>
							Photos </a>
						</li>
              Contact me : <a href="https://www.facebook.com/PranongOi" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
            <li>
                   <div class="col-sm-12 col-sm-offset-0">
                     <div id="imaginary_container">
                       <div class="input-group stylish-input-group">
                         <input type="text" class="form-control"  placeholder="Search" >
                         <span class="input-group-addon">
                           <button type="submit">
                            <span class="glyphicon glyphicon-search"></span>
                          </button>
                        </span>
                      </div>
                    </div>
                  </div>
						</li>
					</ul>
				</div>
				<!-- END MENU -->
			</div>
		</div>
		<div class="col-md-9">
            <div class="profile-content">
              <div v-if="authorized">
                <!-- <div class="container"> -->
                <div>
                  <ul class="breadcrumb">
                    <li class="completed"><a href="#" class="disabled"><i class="glyphicon glyphicon-home"></i>&nbsp;&nbsp;&nbsp;&nbsp; Home </a></li>
                    <li class="active"><a href="#" class="disabled"><i class="glyphicon glyphicon-folder-open"></i>&nbsp;&nbsp;&nbsp;&nbsp; Album of {{realalbum}}</a></li>
                  </ul>
                </div>
                <!-- </div> -->
                <h1>{{realalbum}}</h1>
              </div>
              <div v-else="authorized">
                <h2>Error 6969 : Please Login with Facebook</h2>
                <img src="/static/error.gif" alt="" height="300"><br><br>
                <img src="/static/fb_login.png" alt="" @click="login()" height="50"></img>
              </div>

              <router-view :id="id"></router-view>

              <div v-if="authorized" class="">
                <div class="row">
                  <div class="col-sm-12 col-sm-offset-0">
                    <div v-for="album in albums" v-if="album.cover_photo" class="box">
                      <router-link to="/imagepage"><img class = "img-rounded" width = '50%' @click="setIdAlbum(album.id)" :src="'https://graph.facebook.com/' + album.cover_photo.id + '/picture'" alt=""></img></router-link>
                      <h2>{{ album.name }}</h2>
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
html, body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: #EDF7FF;
  text-align: center;
  padding: 30px;
}
.box {
  background-color: #cee3f6;
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 10px;
}

#imaginary_container{
    margin-top:20%; /* Don't copy this */
}
.stylish-input-group .input-group-addon{
    background: white !important;
}
.stylish-input-group .form-control{
	border-right:0;
	box-shadow:0 0 0;
	border-color:#ccc;
}
.stylish-input-group button{
    border:0;
    background:transparent;
}
@import url("//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css");
a.btn-social,
.btn-social
{
    border-radius: 50%;
    color: #ffffff !important;
    display: inline-block;
    height: 54px;
    line-height: 54px;
    margin: 8px 4px;
    text-align: center;
    text-decoration: none;
    transition: background-color .3s;
    webkit-transition: background-color .3s;
    width: 54px;
}

.btn-social .fa,.btn-social i
{
    backface-visibility: hidden;
    moz-backface-visibility: hidden;
    ms-transform: scale(1);
    o-transform: scale(1);
    transform: scale(1);
    transition: all .25s;
    webkit-backface-visibility: hidden;
    webkit-transform: scale(1);
    webkit-transition: all .25s;
}
.btn-social:hover,.btn-social:focus
{
    color: #fff;
    outline: none;
    text-decoration: none;
}
.btn-social:hover .fa,.btn-social:focus .fa,.btn-social:hover i,.btn-social:focus i
{
    ms-transform: scale(1.3);
    o-transform: scale(1.3);
    transform: scale(1.3);
    webkit-transform: scale(1.3);
}
.btn-social.btn-xs
{
    font-size: 9px;
    height: 24px;
    line-height: 13px;
    margin: 6px 2px;
    width: 24px;
}
.btn-social.btn-sm
{
    font-size: 13px;
    height: 36px;
    line-height: 18px;
    margin: 6px 2px;
    width: 36px;
}
.btn-social.btn-lg
{
    font-size: 22px;
    height: 72px;
    line-height: 40px;
    margin: 10px 6px;
    width: 72px;
}
.btn-facebook
{
    background-color: #3b5998;
}
.btn-facebook:hover
{
    background-color: #4c70ba;
}
/***
User Profile Sidebar by @keenthemes
A component of Metronic Theme - #1 Selling Bootstrap 3 Admin Theme in Themeforest: http://j.mp/metronictheme
Licensed under MIT
***/
.disabled {
   pointer-events: none;
   cursor: default;
}

body {
  background: #F1F3FA;
}

/* Profile container */
.profile {
  margin: 20px 0;
}

/* Profile sidebar */
.profile-sidebar {
  padding: 20px 0 10px 0;
  background: #fff;
}

.profile-userpic img {
  float: none;
  margin: 0 auto;
  width: 50%;
  height: 50%;
  -webkit-border-radius: 50% !important;
  -moz-border-radius: 50% !important;
  border-radius: 50% !important;
}

.profile-usertitle {
  text-align: center;
  margin-top: 20px;
}

.profile-usertitle-name {
  color: #5a7391;
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 7px;
}

.profile-usertitle-job {
  text-transform: uppercase;
  color: #5b9bd1;
  font-size: 12px;
  font-weight: 600;
  margin-bottom: 15px;
}

.profile-userbuttons {
  text-align: center;
  margin-top: 10px;
}

.profile-userbuttons .btn {
  text-transform: uppercase;
  font-size: 11px;
  font-weight: 600;
  padding: 6px 15px;
  margin-right: 5px;
}

.profile-userbuttons .btn:last-child {
  margin-right: 0px;
}

.profile-usermenu {
  margin-top: 30px;
}

.profile-usermenu ul li {
  border-bottom: 1px solid #f0f4f7;
}

.profile-usermenu ul li:last-child {
  border-bottom: none;
}

.profile-usermenu ul li a {
  color: #93a3b5;
  font-size: 14px;
  font-weight: 400;
}

.profile-usermenu ul li a i {
  margin-right: 8px;
  font-size: 14px;
}

.profile-usermenu ul li a:hover {
  background-color: #fafcfd;
  color: #5b9bd1;
}

.profile-usermenu ul li.active {
  border-bottom: none;
}

.profile-usermenu ul li.active a {
  color: #5b9bd1;
  background-color: #f6f9fb;
  border-left: 2px solid #5b9bd1;
  margin-left: -2px;
}

/* Profile Content */
.profile-content {
  padding: 20px;
  background: #fff;
  min-height: 460px;
}
</style>
