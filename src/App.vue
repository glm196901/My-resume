<template>
  <div id="app">
    <style-edit ref="styleEditor" :code="currentStyle" :curCode="compCode"></style-edit>
    <RsumeEdit :markdown="currentMarkdown" :enableHtml="html"></RsumeEdit>
  </div>
</template>

<script>
  import  StyleEdit from './components/StyleEdit.vue'
  import  RsumeEdit from './components/RsumeEdit.vue'
  export default {
    name: 'app',
    data () {
      return {
        html: false,
        currentStyle: '',
        compCode: '',
        fullStyle: [
          `/*
* Inspired by http://strml.net/   模仿 https://github.com/jirengu-inc/animating-resume
* 大家好，我是王晓洋
* 7月了，好多公司都在招聘，你是不是也在准备简历呀。
* 说做就做，我也来写一份简历！
*/
/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 制作一个经典的terminal */
html {
  color: rgb(222,222,222); background: rgba(1,1,1,.8);
}
/* 文字离边框太近了 */
.styleEditor {
  padding: .5em;
  border: 3px solid;
  margin: .5em;
  overflow: auto;
  width: 45vw; height: 90vh;
}
/* 代码高亮 */
.hljs-comment{ color:#988989 ;}
.hljs-selector-class,.hljs-selector-tag{ color: rgb(133,153,0); }
.hljs-attribute{ color: rgb(187,137,0); }
.hljs-built_in{ color: yellow; }
.hljs-number{ color: rgb(42,161,152); }
/* 加点 3D 效果呗 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  -webkit-transition: none;
  transition: none;
  -webkit-transform: rotateY(10deg) translateZ(-100px) ;
          transform: rotateY(10deg) translateZ(-100px) ;
}
/* 接下来我借一个编辑器 */
.resumeEditor{
  position: fixed; right: 0; top: 0;
  padding: .5em;  margin: .5em;
  width: 48vw; height: 90vh;
  border: 1px solid;
  background: white; color: #222;
  overflow: auto;
}
/* 好了，我开始写简历了 */
`,
          `
/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
`
          ,
          `
/* 再对 HTML 加点样式 */
.resumeEditor{
padding: 2em;
}
.resumeEditor h2{
display: inline-block;
border-bottom: 1px solid;
margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
list-style: none;
}
.resumeEditor ul> li::before{
content: '•';
margin-right: .5em;
}
.resumeEditor ol {
counter-reset: section;
}
.resumeEditor ol li::before {
counter-increment: section;
content: counters(section, ".") " ";
margin-right: .5em;
}
.resumeEditor blockquote {
margin: 1em;
padding: .5em;
background: #ddd;
}
`
        ],
        currentMarkdown: '',
        fullMarkdown: `王晓洋
----
初级前端工程师，毕业于南京农业大学(211)怕别人以为野鸡学校。
技能
----
* 前端开发包括
* HTML5 + CSS3 + JavaScript
* 熟悉流行渐进式框架 Vue.js
* 熟练使用 vue-cli + webpack + stylus(CSS预处理)
* Node.js 开发
* 数据库 熟悉主流 NOSQL : mogodb(其实不擅长)，SQL : MYSQL
* 曾自学过python 对面相对象语言，脚本语言有较为深刻理解
* 熟悉ES6标准语法环境
* 熟练使用 linux/unix(mac) 环境

工作经历
----
1. 曾在一个清华创业团队担任项目助手，做一个叫做AIML的项目负责，语义化标签编写，
    该项目主要是团队中服务自然语言处理算法未识别的部分，用基于对话机器人爱丽丝的AIML来编写


链接
----
* [GitHub](https://github.com/glm196901)
> 如果你喜欢这个效果，Fork [我的项目](https://github.com/jirengu-inc/animating-resume)，打造你自己的简历！
`
      }
    },
    components: {
      StyleEdit,
      RsumeEdit
    },
    mounted () {
      this.intStyle();
    },
    methods: {
      intStyle: async function () {
        await  this.showStyle(0);
        await  this.showResume();
        await  this.showStyle(1);
        await  this.enableHtml();
        await  this.showStyle(2);
      },
      showStyle (num) {
        return new Promise((resolve, reject) => {
          let n = 0;
          let nowPre = this.fullStyle.filter((_, index) => {
            return index < num
          }).reduce((p, c) => {
            return p + c
          }, '');


          let thePre = this.fullStyle[num - 1] || '';
          let timer = setInterval(() => {
            n = n + 1;
            this.$refs.styleEditor.goBottom();

            this.currentStyle = nowPre + this.fullStyle[num].substring(0, n);
            if (this.currentStyle.substring(n - 1, n) === ';') {
              this.compCode = this.currentStyle;
            }
            if (n > this.fullStyle[num].length - 1) {
              resolve();
              clearInterval(timer)
            }
          }, 0)
        })
      },
      showResume () {
        return new Promise((resolve, reject) => {
          let n = 0;
          let timer = setInterval(() => {
            //
            n = n + 1;
            this.currentMarkdown = this.fullMarkdown.substring(0, n);
            if (n > this.fullMarkdown.length - 1) {
              resolve();
              clearInterval(timer)
            }
          }, 100)
        })
      },
      enableHtml () {
        this.html = true;
      }
    }
  }
</script>

<style>
  *{margin: 0; padding: 0;}
  *{box-sizing: border-box;}
  *::before{box-sizing: border-box;}
  *::after{box-sizing: border-box;}
</style>
