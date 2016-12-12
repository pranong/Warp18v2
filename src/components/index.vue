<template lang="html">
  <div id="app">
    <div class="container">
      <div class="row profile">
    <div class="col-md-3">
      <div class="Fixed">
      <div class="col-md-5">
        <div class="profile-sidebar">
      <!-- SIDEBAR USERPIC -->
          <div class="profile-userpic">
            <img v-if="authorized" :src="'https://graph.facebook.com/' + this.profile.id + '/picture?width=500'" alt="" class="img-responsive"></img>
            <img v-else="authorized" src="/static/warp.gif" class="img-responsive"></img>
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
      <li class="active">
        <a href="#" class="disabled">
        <i class="glyphicon glyphicon-folder-open"></i>
        Album</a>
      </li>
      <li>
        <a href="#" class="disabled">
        <i class="glyphicon glyphicon-picture"></i>
        Photos </a>
      </li>
        <h4>Contact us : </h4><a href="https://www.facebook.com/PranongOi" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
        <a href="https://www.facebook.com/neativit.keawthong" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
        <a href="https://www.facebook.com/Phanurut.Chamaree" target="_blank" class="btn-social btn-facebook"><i class="fa fa-facebook"></i></a>
        <h4><a href="https://www.facebook.com/PranongOi" STYLE="text-decoration: none">Nut</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.facebook.com/neativit.keawthong" STYLE="text-decoration: none">Nae</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.facebook.com/Phanurut.Chamaree" STYLE="text-decoration: none">Petch</a></h4>
      <li>

      </li>
    </ul>
  </div>
</div>
  <!-- END MENU -->
</div>
</div>
    </div>
    <div class="col-md-9">
      <div class="profile-content">
      <div class="row">
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('cupamag')"><img :src="'https://graph.facebook.com/' + 'cupamag' + '/picture?height=100'" alt=""><br>Cup A</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('cupdmag')"><img :src="'https://graph.facebook.com/' + 'cupdmag' + '/picture?height=100'" alt=""><br>Cup D</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('cupemag')"><img :src="'https://graph.facebook.com/' + 'cupemag' + '/picture?height=100'" alt=""><br>Cup E</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('Angels.mag2')"><img :src="'https://graph.facebook.com/' + 'Angels.mag2' + '/picture?height=100'" alt=""><br>Sexy angel</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('agirlmag')"><img :src="'https://graph.facebook.com/' + 'agirlmag' + '/picture?height=100'" alt=""><br>a girl</h4></router-link>
        </div>
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('1677041912621580')"><img :src="'https://graph.facebook.com/' + '1677041912621580' + '/picture?height=100'" alt=""><br>k-girl</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('1488731304764039')"><img :src="'https://graph.facebook.com/' + '1488731304764039' + '/picture?height=100'" alt=""><br>Makrid</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('1647102408856768')"><img :src="'https://graph.facebook.com/' + '1647102408856768' + '/picture?height=100'" alt=""><br>Leo1991 Studio	</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('812938462158071')"><img :src="'https://graph.facebook.com/' + '812938462158071' + '/picture?height=100'" alt=""><br>Intent Art Gallery</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('798627650219522')"><img :src="'https://graph.facebook.com/' + '798627650219522' + '/picture?height=100'" alt=""><br>Remembrance Life</h4></router-link>
        </div>
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('136276129896856')"><img :src="'https://graph.facebook.com/' + '136276129896856' + '/picture?height=100'" alt=""><br>Chayang Vintage Photographer</img></h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('203413816686770')"><img :src="'https://graph.facebook.com/' + '203413816686770' + '/picture?height=100'" alt=""><br>Guypic</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('695316487270076')"><img :src="'https://graph.facebook.com/' + '695316487270076' + '/picture?height=100'" alt=""><br>Wenge.</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('820679761371818')"><img :src="'https://graph.facebook.com/' + '820679761371818' + '/picture?height=100'" alt=""><br>Wonderful Gallery</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('833137843460451')"><img :src="'https://graph.facebook.com/' + '833137843460451' + '/picture?height=100'" alt=""><br>anp.k</h4></router-link>
        </div>
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('1666155086932470')"><img :src="'https://graph.facebook.com/' + '1666155086932470' + '/picture?height=100'" alt=""><br>TuaTung Photographer</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('437871096289260')"><img :src="'https://graph.facebook.com/' + '437871096289260' + '/picture?height=100'" alt=""><br>ICEZ PHOTO</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('487003051473640')"><img :src="'https://graph.facebook.com/' + '487003051473640' + '/picture?height=100'" alt=""><br>Slotmxcn</h4></router-link>
            <router-link to="/albumpage" class="thumbnail"><h4 @click="setIdPage('nonlenssnap')"><img :src="'https://graph.facebook.com/' + 'nonlenssnap' + '/picture?height=100'" alt=""><br>F/1352</h4></router-link>
        </div>
      </div>
    </div>
  </div>
</div>
</div>
</div>
</template>

<script>
/* global FB */
import Hello from './Hello'
export default {
  name: 'index',
  props: ['setIdPage'],
  components: {
    Hello
  },
  data () {
    return {
      pagename: [],
      profile: {},
      ready: false,
      re: true,
      authorized: false
    }
  },
  methods: {
    getProfilePhoto (albumId) {
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
      vm.re = false
      console.log('statusChangeCallback')
      console.log(response)
      console.log(vm.re)
      if (response.status === 'connected') {
        vm.authorized = true
        vm.getProfile()
        vm.getAlbums()
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
  background-color: #FFFFFF;
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 5px;
}/*
img {
  width: 100%;
}*/
</style>
