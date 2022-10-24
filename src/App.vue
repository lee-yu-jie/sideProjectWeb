<template>
  <div class="mouse"></div>
  <header>
    <div class="header">
      <h1>Yu Jie</h1>
      <p :ref="setTextSpin" >Side Project</p>
      <p :ref='setTextSpin'>每張卡的背面皆有說明</p>
    </div>
  </header>
  <main>
    <section class="project">
      <div v-for="item in projectList.data" :key="item.name" class="projectCard">
        <div class="title">
          <h2>{{item.name}}</h2>
        </div>
        <div class="content">
          <div class="card cover">
            <img :src="item.cover"  :alt="item.name">
          </div>
          <div class="card back">
            <p>使用技術：{{item.skill}}</p>
            <p>簡介：{{item.introduce}}</p>
            <div v-if="item.responsibility">負責範疇：
              <p v-for="item, idx in item.responsibility" :key="idx">
                {{idx + 1}}. {{item}}
              </p>
            </div>
            <p>展示：<a :href="item.web">Click Me</a></p>
            <p>程式碼：<a :href="item.code">Click Me</a></p>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import { onMounted, ref, reactive } from 'vue'
export default{
  setup(){

    // const textSpin = ref(null);
    
    const textSpins = ref([]);
    const setTextSpin = el => {
      textSpins.value.push(el);
    };

    const projectList = reactive({data: {}})
    const fetchData = () => {
      fetch('data/projectList.json')
      .then(d => d.json())
      .then(res => {
        
        projectList.data = res
        console.log(projectList);
      });
    }
    onMounted(() => {

      textSpins.value.forEach(el => {
        el.innerHTML = el.textContent.replace(/\S/g, '<span>$&</span>')
        document.querySelectorAll('span').forEach((span, index) => {
          span.style.setProperty('--delay', `${index *0.1}s`)
        })
      })

      // textSpin.value.innerHTML = textSpin.value.textContent.replace(/\S/g, '<span>$&</span>')
      // document.querySelectorAll('span').forEach((span, index) => {
      //   span.style.setProperty('--delay', `${index *0.1}s`)
      // })
      fetchData()
      const mouse = document.querySelector('.mouse');
      document.addEventListener("mousemove", function(e){
        var oLeft = e.clientX;  
        var oTop = e.clientY;          
        mouse.style.left = oLeft + "px"; 
        mouse.style.top = oTop + "px"; 
      });
    })
    return{
      // textSpin,
      projectList,
      setTextSpin
    }
  }
}


</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Freehand&family=Fuzzy+Bubbles&family=Pacifico&family=Varela+Round&display=swap');
@mixin pad{
  @media(min-width:500px){
    @content;
  }
}
body {
  background-image: url("../public/img/back2.jpeg");
  background-size: cover;
  // cursor: none;
  cursor: url("~@/cat.ico"),auto;
}
#app{
  padding: 0 2em;
  max-width: 1200px;
  margin: auto;
  .header{
    text-align: center;
    padding: 40px 0;
    h1{
      font-size: 4em;
      font-family: 'Fuzzy Bubbles', cursive;
      font-weight: 900;
      color: transparent;
      text-shadow: rgba(0, 0, 0, 0.8) 0px 3px 3px;
    }
    h1 + p{
      font-size: 1.5em;
      font-family: 'Pacifico', cursive;
    }
    p{
      line-height: 1.5em;
      font-size: 1.3em;
      color: transparent;
      font-weight: 800;
      color:transparent;
      -webkit-text-stroke: 1px rgb(145, 145, 145);
      span{
        display: inline-block;
        animation: blink 3s ease-in-out infinite 1s;
        animation-delay: var(--delay);
      }
    }
  }
  main{
    .project{
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
      .projectCard{
        margin: 0 1.2em 2em;
        .title{
          width:fit-content;
          border-radius: 1em;
          overflow: hidden;
          margin: 0 auto 1em;
          box-shadow: 10px 10px 10px rgb(131, 131, 131);
          animation: shine .8s ease-in infinite;
          h2{
            color: #294d70;
            font-size: 1.5em;
            font-weight: 800;
            text-align: center;
            padding: .6em;
            border: 1px solid wheat;
            background-color: #a25702;
            background-clip: text;
            color: transparent;
            text-shadow: rgba(255,255,255,0.5) 0px 3px 3px;
          }
        }
        .content{
          position: relative;
          width: 350px;
          height: 200px;
          &:hover .cover{
          transform: rotateY(180deg);
          }
          &:hover .back{
            transform: rotateY(0deg);
          }
          @include pad{
            width: 500px;
            height: 300px;
          }
        }
        .card{
          box-shadow: 10px 10px 10px rgb(97, 97, 97);
          border: #DAAF08 5px solid;
          border-style: ridge;
          top:0;
          bottom: 0;
          left: 0;
          right: 0;
          position: absolute;
          backface-visibility: hidden;
          transition: 1s ease;
          border-radius: 20px;
          overflow: hidden;
        }
        .cover {
          transform: perspective(1000px) rotateY(0deg);
          background:linear-gradient(45deg, #daa223 0%, #DAAF08 45%, #FEE9A0 70%, #DAAF08 85%, #daa233  100%);;
          img{
            width: 100%;
            height: 100%;
            object-fit: contain;
          }
        }
        .back{
          transform: rotateY(-180deg);
          padding: 0 2em;
          background: linear-gradient(152deg, rgb(1, 51, 141), rgb(0, 13, 43));
          color: white;
          display: flex;
          flex-direction: column;
          justify-content: center;
          line-height: 1.2em;
          a{
            color: #6200ff;
            padding: 3px 5px;
            background: #DAAF08;
            border-radius: 10px;
          }
          & > p, div{
            margin-bottom: 1em;
          }
        }
      }
    }
  }
}

@keyframes blink {
  0%, 100% {
    color: inherit;
  }
  50% {
    color: rgb(66, 66, 66);
  }
}
@keyframes shine {
  0% ,100%{
    background: linear-gradient(45deg, #daa223 0%, #DAAF08 45%, #FEE9A0 70%, #DAAF08 85%, #daa233  100%);
  }
  20%{
    background: linear-gradient(45deg, #daa223 0%, #daa223 45%, #DAAF08 70%, #FEE9A0 85%, #DAAF08 100%);
  }
  40%{
    background: linear-gradient(45deg, #DAAF08 0%, #daa223 45%, #daa223 70%, #DAAF08 85%, #FEE9A0 100%);
  }
  60%{
    background: linear-gradient(45deg, #FEE9A0 0%, #DAAF08 45%, #daa223  70%, #daa223 85%, #DAAF08 100%);
  }
  80%{
    background: linear-gradient(45deg, #DAAF08 0%, #FEE9A0 45%, #DAAF08 70%, #daa223  85%, #daa223 100%);
  }
}
</style>
