<template>
  <header>
    <div class="header">
      <h1>Yu Jie</h1>
      <p ref='textSpin'>存放著我的小作品</p>
      <p ref='textSpin'>每張卡的背面皆有說明</p>
    </div>
  </header>
  <main>
    <section class="project">
      <div class="projectCard">
        <div class="title">
          <h2>Vue-計算機</h2>
        </div>
        <div class="card cover cover1">
          <img src="@/assets/img/projects/calculator.png" alt="">
        </div>
        <div class="card back back1">
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import { onMounted, ref } from 'vue'
export default{
  setup(){
    const textSpin = ref(null)
    const fetchData = () => {
      fetch('data/projectList.json')
      .then(d => d.json())
      .then(res => {
        console.log(res);
      });
    }
    onMounted(() => {
      textSpin.value.innerHTML = textSpin.value.textContent.replace(/\S/g, '<span>$&</span>')
      document.querySelectorAll('span').forEach((span, index) => {
        span.style.setProperty('--delay', `${index *0.1}s`)
      })
      fetchData()
    })
    return{
      textSpin,
    }
  }
}


</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Freehand&family=Fuzzy+Bubbles&family=Pacifico&family=Varela+Round&display=swap');
body {
  color: #2c3e50;
  background-image: url("~@/assets/img/back2.jpeg");
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
      font-size: 1.1em;
      span{
        display: inline-block;
        animation: blink .4s ease-in-out ;
        animation-delay: var(--delay);
      }
    }
  }
  main{
    .project{
      .projectCard{
        width: 300px;
        position: relative;
        .title{
          background: linear-gradient(45deg, #ce921a 0%, #DAAF08 45%, #FEE9A0 70%, #DAAF08 85%, #daa233 90% 100%);
          width:fit-content;
          border-radius: 1em;
          overflow: hidden;
          margin: 0 auto 1em;
          box-shadow: 10px 10px 10px rgb(131, 131, 131);
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
        .card{
        box-shadow: 10px 10px 10px rgb(97, 97, 97);
        border: #DAAF08 5px solid;
        border-style: ridge;
        }
        .cover, .back{
          width: 300px;
          height: 350px;
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
        }
        .back1{
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
    color: yellow;
  }
}
</style>
