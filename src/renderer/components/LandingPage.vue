<template>
  <div id="wrapper">
    <!--<img id="logo" src="~@/assets/logo.png" alt="electron-vue">-->
    <h1>{{settings.content.messages.header}}</h1>
    <main>
      <div class="content">
        <div v-show="showGrid" class="grid">
          <div v-for="(item, index) in reverseMessages" track-by="uid"  
               :id="'msg-'+item.uid"
               class="grid__item">
              <div :id="item.uid"
               :class="'tooltip tooltip--'+getToolTipName(item.body)"
               :data-type="getToolTipName(item.body)"
               :style="">
                <div class="tooltip__trigger" :class="{active: item.isActive}">
                  <span class="phone">📱</span>
                  <span class="tooltip__trigger-text">{{item.author}}</span>
                  <span class="leaf-separator">
                    <svg xmlns="http://www.w3.org/2000/svg" data-name="Layer 1" viewBox="0 0 100 125" x="0px" y="0px"><path d="M27.67,52.16l1.24-.84A17.5,17.5,0,0,0,33.52,27l-.84-1.24-1.24.84a17.52,17.52,0,0,0-4.61,24.32ZM31.86,30a14.51,14.51,0,0,1-3.38,17.83A14.53,14.53,0,0,1,31.86,30Z"/><path d="M64,33.07A17.5,17.5,0,0,0,68.59,8.75l-.84-1.24-1.24.84A17.52,17.52,0,0,0,61.9,32.67l.84,1.24Zm3-21.27a14.51,14.51,0,0,1-3.38,17.83A14.53,14.53,0,0,1,66.93,11.8Z"/><path d="M26.41,60.08l-1.24.84L26,62.16a17.38,17.38,0,0,0,11.2,7.34,17.63,17.63,0,0,0,3.3.31,17.37,17.37,0,0,0,9.82-3l1.24-.84-.84-1.24A17.52,17.52,0,0,0,26.41,60.08Zm11.37,6.48a14.38,14.38,0,0,1-8.32-4.82,14.53,14.53,0,0,1,17.83,3.38A14.38,14.38,0,0,1,37.77,66.55Z"/><path d="M69.44,30.69,68.69,32l.88.5a80.36,80.36,0,0,0-22.93,7.67,17.51,17.51,0,0,0,3.57-23.53l-.84-1.24-1.24.84a17.5,17.5,0,0,0-4.61,24.32l.64.94A80.66,80.66,0,0,0,21.05,60.8a17.47,17.47,0,0,0-2.2-15.14L18,44.43l-1.24.84a17.5,17.5,0,0,0-4.61,24.32L13,70.83,14.24,70c.2-.13.37-.29.56-.43A79.93,79.93,0,0,0,9.51,79.7a81.42,81.42,0,0,0-4.13,12l2.9.75a78.58,78.58,0,0,1,4-11.6c.61-1.41,1.27-2.79,2-4.15a17.37,17.37,0,0,0,11,7.08,17.63,17.63,0,0,0,3.3.31,17.37,17.37,0,0,0,9.82-3l1.24-.84L38.73,79A17.52,17.52,0,0,0,16,73.45a77.41,77.41,0,0,1,24.22-26l.71,1a17.38,17.38,0,0,0,11.2,7.34,17.73,17.73,0,0,0,3.3.31,17.38,17.38,0,0,0,9.82-3l1.24-.84L65.63,51a17.47,17.47,0,0,0-20.77-6.46,77.46,77.46,0,0,1,18.73-7.67A17.47,17.47,0,0,0,84.87,48.55l1.45-.39-.39-1.45a17.38,17.38,0,0,0-8.19-10.6,17.69,17.69,0,0,0-2.91-1.32l.34,0v-.05a17.5,17.5,0,0,0,18.72-8.43l.75-1.3-1.3-.75A17.52,17.52,0,0,0,69.44,30.69ZM43.73,28a14.39,14.39,0,0,1,4.82-8.32,14.53,14.53,0,0,1-3.38,17.83A14.39,14.39,0,0,1,43.73,28ZM13.82,66.54A14.51,14.51,0,0,1,17.2,48.71,14.53,14.53,0,0,1,13.82,66.54Zm21.47,12.9a14.51,14.51,0,0,1-17.83-3.38A14.53,14.53,0,0,1,35.29,79.44Zm26.9-28a14.51,14.51,0,0,1-17.83-3.38A14.53,14.53,0,0,1,62.19,51.43ZM82.53,46a14.53,14.53,0,0,1-15.74-9,14.51,14.51,0,0,1,15.74,9Zm0-14.44a14.39,14.39,0,0,1-9.59-.7A14.53,14.53,0,0,1,90.41,26.1,14.39,14.39,0,0,1,82.49,31.55Z"/></svg>
                  </span>
                  <span class="time">
                    <timeago :since="item.createdAt" :auto-update="60"></timeago>
                  </span>
                </div>
                <div class="tooltip__base">
                  <component :is="getToolTipName(item.body)"></component>
                  <div class="tooltip__content" v-html="emojify(item.body)"></div>
                </div>
              </div>
            </div>
        </div>
      </div>
      <div class="mail">
        <div class="mail-wrapper top-left" :title="mail[mail.length-1].author.name">
          <img :src="mail[mail.length-1].image.url" alt="" >
        </div>
        <div class="mail-wrapper top-right" :title="mail[mail.length-2].author.name">
          <img :src="mail[mail.length-2].image.url" alt="" >
        </div>
        <div class="mail-wrapper bottom-left" :title="mail[mail.length-3].author.name">
          <img :src="mail[mail.length-3].image.url" alt="" >
        </div>
        <div class="mail-wrapper bottom-right" :title="mail[mail.length-4].author.name">
          <img :src="mail[mail.length-4].image.url" alt="" >
        </div>
      </div>
    </main>
    <footer>
      {{settings.content.messages.footer}}
    </footer>
  </div>
