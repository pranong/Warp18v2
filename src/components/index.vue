<template lang="html">
  <div class="">
    <nav class="navbar navbar-default" role="presentation">
      <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
          <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="#">Warp 18+</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
          <ul class="nav navbar-nav">
            <li class="active"><router-link to="/">Home<span class="sr-only">(current)</span></router-link></li>

            <li class="dropdown">
              <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Select Page <span class="caret"></span></a>
              <ul class="dropdown-menu">
                <li><router-link to="/albumpage"><h5 @click="setIdPage('cupamag')">Cup A</h5></router-link>
                <li><router-link to="/albumpage"><h5 @click="setIdPage('cupdmag')">Cup D</h5></router-link>
                <li><router-link to="/albumpage"><h5 @click="setIdPage('cupemag')">Cup E</h5></router-link>
                <li><router-link to="/albumpage"><h5 @click="setIdPage('Angels.mag2')">Sexy angel</h5></router-link>
                <li><router-link to="/albumpage"><h5 @click="setIdPage('agirlmag')">A girl</h5></router-link>
                <li role="separator" class="divider"></li>
                <li><a href="#">Select other pages</a></li>
              </ul>
            </li>

          </ul>
          <form class="navbar-form navbar-right">
            <div class="form-group">
              <input type="text" class="form-control" placeholder="Input Page_ID" length="2" id="search">
            </div>
              <router-link to="/albumpage"><button type="submit" class="btn btn-default" @click="setIdPage(document.getElementById('search').value)">Search</button></router-link>

              <img v-if="authorized" :src="'https://graph.facebook.com/' + this.profile.id + '/picture?width=40'" alt=""></img>
              <button v-if="authorized" @click="logout()" class="btn btn-danger">Logout</button>
              <button v-else="authorized" @click="login()" class="btn btn-primary">Login</button>

          </form>
        </div><!-- /.navbar-collapse -->
      </div><!-- /.container-fluid -->
    </nav>

    <div class="box">
      <div class="row">
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('cupamag')"><img :src="'https://graph.facebook.com/' + 'cupamag' + '/picture?height=100'" alt=""><br>Cup A</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('cupdmag')"><img :src="'https://graph.facebook.com/' + 'cupdmag' + '/picture?height=100'" alt=""><br>Cup D</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('cupemag')"><img :src="'https://graph.facebook.com/' + 'cupemag' + '/picture?height=100'" alt=""><br>Cup E</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('Angels.mag2')"><img :src="'https://graph.facebook.com/' + 'Angels.mag2' + '/picture?height=100'" alt=""><br>Sexy angel</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('agirlmag')"><img :src="'https://graph.facebook.com/' + 'agirlmag' + '/picture?height=100'" alt=""><br>a girl</h5></router-link>
        </div>
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('1677041912621580')"><img :src="'https://graph.facebook.com/' + '1677041912621580' + '/picture?height=100'" alt=""><br>k-girl</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('1488731304764039')"><img :src="'https://graph.facebook.com/' + '1488731304764039' + '/picture?height=100'" alt=""><br>Makrid</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('1647102408856768')"><img :src="'https://graph.facebook.com/' + '1647102408856768' + '/picture?height=100'" alt=""><br>Leo1991 Studio	</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('812938462158071')"><img :src="'https://graph.facebook.com/' + '812938462158071' + '/picture?height=100'" alt=""><br>Intent Art Gallery</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('798627650219522')"><img :src="'https://graph.facebook.com/' + '798627650219522' + '/picture?height=100'" alt=""><br>Remembrance Life</h5></router-link>
        </div>
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('136276129896856')"><img :src="'https://graph.facebook.com/' + '136276129896856' + '/picture?height=100'" alt=""><br>Chayang Vintage Photographer</img></h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('203413816686770')"><img :src="'https://graph.facebook.com/' + '203413816686770' + '/picture?height=100'" alt=""><br>Guypic</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('695316487270076')"><img :src="'https://graph.facebook.com/' + '695316487270076' + '/picture?height=100'" alt=""><br>Wenge.</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('820679761371818')"><img :src="'https://graph.facebook.com/' + '820679761371818' + '/picture?height=100'" alt=""><br>Wonderful Gallery</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('833137843460451')"><img :src="'https://graph.facebook.com/' + '833137843460451' + '/picture?height=100'" alt=""><br>anp.k</h5></router-link>
        </div>
        <div class="col-xs-6 col-md-3">
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('1666155086932470')"><img :src="'https://graph.facebook.com/' + '1666155086932470' + '/picture?height=100'" alt=""><br>TuaTung Photographer</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('437871096289260')"><img :src="'https://graph.facebook.com/' + '437871096289260' + '/picture?height=100'" alt=""><br>ICEZ PHOTO</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('487003051473640')"><img :src="'https://graph.facebook.com/' + '487003051473640' + '/picture?height=100'" alt=""><br>Slotmxcn</h5></router-link>
            <router-link to="/albumpage" class="thumbnail"><h5 @click="setIdPage('nonlenssnap')"><img :src="'https://graph.facebook.com/' + 'nonlenssnap' + '/picture?height=100'" alt=""><br>F/1352</h5></router-link>
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
  background-color: #B6E3F6;
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 5px;
}/*
img {
  width: 100%;
}*/
</style>
