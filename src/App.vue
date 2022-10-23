<template>
  <header>
    <div class="header">
      <h1>Yu Jie</h1>
      <!-- 這裡改用 v-bind 綁定 ref -->
      <p :ref='setTextSpin'>存放著我的小作品</p>
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
        // 這裡可以取得兩個 p 的 DOM
        console.log(el);
      })

      // textSpin.value.innerHTML = textSpin.value.textContent.replace(/\S/g, '<span>$&</span>')
      // document.querySelectorAll('span').forEach((span, index) => {
      //   span.style.setProperty('--delay', `${index *0.1}s`)
      // })
      fetchData()
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
body {
  background-image: url("../public/img/back2.jpeg");
  background-size: cover;
}
#app{
  padding: 0 5em;
  .header{
    text-align: center;
    padding: 40px 0;
    h1{
      font-size: 3.5em;
      font-family: 'Fuzzy Bubbles', cursive;
      font-weight: 600;
      color: transparent;
      text-shadow: rgba(0, 0, 0, 0.5) 0px 3px 3px;
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
        animation: blink 5s ease-in-out infinite 1s;
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
        margin-right: 1.2em;
        margin-bottom: 2em;
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
          width: 300px;
          position: relative;
          height: 350px;
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
          transform: rotateY(0deg);
          img{
            width: 100%;
            height: 100%;
            object-fit: cover;
          }
        }
        .back{
          transform: rotateY(-180deg);
          background: linear-gradient(152deg, rgb(1, 51, 141), rgb(0, 13, 43));
        }
        &:hover .cover{
          transform: rotateY(180deg);
        }
        &:hover .back{
          transform: rotateY(0deg);
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
    color: rgb(109, 109, 109);
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
