<template>
  <div class="root">
    <div class="field">
      <div class="title">
        <h1>
          Simon Game
        </h1> 
      </div>
      <div class="gameZone">
        <div class="circle">
          <div @click="tap($event)" :class="objData[0] ? 'lighter' : ''" class="btn  btnRed">1
            <audio v-if="objData[0]" play autoplay src="../audio/doo.mp3"></audio>
          </div>
          <div @click="tap($event)" :class="objData[1] ? 'lighter' : ''" class="btn btnBlue">2
            <audio v-if="objData[1]"  play autoplay src="../audio/ree.mp3"></audio>
          </div>
          <div @click="tap($event)" :class="objData[2] ? 'lighter' : ''" class="btn  btnGreen">3
            <audio v-if="objData[2]"  play autoplay src="../audio/mii.mp3"></audio>
          </div>
          <div @click="tap($event)" :class="objData[3] ? 'lighter' : ''" class="btn btnYellow">4
            <audio v-if="objData[3]"  play  autoplay src="../audio/faa.mp3"></audio>
          </div>
        </div>
        <div class="gameInfo">
          <div class="round"></div>
          <button @click="startGame" class="btnStart">
            Start
          </button>
          <div class="info"></div>
          <div class="gameOpt">
            <h2>Game Options</h2>
            <p>
              Tap number {{simArr.length}}
            </p>
            <p v-if="!correct">
              you lose on {{simArr.length}}
            </p>
            <input type="radio" name= 'level'  v-model="level" value="1">
            Level 1 <br>
            <input type="radio" name= 'level'  v-model="level" value="2">
            Level 2 <br>
            <input type="radio" name= 'level'  v-model="level" value="3">
            Level 3 <br>
          </div>
        </div>
        <audio src="">
          <source src="../audio/faa.mp3">
        </audio>
        
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'simonComponent',  
  data() {
    return  {
      level: '1',
      simArr:[],
      userArr:[], 
      correct: true,
      started: false,
      objData: [ false, false, false, false],
      musicData: ['/media/faa.32550a54.mp3', '/media/mii.7edb93d1.mp3','/media/ree.0c7256bb.mp3','/media/doo.1107b94f.mp3']
    }
    
  },
  methods: {
      startGame() {
        var audio = new Audio()
        var int;
        console.log(this.level)
        switch(this.level) {
          case '1': int = 1500
          break;
          case '2': int = 1000
          break;
          case '3': int = 400
          break;
        }
        this.simArr = []
        this.userArr = []
        this.correct = true
        this.started = true
        const index = Math.floor(Math.random() * 4 ) + 1
        this.simArr.push(index)
        this.objData[index-1] = true
        audio.src = this.musicData[index-1]
        audio.play()
        setTimeout(() => {
          this.objData[index-1] = false
        }, int)
      },
      tap($e) {
        let audioSrc;
        const index = $e.target.innerText
        switch (index) {
          case '1': audioSrc='/media/faa.32550a54.mp3'
          break;
          case '2': audioSrc='/media/mii.7edb93d1.mp3'
          break;
          case '3': audioSrc= '/media/ree.0c7256bb.mp3'
          break;
          case '4': audioSrc= '/media/doo.1107b94f.mp3'
          break;
        }
        var audio = new Audio()
        audio.src = audioSrc
        console.log(audio.src)
        audio.play()
        if(this.started && this.simArr.length !== this.userArr.length) {
          const index = $e.target.innerText
          this.userArr.push(index)
          }
        if(this.simArr.length == this.userArr.length && this.simArr.join() !== this.userArr.join()) {
          this.correct = false 
          this.started = false
          this.userArr.length  = 0
        } else if(this.started && this.correct &&  this.simArr.length == this.userArr.length &&  this.simArr.join() == this.userArr.join()) {
          this.userArr.length  = 0
          this.continueGame();
        }
            
      },

      continueGame() {
        console.log('game continued')
        console.log(this.userArr)
          const index = Math.floor(Math.random() * 4 ) + 1
          this.simArr.push(index)
          this.addClass()
      },
      addClass() {
        let interval;
        switch(this.level) {
          case '1':
            interval = 1500
            break;
          case '2':
            interval = 1000
            break;
          case '3':
            interval = 400
            break;
        }
        let i= 0;
        var audio = new Audio()

        var changeColor = setInterval(()=> {
          if (i>=this.simArr.length){
            clearInterval(changeColor)
            console.log('the end')
            this.objData.forEach((item, i, arr) => {arr[i] = false})
          }else {
            this.objData.forEach((item, i, arr) => {arr[i] = false})
            this.objData[this.simArr[i]-1]=true 
            audio.src = this.musicData[this.simArr[i]-1]
            audio.play()
            i++}
        }, interval) 
        
    },
  }
  
}
</script>

<style lang="sass">
h2
  font-size: 20px
.field

.gameZone
  display: flex
  justify-content: center 
.btn
  filter: brightness(0.7)    
  user-select: none 
  color: transparent
  width: 160px
  height: 160px
  &:active
    filter: brightness(1.8)!important
.btnBlue
  background-color: blue
.btnGreen
 background-color: green
 
.btnRed
 background-color: red
.btnYellow
 background-color: yellow   
.circle
  display: flex
  flex-wrap: wrap
  width: 320px
  height: 320px
  border-radius: 50%
  overflow: hidden
.lighter
  filter: brightness(1.8)!important
</style>
