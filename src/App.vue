<template>
  <div id="app">
    <div class="container">
      <ul>
        <li v-for="item in list.getItems()" :key="item.id">{{item.name}} {{item.description}}</li>
      </ul>
      <form action="POST" class="form" @submit.prevent="submit" @change="error = false">
        <div class="field">
          <label>Name:</label>
          <input type="text" name="name" id="" class="input" v-model="list.name">
          <span class="error" v-if="error">This field is required</span>
          <label>Description:</label>
          <input type="text" name="description" class="input" v-model="list.description">
          <span class="error" v-if="error">This field is required</span>
          <button class="button">Create</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>

 class List {
   constructor(data) {
     for(let field in data) {
       this[field] = data[field]; // love this solution
     }
     this.listArray = [], //axios get request
     this.errorMessage = 'This field is required',
     this.succesMessage = 'Succesfully added'
     this.counter = 0;
   }
  getItems() {
    if (this.listArray.length > 0) {
      return this.listArray
    } else {
      return [{
         name : "Please add an item below: 🔽",
         description: ""
      }];
   }
  }
  add() {
    if(this.name && this.description) {
        const newItem = {
        name: this.name,
        description: this.description,
        id: this.counter
      };
      this.listArray.push(newItem); //axios post request
      this.counter++;
      return this.succesMessage;
    } else {
      return this.errorMessage;
    }
    
  }
  clear() {
      this.name = '';
      this.description = ''
    }
 }

export default {
  name: 'App',
  data() {
    return {
      list: new List({
        name: '',
        description: ''
      }),
      error: false
    }
  },
  methods: {
    submit() {
      //check if form is filled
      const fillForm = this.list.add();
      if(fillForm === 'This field is required') {
        this.error = true;
      } else {
        alert(`You've added an item`)
        this.list.clear();
      }
      
    }
    
  }

}
</script>

<style>
ul {
  margin-top: 2rem;
}
form{
  margin-top: 1rem;
}
input {
    margin-bottom: 1rem;
  }
.error {
  color: red;
  display: block;
  margin-bottom: 1rem;
}
</style>
