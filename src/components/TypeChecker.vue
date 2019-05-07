<template>
  <div class="type-checker">
    <div class="input">
      <textarea class="textarea" v-model="typeOrigin" placeholder="在這裡輸入"></textarea>
    </div>
    <div class="output">
      <div class="result" id="result"></div>
    </div>
  </div>
</template>

<script>
// String splice
String.prototype.splice = function(idx, rem, str) {
    return this.slice(0, idx) + str + this.slice(idx + Math.abs(rem));
};

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
      let newTypeOrigin = this.typeOrigin;

      // check space front
      newTypeOrigin = newTypeOrigin.replace(/[\u4e00-\u9fa5]+[\\/.,;?$A-Za-z0-9]+/g, (match) => {
        const headIndex = match.search(/[\\/.,;?$A-Za-z0-9]/);
        const front = match.slice(0, headIndex);
        const end = match.slice(headIndex);
        return front + ' ' + end;
      })
      // check space end
      newTypeOrigin = newTypeOrigin.replace(/[\\/.,;?$A-Za-z0-9]+[\u4e00-\u9fa5]+/g, (match) => {
        const headIndex = match.search(/[\u4e00-\u9fa5]/);
        const front = match.slice(0, headIndex);
        const end = match.slice(headIndex);
        return front + ' ' + end;
      })
      // replace breakline style
      newTypeOrigin = newTypeOrigin.replace(/\n+\n/g, () => {
        return '\n\u3000\n'
      })
      
      this.typeOrigin = newTypeOrigin;

      // --------
      let typeContent = this.typeOrigin;

      // highlight half-space
      typeContent = typeContent.replace(/[\u0020]/g, '<i class="symbol half">&nbsp;</i>');
      
      // highlight full-space
      typeContent = typeContent.replace(/[\u2003]/g, '<i class="symbol fill">&emsp;</i>');
      typeContent = typeContent.replace(/[\u3000]/g, '<i class="symbol fill">&emsp;</i>');
      
      // highlight break line
      typeContent = typeContent.replace(/\n/g, '<i class="symbol break"><i class="fas fa-level-down-alt"></i></i><br/>');

      DOM_result.innerHTML = typeContent;
    },
  },
}
</script>

<style>
  .type-checker {
    display: flex;
    margin: auto;
    width: 100%;
    height: 100%;
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
    font-size: 18px;
    box-sizing: border-box;
    overflow-y: auto;
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
    font-size: 18px;
  }
  .textarea:focus {
    outline: none;
  }
  .symbol {
    display: inline-block;
    vertical-align: middle;
    margin: 0 2px;
    border: 1px dashed hsl(0, 0%, 70%);
    line-height: 18px;
  }
  .half {
    width: 6px;
  }
  .fill {
    width: 18px;
  }
  .half:before,
  .fill:before {
    content: "|";
    visibility: hidden;
  }
  .break {
    width: 18px;
    text-align: center;
  }
  .break i {
    transform: rotate(90deg);
    font-size: 10px;
    color: hsl(0, 0%, 60%);
  }
</style>
