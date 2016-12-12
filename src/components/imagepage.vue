<template>
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
                  <img v-else="authorized" src="/static/man.jpg" class="img-responsive"></img>
				        </div>
				<!-- END SIDEBAR USERPIC -->
				<!-- SIDEBAR USER TITLE -->
				        <div class="profile-usertitle" v-if="authorized">
					        <div class="profile-usertitle-name">
						              <h3>{{ profile.name }}</h3>
					        </div>
					        <div class="profile-usertitle-job">
						              <h4>Facebook User</h4>
					        </div>
				        </div>
                <div class="profile-usertitle" v-else="authorized">
                  <h4>Newbie</h4>
				        </div>
				<!-- END SIDEBAR USER TITLE -->
				<!-- SIDEBAR BUTTONS -->
				<div class="profile-userbuttons">
					<button v-if="authorized" @click="logout()" class="btn btn-danger"><span class="glyphicon glyphicon-off"></span> Logout</button>
          <button v-else="authorized" @click="login()" class="btn btn-primary"><i class="fa fa-facebook"></i> Sign in with Facebook</button>
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
						<li>
              <router-link to="/albumpage">
							<a href="#">
							<i class="glyphicon glyphicon-folder-open"></i>
							Album of {{realalbum}}</a>
            </router-link>
						</li>
						<li class="active">
							<a href="#" class="disabled">
							<i class="glyphicon glyphicon-picture"></i>
							Photos of {{this.realname.name}} </a>
						</li>
            <h4>Contact us : </h4><a href="https://www.facebook.com/PranongOi" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
            <a href="https://www.facebook.com/neativit.keawthong" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
            <a href="https://www.facebook.com/Phanurut.Chamaree" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
            <h4>Nut&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nae&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Petch</h4>
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
                    <li class="completed"><a href="#" class="disabled"><i class="glyphicon glyphicon-folder-open"></i>&nbsp;&nbsp;&nbsp;&nbsp; Album of {{realalbum}}</a></li>
                    <li class="active"><a href="#" class="disabled"><i class="glyphicon glyphicon-picture"></i>&nbsp;&nbsp;&nbsp;&nbsp; Photos of {{this.realname.name}} </a></li>
                  </ul>
                </div>
                <!-- </div> -->
                <h1>{{realalbum}}</h1>
                <h2>{{this.realname.name}}</h2>
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
                        <img class = "img-rounded" width = '50%' :src="photo.images[5].source" alt="" /><br>
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
html, body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: #EDF7FF;
  text-align: center;
  padding: 30px;
}
.breadcrumb {
    padding: 0px;
	background: #D4D4D4;
	list-style: none;
	overflow: hidden;
    margin-top: 20px;
}
.breadcrumb>li+li:before {
	padding: 0;
}
.breadcrumb li {
	float: left;
}
.breadcrumb li.active a {
	background: brown;                   /* fallback color */
	background: #ffc107 ;
}
.breadcrumb li.completed a {
	background: brown;                   /* fallback color */
	background: hsla(153, 57%, 51%, 1);
}
.breadcrumb li.active a:after {
	border-left: 30px solid #ffc107 ;
}
.breadcrumb li.completed a:after {
	border-left: 30px solid hsla(153, 57%, 51%, 1);
}

.breadcrumb li a {
	color: white;
	text-decoration: none;
	padding: 10px 0 10px 45px;
	position: relative;
	display: block;
	float: left;
}
.breadcrumb li a:after {
	content: " ";
	display: block;
	width: 0;
	height: 0;
	border-top: 50px solid transparent;           /* Go big on the size, and let overflow hide */
	border-bottom: 50px solid transparent;
	border-left: 30px solid hsla(0, 0%, 83%, 1);
	position: absolute;
	top: 50%;
	margin-top: -50px;
	left: 100%;
	z-index: 2;
}
.breadcrumb li a:before {
	content: " ";
	display: block;
	width: 0;
	height: 0;
	border-top: 50px solid transparent;           /* Go big on the size, and let overflow hide */
	border-bottom: 50px solid transparent;
	border-left: 30px solid white;
	position: absolute;
	top: 50%;
	margin-top: -50px;
	margin-left: 1px;
	left: 100%;
	z-index: 1;
}
.breadcrumb li:first-child a {
	padding-left: 15px;
}
.breadcrumb li a:hover { background: #ffc107  ; }
.breadcrumb li a:hover:after { border-left-color: #ffc107   !important; }
.box {
  background-color: #FFFFFF;
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 5px;
}/*
img {
  width: 100%;
}*/
h1 {
  font-family: 'Lobster', cursive;
  font-size: 60px;
}
h2 {
  font-family: 'Lobster', cursive;
  font-size: 30px;
}
h3 {
  font-family: 'Raleway', sans-serif;
  font-size: 20px;
}
h4 {
  font-family: 'Raleway', sans-serif;
  font-size: 15px;
}
li {
  font-family: 'Raleway', sans-serif;
  font-size: 15px;
}
</style>
