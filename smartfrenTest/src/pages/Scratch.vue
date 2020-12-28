<template>
<div class="q-pa-md row items-start q-gutter-md flex flex-center" key="render">
    <q-card class="my-card">
      <div class="body">
          <canvas ref="canvas" id="canvas" width="300px" height="250px"></canvas>
        </div>
    </q-card>
    <q-dialog v-model="dialog">
      <q-card>
        <q-card-section>
          <div class="text-h6">Selamat Kepada Anda</div>
          <div class="text-h7">{{data}}</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Anda Berhak Mendapatkan Hadiah dikarenakan nilai anda melebihi 50
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-dialog v-model="dialogZonk">
      <q-card>
        <q-card-section>
          <div class="text-h1">Zonk</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Kamu Ingin Coba Lagi
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="Tidak" color="negative" v-close-popup />
          <q-btn push label="Ya" color="positive" v-close-popup @click="_tryAgain" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>

</template>

<script>
import axios from 'axios'
export default {
  name: 'Scratch',
  components: {

  },
  data() {
        return {
        canvas: null,
        context: null,
        width: 300,
        height: 250,
        isDrawing: null,
        startX:0,
        startY:0,
        stars: 4,
        result: 0,
        render: 0,
        dialog: false,
        dialogZonk: false,
        data: [],
        dataTryAgain:[],
        backGround1: 'url("data:image/jpeg;base64,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") no-repeat center',
        backGround2: 'url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRzy9fRHoJ1Tu9JLBJWV1WcxAGsH7r8zkc0zQ&usqp=CAU") no-repeat center'
        }
    },
  methods:{
    _tryAgain(){
      location.reload();
      return false;
    },
    initialize() { 
      if (this.result == 0) return
      console.log(this.result)
      let image = ""
        if (this.result > 50){
          image = this.backGround2;
        } else {
          image = this.backGround1;
        }

        var vm = this,context = vm.context,
        canvas = vm.canvas
        context.fillStyle = '#ddd';
        context.fillRect(0, 0, vm.width, vm.height); 
        canvas.style.background = image;
        canvas.style.backgroundSize = "100% 100%";
        context.lineWidth = 35;
        context.lineJoin = "round";
    },
    mouseDown(e){
        var vm = this
        vm.isDrawing = true;
        vm.startX = e.layerX;
        vm.startY = e.layerY;
    },
    mouseMove(e){
      var vm = this,
          context = vm.context
      if(vm.isDrawing){
        var x = e.layerX;
        var y = e.layerY;
        context.globalCompositeOperation = "destination-out"; 
        context.beginPath();
        context.moveTo(vm.startX, vm.startY);
        context.lineTo(x, y);
        context.closePath(); 
        context.stroke();
        vm.startX = x;
        vm.startY = y; 
      } 
    },
    mouseUp(e){
      this.dialog = false;
      this.dialogZonk = false;
      var vm = this
      vm.isDrawing = false;
      var data = vm.context.getImageData(0, 0, vm.width, vm.height).data;
      var length = data.length;
      var k = 0;
      for (var i = 0; i < length - 3; i += 4) {
        if (data[i] == 0 && data[i + 1] == 0 && data[i + 2] == 0 && data[i + 3] == 0) {
          k++;
        }
      }
      if(k>vm.width*vm.height*0.2){
        this.context.fillStyle = "blue";
        this.context.fillRect(0, 0, vm.width, vm.height);
        if (this.result > 50){
          this.dialog = true;
        } else if (this.result <= 50 && this.result != 0) {
          this.dialogZonk = true
        }
      }
    }
  },

  async mounted(){
    var vm = this
    let data = Math.floor(Math.random() * 20);
    const response = await axios.get('https://5fe876802e12ee0017ab4704.mockapi.io/api/scratch/scratch')
    this.dataTryAgain = response.data;
    for (let obj of response.data){
      if (obj.id == data){
        this.result = obj.value;
        this.data = obj;
      }
    }
    vm.canvas = vm.$refs.canvas
    vm.context = vm.canvas.getContext("2d");
    vm.initialize()
    vm.canvas.addEventListener('mousedown', vm.mouseDown, false);
    vm.canvas.addEventListener('mousemove', vm.mouseMove, false);
    vm.canvas.addEventListener('mouseup', vm.mouseUp, false);
  },
}

</script>
<style lang="sass" scoped>
  .q-pa-md
    background-image: url(../assets/scracth.jpg)
    background-attachment: cover
    width: 100%
    height: 550px
    margin-top: 0px
  .my-card
    width: 100%
    max-width: 300px
    cursor: crosshair
    

    
  
</style>
