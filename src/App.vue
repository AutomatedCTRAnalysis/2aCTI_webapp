<template>
  <div id="app" class="container">
  
  <center><div id = "logo">
    <img src="./assets/2aCTI.png">
  </div></center>


  <h1 class="title mt-0">Report Classification</h1>
  
  <h2 class="mx-6">Enter the name of a report to predict the tactics and techniques (TTPs)</h2>
  <br><br>

  <div class="searchBar">
    <b-input placeholder="Search..." rounded v-model="searchBar"></b-input>
  </div>
  
  <br>
  <h2>or enter directly a report to retrieve its corresponding TTPs</h2>
  <br>

  <div class="textbox">
    <b-field>
      <b-input type="textarea"
        minlength="10"
        placeholder="Submit Report"
        v-model="searchTextArea">
      </b-input> 
    </b-field>
    <br>
  </div>

  <b-button type="is-link is-light" rounded @click="submitTextForm">Predict !</b-button><br />
  <br>


<div class="columns" v-if="display_boxes">

  <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          {{d_tactic[0].label}}
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content" :class="{ 'has-background-success': fake_tactics.includes(d_tactic[0].id), 'has-background-danger': !fake_tactics.includes(d_tactic[0].id) }">
        <div class="content">
          Technique 1 
        </div>
      </div>
    </div>
  </div>
  <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          2) Resource Development
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 2 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          3) Initial Access
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 3 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          4) Execution
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 4 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          5) Persistence
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content has-background-danger">
        <div class="content">
          Technique 5 
        </div>
      </div>
    </div>
  </div>
</div>

<div class="columns" v-if="display_boxes">
  <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          6) Privilege Escalation
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 1 
        </div>
      </div>
    </div>
  </div>
  <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          7) Defense Evasion
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 2 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          8) Credential Access
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 3 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          9) Discovery
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content has-background-success">
        <div class="content">
          Technique 4 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          10) Lateral Movement
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content has-background-danger">
        <div class="content">
          Technique 5 
        </div>
      </div>
    </div>
  </div>
</div>
 

<div class="columns" v-if="display_boxes">
  <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          11) Collection
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 1 
        </div>
      </div>
    </div>
  </div>
  <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          12) Command and Control
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 2 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          13) Exfiltration
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content">
        <div class="content">
          Technique 3 
        </div>
      </div>
    </div>
  </div>
   <div class="column">
        <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          14) Impact
        </p>
        <button class="card-header-icon" aria-label="more options">
          <span class="icon">
            <i class="fas fa-angle-down" aria-hidden="true"></i>
          </span>
        </button>
      </header>
      <div class="card-content has-background-success">
        <div class="content">
          Technique 4 
        </div>
      </div>
    </div>
  </div>
</div>

  <!-- <footer class="mx-1 footer is-centered">
      <p class="content has-text-centered">
        <strong>2aCTI</strong> is a group project. The source code can be found on <a href="https://github.com/AutomatedCTRAnalysis/Automated_CTR_Analysis">Github</a>.
      </p>
  </footer> -->


</div>

  
</div>
</template>

<script>
import axios from 'axios'; // chercher dans node module axios

export default {
  name: 'App',
  data() {
    return {
      d_tactic: [
        {
          id: 'TA0006',
          label: '1) Reconnaissance'
        }
      ],

      searchBar: '',
      searchTextArea: '', 
      //d_text = {'sentence': searchTextArea}, // created dictionary for input text
      api_url: "http://localhost:8000/classification", // change api address 
      tactics: [],
      fake_tactics: ['TA0006', 'TA0002', 'TA0040', 'TA0004', 'TA0008', 'TA0005',
       'TA0010', 'TA0007', 'TA0009', 'TA0011'],
      display_boxes: false,
    }
  },
  methods: {
    submitSearchForm() {
      alert(this.searchBar)
    },
    async submitTextForm(){ // asynchronous request
      //this.d_text = response.data
      //this.tactics = []
      console.log(this.fake_tactics.includes('TA0006'))
      let response = await axios.post(this.api_url, {'sentence': this.searchTextArea}) // call API
      .then(response => this.tactics = response.data) // retrieve response
      
      //this.searchTextArea = response.data
      //this.tactics = response.data
      this.display_boxes = true
      //response.data.forEach(element =>{
      //  this.tactics.push(element.tactics)
      //})
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color:midnightblue;
}
.searchBar{
  width: 600px;
  margin: 0 auto;
}
.textbox{
  width: 600px;
  margin: 0 auto;
}
h1.title{
  font-size: 35px;
  color: rgb(27, 27, 125);
  margin-top: 0%; 
}
h2{
  color: #666e77;
  font-size: 20px;
}
#nav {
  padding: 30px;
}
input{
    display: inline; 
    padding: 10px 6px; 
    margin: 30px;
    width: 60%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd; 
    color: rgb(141, 138, 138); 
  }
button{
  margin: 0 10px; 
  padding: 10px; 
  border: none; 
  border-radius: 4px; 
  color: midnightblue;
}
#logo {
  height: 200px;
  width: 310px; 
  text-align: center;
  display: flex;
}
#logo > img{
  text-align: center;
  align-content: center;
  max-width: 100%;
  max-height: 100%;
}
footer{
  display: inline;
  text-align: center;
  height: 50px;
}
</style>
