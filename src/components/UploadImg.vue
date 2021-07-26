<template>
  <div class="upload-img">
    <section>
      <img :src="img.image" alt="" srcset="">
      <div class="canvas-paper"></div>
      <input type="file" accept="image/png, image/jpeg" @change="uploadImg">
    </section>
    <section>
      <table>
        <thead>
          <tr>
            <th></th>
            <th>X</th>
            <th>Y</th>
            <th>Width</th>
            <th>Height</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(block, index) in blocks" :key="'block'+index">
            <th>{{ index + 1 }}</th>
            <td>{{ block.X }}</td>
            <td>{{ block.Y }}</td>
            <td>{{ block.width }}</td>
            <td>{{ block.height }}</td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script>
export default {
  name: 'UploadImg',
  props: {},
  watch: {
    img() {
      if(Boolean(this.img)) {
        document.querySelector('.canvas-paper').classList.add('active');
        document.querySelector('section img').classList.add('active');
      };
    }
  },
  data: () => ({
    img: {},
    newBlock: {},
    blocks: []
  }),
  methods: {
    async uploadImg(){
      var reader  = new FileReader();
      reader.addEventListener("load", ()=> {
        // preview.src = reader.result;
        this.img = {
          image: reader.result,
          imgBase64: reader.result.split(',')[1]
        };
      }, false);
      if (event.currentTarget.files[0]) {
        reader.readAsDataURL(event.currentTarget.files[0]);
      }
    },
    randomColor() {
      function random(min,max) {
        const num = Math.floor(Math.random()*(max-min)) + min;
        return num;
      };
      return 'rgb(' + random(0,255) + ', ' + random(0,255) + ', ' + random(0,255) +  ')';
    },
    mousedownEvent() {
      document.querySelector('.canvas-paper').addEventListener('mousedown', (e)=>{
        this.newBlock.X = e.offsetX;
        this.newBlock.Y = e.offsetY;
      });
    },
    mouseupEvent() {
      document.querySelector('.canvas-paper').addEventListener('click', (e)=>{
        if(this.blocks.length < 3) {
          this.newBlock.width = e.offsetX - this.newBlock.X;
          this.newBlock.height = e.offsetY - this.newBlock.Y;
          this.blocks.push(this.newBlock);

          const newDiv = document.createElement("div");
          document.querySelector('.canvas-paper').appendChild(newDiv);
          const newBlock = document.querySelector('.canvas-paper').lastChild;
          newBlock.classList.add('block');
          newBlock.style.setProperty('top', this.newBlock.Y + 'px');
          newBlock.style.setProperty('left', this.newBlock.X + 'px');
          newBlock.style.setProperty('width', this.newBlock.width + 'px');
          newBlock.style.setProperty('height', this.newBlock.height + 'px');
          newBlock.style.setProperty('background-color', this.randomColor());

          newBlock.addEventListener('click', () => {
            this.delBlock();
            event.stopPropagation();
          });

          this.newBlock = {};
        };
      });
    },
    delBlock() {
      const blocksDom = document.querySelector('.canvas-paper').childNodes;
      const currentBlock = event.currentTarget;
      blocksDom.forEach((blockDom, index) => {
        if(blockDom.outerHTML === currentBlock.outerHTML) {
          event.currentTarget.remove();
          this.blocks.splice(index, 1);
        };
      });
    },
  },
  mounted() {
    this.mousedownEvent();
    this.mouseupEvent();
  },
}
</script>

<style scoped lang="sass">
.upload-img::v-deep
  padding: 40px
  font-size: 0
  section
    position: relative
    display: inline-block
    width: 50%
    font-size: 18px
    vertical-align: top
    input
      cursor: pointer
    img
      display: none
      width: 400px
      height: 300px
      object-fit: cover
    .canvas-paper
      display: none
      position: absolute
      top: 0
      left: 50%
      width: 400px
      height: 300px
      transform: translateX(-50%)
      opacity: 0.6
      background-color: azure
    .active
      display: inline-block
  table
    width: 100%
  .block
    display: inline-block
    position: absolute
    outline: solid 1px black
    background-color: red
    opacity: 0.5
    cursor: pointer
    &:hover::after
      content: 'X'
      position: absolute
      top: 0
      left: 100%
      width: 20px
      height: 20px
      display: flex
      justify-content: center
      align-items: center
      font-size: 12px
      outline: solid 1px black
</style>
