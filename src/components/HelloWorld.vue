<template lang="pug">
  .hello
    input.hello-input(type="file" id="csv" @change="clickUpload")
    label.hello-label(type="button" for="csv")
      | csv入れるとこ（これはボタンです！！！！！！！！！！）
    button(@click="checkCsv")
      | csvアップロードしやつをここクリックしたらコンソールログにでる
    .output
      h2 csvプレビューするとこ
      .output-container(v-if="csvHtml" v-html="csvHtml")

</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      csvFile: null,
      csvHtml: null
    }
  },
  methods: {
    checkCsv () {
      console.log(this.csvFile)
    },
    clickUpload (e) {
      this.csvFile = e.target.files || e.dataTransfer.files
      this.setFiles(this.csvFile)
    },
    setFiles (files) {
      const self = this
      const reader = new FileReader()
      // ファイル読み取りに失敗したとき
      reader.onerror = () => {
        alert('ファイル読み取りに失敗しました')
      }
      // ファイル読み取りに成功したとき
      reader.onload = () => {
        const lineArr = reader.result.split('\n')
        // 行と列の二次元配列にする
        const itemArr = []
        for (let i = 0; i < lineArr.length; i++) {
          itemArr[i] = lineArr[i].split(',')
        }

        // tableで出力
        let insert = '<table>'
        for (let i = 0; i < itemArr.length; i++) {
          insert += '<tr>'
          for (let j = 0; j < itemArr[i].length; j++) {
            insert += '<td>'
            insert += itemArr[i][j]
            insert += '</td>'
          }
          insert += '</tr>'
        }
        insert += '</table>'
        self.csvHtml = insert
      }

      // ファイル読み取りを実行
      reader.readAsText(files[0])
    }
  }
}
</script>

<style lang="scss">

.hello-input {
  display: none;
}

.hello-label {
  border: 1px solid #000;
  cursor: pointer;
}

h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