</template>

<script>
  import settings from '@/lib/settings'
  import Tooltip from '../lib/tips.js'
  var Chance = require('chance')
  var chance = new Chance()
  let tipOptions = ['cora','smaug']
  var Firebase = require('firebase')
  const emojione = require('emojione')
  var firebaseApp = Firebase.initializeApp({
        databaseURL: 'https://sms-feed.firebaseio.com'
      })
  var db = firebaseApp.database()
  function map_range(value, low1, high1, low2, high2) {
      return Math.floor(low2 + (high2 - low2) * (value - low1) / (high1 - low1));
  }

  export default {
    name: 'landing-page',
    firebase: {
      messages: {
        source: db.ref('sms'),
        readyCallback: function () {
          let vm = this
          console.log('data loaded')
          this.$nextTick(function () {
            vm.initTips()
            vm.rotateDisplay()
          })
          
        }
      },
      mail: {
        source: db.ref('mail'),
        readyCallback: function () {
          let vm = this
          console.log('mail loaded')
          this.$nextTick(function () {
          })
        }
      }
    },
    data() {
      return {
        settings,
        showGrid: false,
        newMessages: [],
        messages: [],
        mail:[],
        tips: {},
        clear: false,
        newMessageReceived: true,
        rotateTimeout:null
      }
    },
    watch: {
      messages() {
        let vm = this
        console.log( 'Change detected...' )
        setTimeout(()=>{
          vm.resetGrid(true)
        }, 1000)
      }
    },
    components: {  },
    computed: {
       reverseMessages() {
         if(this.clear){
           return []
         } else if(this.newMessageReceived){
           return this.messages.slice(this.messages.length - 9).reverse()
         } else {
           return chance.pickset(this.messages, 9)
         }
       },
       randomMessage(){
         return chance.pickset(this.messages, 9)
       }
    },
    mounted(){
     /*
      a Pen by DIACO : twitter.com/Diaco_ml  ||  codepen.io/MAW
      powered by GSAP : https://www.greensock.com/
      */
      
    },
    methods: {
      getRandPos(){
         return chance.pickone(['10', '-20', '-5', '30', '-20'])
      },
      initTips(){
        let vm = this
        const tooltips = Array.from(document.querySelectorAll('.tooltip'))
        TweenMax.set(tooltips, {opacity:1, y:0})
        const init = (() => tooltips.forEach(t =>{
          vm.tips[t.id] = new Tooltip(t)
          vm.tips[t.id].hide()
          TweenMax.set(vm.tips[t.id].DOM.trigger, {opacity:0})
        }))()
        setTimeout(()=>{this.showGrid = true}, 500)
        console.log(vm.tips)
      },
      rotateDisplay(){
        let vm = this
        var obj_keys = Object.keys(this.tips)
        if (obj_keys.length > 0) {
          var ran_key = obj_keys[Math.floor(Math.random() * obj_keys.length)]
          TweenLite.to(this.tips[ran_key].DOM.trigger, 1, {opacity:1, ease:Power2.easeInOut, onComplete: ()=>{
            this.tips[ran_key].show()
            this.rotateTimeout = setTimeout(()=>{
              //this.tips[ran_key].hide()
              delete(this.tips[ran_key])
              this.rotateDisplay()
            }, chance.pickone(['1000', '1500', '500', '2500', '900']))
          }})
        } else {
          this.resetGrid(false)
        }
      },
      resetGrid(newMessage){
        let vm = this
        clearTimeout(this.rotateTimeout)
        const tooltips = Array.from(document.querySelectorAll('.tooltip'))
        TweenMax.staggerTo(tooltips, 0.5, {opacity:0, y:-100, ease:Back.easeIn}, 0.1,  ()=>{
          this.clear = true
          this.showGrid = false
          this.newMessageReceived = newMessage
          this.clear = false
          this.$nextTick(function () {
            vm.initTips()
            vm.rotateDisplay()
          })
        })
      },
      getToolTipName(content){
        //console.log(index)
        //console.log(map_range(index, 0, this.messages.length-1, 0, tipOptions.length-1))
        if(content.length > 100){
          return 'cora'
        } else {
          return 'smaug'
        }
        //return tipOptions[map_range(index, 0, this.messages.length-1, 0, tipOptions.length-1)]
      },
      emojify(stringInput){
        return emojione.unicodeToImage(stringInput)
      },
      sendMessage () {
        this.$spacebro.emit(this.$spacebro.config.client.out.outMessage.eventName, {
          message: 'thank you'.split('').sort(function () { return 0.5 - Math.random() }).join('')
        })
      },
      sendMedia () {
        this.$spacebro.emit(this.$spacebro.config.client.out.outMedia.eventName, {
          url: 'http://lorempixel.com/400/200/business/spacebro-is-' + 'awesome'.split('').sort(function () { return 0.5 - Math.random() }).join(''),
          file: 'file.jpg',
          type: 'image/jpg',
          details: {
            width: 320,
            height: 240
          },
          meta: {
            title: 'galaxy',
            description: 'galaxy image'
          }
        })
      },
      open (link) {
        this.$electron.shell.openExternal(link)
      }
    }
  }
