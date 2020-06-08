<template>
  <div class="container container-fluid">
    <div class="row">
      <div class="col-md-6">
        <h1>Drevitsky Vsevolod</h1>
        <div v-for="(value, name) in contacts" class="row contact__row">
          <div class="col-md-2 contact contact__name">
            {{name}} :
          </div>
          <div class="col-md-10 contact contact__value">
            {{ value }}
          </div>
        </div>    
      </div>
      <div class="col-md-6">
        <img src="/img/my-img.jpeg" alt="my-img">
      </div> 
    </div>
    <div class="row">
      <div class="col">
        <transition-group name="skill" tag="div">
        <div v-show="isTrue" class="skill"  v-for="skill in skills" :key="skill">
          {{skill}}
        </div>
        </transition-group>
      </div>
    </div> 
    <div class="row">
      <div class="col">
        <button @click="stopShuffle" class="btn btn-secondary">Stop Shuffle</button>
      </div>
      
    </div>   

    
  </div>
  
</template>

<script>


export default {
  data() {
    return {
      skills: [
        'HTML',
        'CSS',
        'JavaScript',
        'ES 6',
        'Vue.js',
        'React.js',
        'Nuxt.js',
        'Vuex',
        'Redax',
        'JQuery',
        'Bootstrap',
        'Wordpress',
        'Flexbox',
        'Sass/scss',
        'Pug',
        'Gulp',
        'Webpack',
        'Git',
        'PHP',
        'MySQL',
        'Laravel',
        'Phothoshop',
        'Corel Draw',
        'Figma',
        'Adobe XD'
      ],
      contacts: {
        phone: '+ 38 050 422 77 23',
        email: 'iq180@ukr.net',
        linkedin: 'linkedin.com/in/vsevolod-drevitsky/',
        skype: 'ymnik180',
        github: 'github.com/drevitsky'
      },
      isTrue:false,
      timerId:null

    }
  },
  mounted() {
    this.startShuffle()
  },
  methods: {
    shuffle(array) {
      let currentIndex = array.length, temporaryValue, randomIndex;
      
      while (0 !== currentIndex) {        
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;        
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }
      return array;
    },
    stopShuffle() {
      clearInterval(this.timerId)    
    },
    startShuffle() {
      this.timerId = setInterval(() => {
        this.skills = this.shuffle(this.skills)
        setTimeout(() => {
          this.isTrue = !this.isTrue
        }, 0);
        this.isTrue = !this.isTrue
      }, 5000);      
      this.isTrue = !this.isTrue
    }
    
  }
}
</script>

<style scoped lang="scss">
.container-fluid {
  // outline: 1px solid red;
}
.container {
  margin: 0 auto;
  min-height: 80vh;
  /* display: flex; */
  /* justify-content: space-between; */
  /* align-items: center; */
  text-align: center;
}
/* ============= */
.col-md-6 {
  // outline: 1px solid red;
}
.skill {
  display: inline-block;
  padding: 15px;
  margin: 15px;
  font-size: 20px;
  border: 1px solid #5555;
  border-radius: 5px;
}
.skill-move {
  transition: transform 1s;
}
.btn-secondary {
  padding: 15px;
  margin: 15px;
  font-size: 20px;
}

.contact {
  // outline: 1px solid red;
  padding: 10px ;
  font-size: 16px;
  &__name {
    text-align: left;
  }
  &__value {
    text-align: left;
  }
}

</style>
