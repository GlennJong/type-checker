<template>
  <div class="type-checker">
    <div class="input">
      <textarea class="textarea" v-model="typeOrigin" placeholder="在這裡輸入"></textarea>
    </div>
    <div class="output">
      <div class="result" id="result"></div>
    </div>
    <div class="hint">
      <div><i class="fas fa-square"></i>：半形空白</div>
      <div><i class="fas fa-clone"></i>：全形空白</div>
      <div><i class="fas fa-hand-point-down"></i>：換行符號</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TypeChecker',
  props: {
  },
  updated() {
    this.transferContent();
  },
  data() {
    return {
      typeOrigin: "",
    }
  },
  methods: {
    transferContent: function () {
      const DOM_result = document.getElementById('result');
      let typeContent = this.typeOrigin;

      // highlight hald-space
      typeContent = typeContent.replace(/[\u0020]/g, '<i class="fas fa-square"></i>');
      
      // highlight hald-space
      typeContent = typeContent.replace(/[\u2003]/g, '<i class="fas fa-clone"></i>');
      
      // highlight break line
      typeContent = typeContent.replace(/\n/g, '<i class="far fa-hand-point-down"></i><br/>');

      DOM_result.innerHTML = typeContent;
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .type-checker {
    display: flex;
    margin: auto;
    width: 100%;
    height: 100%;
    /* max-width: 80%; */
    /* max-height: 80%; */
    box-shadow: 2px 4px 12px hsla(0, 0%, 0%, .5);
  }
  .input {
    border-right: 1px solid hsl(0, 0%, 85%);
    padding: 24px;
    width: 50%;
    box-sizing: border-box;
  }
  .output {
    padding: 24px;
    width: 50%;
    font-size: 24px;
    box-sizing: border-box;
  }
  .hint {
    position: absolute;
    right: 0;
    bottom: 0;
  }
  .textarea {
    resize: none;
    border: 0;
    padding: 0;
    box-shadow: 0;
    width: 100%;
    height: 100%;
    font-size: 24px;
  }
  .textarea:focus {
    outline: none;
  }
  .far, .fas {
    margin: 0 2px;
  }
  .fa-square {
    font-size: 12px;
  }
  .fa-clone {
    font-size: 12px;
  }
  .fa-hand-point-down {
    font-size: 14px;
  }
</style>