</script>

<style>
  @import '../style/css/normalize';
  @import '../style/css/demo';
  @import '../style/css/component';
  @import '../../../node_modules/animate.css/animate.min.css';

  @import url('https://fonts.googleapis.com/css?family=Amatic+SC:400,700|Sedgwick+Ave|VT323&subset=latin-ext');
  
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }
  .dot{
    width:35px;
    height:35px;
    position:absolute;
    background: url(http://www.clipartqueen.com/image-files/red-lobed-fall-clipart-leaf.png);
    background-size: 100% 100%;
  }
  h1{
      font-family: 'Amatic SC', cursive;
      text-align: center;
      font-weight: 700;
      font-size: 52px;
      color: rgba(29, 76, 154, 1);
      z-index:10;
      position: relative;
  }
  .mail{
    position: absolute;
    top: 0;
    left: 0;
    z-index:0;
    width: 100vw;
    height: 100vh;
  }

  .mail-wrapper{
    background: #fff;
    display: inline;
    float: left;
    margin: 0 0 27px 30px;
    width: 30%;
    padding: 10px 10px 15px;
    text-align: center;
    font-family: "Marker Felt", sans-serif;
    text-decoration: none;
    color: #333;
    font-size: 18px;
    -webkit-box-shadow: 0 3px 6px rgba(0,0,0,.25);
    -moz-box-shadow: 0 3px 6px rgba(0,0,0,.25);
    -webkit-transform: rotate(-2deg);
    -webkit-transition: -webkit-transform .15s linear;
    -moz-transform: rotate(-2deg);
    overflow: hidden;
  }
  .mail-wrapper.top-left{
    left: -17px;
    position: absolute;
    transform: rotate(-2deg);
    top: -8px;
    width: 20%;
  }
  .mail-wrapper.top-right{
    right: -17px;
    position: absolute;
    transform: rotate(8deg);
    top: 10px;
    width: 20%;
  }
  .mail-wrapper.bottom-right{
    right: -17px;
    position: absolute;
    transform: rotate(4deg);
    bottom: -5px;
    width: 15%;
  }
  .mail-wrapper.bottom-left{
    left: -37px;
    position: absolute;
    transform: rotate(-18deg);
    bottom: -10px;
    width: 25%;
  }
  
  .mail-wrapper:after {
    content: attr(title);
  }
  .mail-wrapper img{
    border: 0;
    object-fit: contain;
    width: 100%;
  }

  #wrapper {
    background:
      radial-gradient( ellipse at top left, #f9edd2 40%, rgba(229, 229, 229, .9) 100% );
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    justify-content: space-between;
    height:85%;
  }

  main .content {
    width: 100vw;
    z-index:3;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  p {
    color: black;
    margin-bottom: 10px;
  }

  footer{
    font-family: 'VT323', monospace;
    text-align:center;
    font-size:21px;
  }

  button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
</style>
