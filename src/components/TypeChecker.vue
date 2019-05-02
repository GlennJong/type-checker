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
      <div><i class="fas fa-clone"></i>：全形空白&emsp;</div>
      <div><i class="fas fa-hand-point-down"></i>：換行符號</div>
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
      const regex = /[A-Za-z]+||[0-9]+\.[0-9]+/g;
      let typeContent = this.typeOrigin;

      // get English words
      const englishWords = typeContent.match(/[.,;?$A-Za-z0-9]+/g) || [];
      englishWords.forEach((word) => {
        const wordHead = typeContent.indexOf(word);
        const wordTail = wordHead + word.length -1;
        // console.log(typeContent.charAt(wordHead) + ', ' + typeContent.charAt(wordTail))
        if (typeContent.charAt(wordHead - 1) !== ' ' ||
            typeContent.charAt(wordTail + 1) !== ' ') {
          console.log(typeContent.slice(0, wordHead) + '!' + typeContent.slice(wordHead));
          // console.log('fail')
        }
        else {
          console.log('success')
        }
      })
      // numberWords.forEach((word) => {
      //   console.log(word)
      //   const wordHead = typeContent.indexOf(word);
      //   console.log(typeContent.indexOf(word));
      //   console.log(typeContent.indexOf(word) + word.length);
      // })

      // highlight half-space
      typeContent = typeContent.replace(/[\u0020]/g, '<i class="symbol half"></i>');
      
      // highlight hald-space
      typeContent = typeContent.replace(/[\u2003]/g, '<i class="symbol fill"></i>');
      typeContent = typeContent.replace(/[\u3000]/g, '<i class="symbol fill"></i>');
      
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
    font-size: 24px;
    box-sizing: border-box;
    overflow-y: auto;
  }
  .result {
    /* overflow-y: auto; */
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
  .symbol {
    display: inline-block;
    vertical-align: middle;
    margin: 0 2px;
    border: 1px dashed hsl(0, 0%, 70%);
    line-height: 24px;
  }
  .half {
    width: 6px;
  }
  .fill {
    width: 24px;
  }
  .half:before,
  .fill:before {
    content: "|";
    visibility: hidden;
  }
  .break {
    width: 24px;
    text-align: center;
  }
  .break i {
    transform: rotate(90deg);
    font-size: 10px;
    color: hsl(0, 0%, 60%);
  }
</style>
