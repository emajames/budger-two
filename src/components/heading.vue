<template>
<div class="container-fluid px-0 mt-0">
    <div class="nav-container">
      <nav class="d-flex align-items-center">
        <div class="image-container rounded-circle">
          <img src="../assets/cashguard.png" class="nav-image" alt="">
        </div>
        <!-- <h1 class="text-white ml-1">Cash<span class="guid">-Guard</span></h1> -->
      </nav>

      <div class="text-center text-light month">
        <h1 class="mt-md-3 mb-md-3 my-sm-3">Monthly Budget Application</h1>
        <h2><span>{{month}}</span> <span>{{year}}</span></h2>
        <h1 class="total-value my-md-4 my-sm-3"> {{balance}} </h1>
      </div>

      <div class="row text-white">
        <div class="col-md-6 col-sm-12 px-md-5">
          <div class="income d-flex justify-content-between income-container rounded mx-5 mt-3 py-md-2">
            <h6>Income</h6>
            <h6>{{totalIncome}}</h6>
          </div>
        </div>

        <div class="col-md-6 col-sm-12 px-md-5">
          <div class="expenses d-flex justify-content-between income-container rounded mx-5 mt-3 mb-3 py-md-2">
            <h6>Expenses</h6>
            <h6> {{totalExpenses}} </h6>
          </div>
        </div>
      </div>
    </div>

    <div class="jumbotron mb-0 sect-1">
      <div class="in-ex-selector text-center d-flex flex-wrap justify-content-center">
        <div class="input-sub">
          <select name="" id="cont" v-model="budget.category">
            <option value="income">Income</option>
            <option value="expenses">Expenses</option>
          </select>
          
        </div>

        <div class="input-sub">
          <input type="text" name="" id="item" placeholder="Description" v-model="budget.desc">
          
        </div>
        <div class="input-sub">
          <input type="number" name="" id="item" v-model="budget.amount">
          
        </div>

        <div class="input-sub">
          <button class="btn-btn add" @click="addBudget()">Add</button>
        </div>
        
      </div>
    

      <div class="row mt-5">
        
        <div class="col-md-6 col-sm-12 px-md-5 px-sm-1">
          <div class="bg-light rounded p-3">
            <h5 class="text-center">Income</h5>

            <div class="d-flex justify-content-between" v-for="(inc, index) in income" :key="index">
              <div class="item-desc">{{inc.desc}} </div>
              <div class="item-value"> {{inc.amount}} <button class="delete-btn" v-on:click="deleteInc(index)">
                <i class="fa fa-trash" aria-hidden="true"></i></button></div>
            </div>
          </div>
        </div>

        <div class="col-md-6 col-sm-12 px-md-5 px-sm-1">
          <div class="bg-light rounded p-3 expend">
            <h5 class="text-center">Expenses</h5>
            <div class="d-flex justify-content-between" v-for="(exp, index) in expense" :key="index">
              <div class="item-desc"> {{exp.desc}} </div>
              <div class="item-value"> {{exp.amount}} <button class="delete-btn" v-on:click="deleteExp(index)">
                <i class="fa fa-trash" aria-hidden="true"></i></button></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
    
</template>

<script>

export default {
    data() {
      return{
        name: '',
        month: '',
        year:'',
        allBudgets: [],
        income: '',
        expense:'',
        totalIncome: '',
        totalExpenses: '',
        balance: '',
        unformattedIncome: '',
        unformattedExpenses: '', 
        budget: {
          category: '',
          desc: '',
          amount: ''
        }
      }   
    },

    methods:{
      getMonth(){
        let d = new Date()
        let month = [];
          month[0] = "January";
          month[1] = "February";
          month[2] = "March";
          month[3] = "April";
          month[4] = "May";
          month[5] = "June";
          month[6] = "July";
          month[7] = "August";
          month[8] = "September";
          month[9] = "October";
          month[10] = "November";
          month[11] = "December";
        this.month  = month[d.getMonth()];
        this.year  = d.getFullYear()
      },
      addBudget(){
        this.allBudgets.push(this.budget)
        console.log(this.allBudgets);
        localStorage.setItem('emmaaa', JSON.stringify(this.allBudgets))
        this.allBudgets = JSON.parse(localStorage.getItem("emmaaa"));
        this.budget.category = ''   
        this.getIncome()
        this.getExpenses()
        this.getBalance();
        this.budget.category = "";
        this.budget.desc = "";
        this.budget.amount = "";
      },
      getIncome(){
          // Filtering All Budget List
          this.allBudgets = JSON.parse(localStorage.getItem("emmaaa"));
          this.income = this.allBudgets.filter((budget) => budget.category == 'income')
        
        
        this.totalIncome = this.income.map(item => +item.amount).reduce((prev, next) => prev + next);
        this.unformattedIncome = this.totalIncome
        this.totalIncome = this.formatCurrency(this.totalIncome)
        this.getBudgets()
        this.getBalance()

      },
      getExpenses(){
          // Filtering All Budget List
          this.allBudgets = JSON.parse(localStorage.getItem("emmaaa"));
        this.expense = this.allBudgets.filter((budget) => budget.category == 'expenses');
        this.totalExpenses = this.expense.map(item => +item.amount).reduce((prev, next) => prev + next);
        this.unformattedExpenses = this.totalExpenses
        this.totalExpenses = this.formatCurrency(this.totalExpenses)
        this.getBudgets()
        this.getBalance()

      },

      formatCurrency(amount){
        let formatter = new Intl.NumberFormat('en-US', {
            style: 'currency',
            currency: 'NGN',
          });
          amount = formatter.format(amount)
          return amount

      },

      getBalance(){
        this.balance = +this.unformattedIncome - +this.unformattedExpenses
        this.balance = this.formatCurrency(this.balance)
      },

      getBudgets(){
      this.allBudgets = JSON.parse(localStorage.getItem("emmaaa"));
      },

      deleteInc(index){
        let deletedItem = this.income.splice(index, 1)
        console.log(deletedItem)
        this.allBudgets.splice(index, 1)
        // this.allBudgets = JSON.parse(localStorage.getItem("emmaaa")).splice(index, 1);
        localStorage.setItem('emmaaa', JSON.stringify(this.allBudgets))
      },

      deleteExp(index){
        let deletedItem = this.expense.splice(index, 1)
        console.log(deletedItem)
        this.allBudgets.splice(index, 1)
        localStorage.setItem('emmaaa', JSON.stringify(this.allBudgets))
      }
    },
    mounted(){
       
    },
    created(){
      localStorage.setItem('emmaaa', this.balance)
      this.getBudgets()
      this.getMonth()
      this.getIncome()
      this.getExpenses()
    }
}
</script>


<style scoped>
* {
  margin: 0;
  padding: 0;
}
.nav-container {
  background-image: url("../assets/image one.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-color: rgba(12, 11, 22, 0.699);
  background-blend-mode: overlay;
  padding: 3vmin 3vmin;
  height: 80vh;
}
.nav-image {
  padding: 0.3vmin;
  width: 25vmin;
}

#cont{
  outline: none;
}
.guid{
  color:rgb(29, 149, 29) ;
}
nav h1 {
  font-size: 5vmin;
}
.month h1, h2 {
  font-size: 8vmin;
}
.total-value h1 {
  font-size: 15vmin;
  /* margin-bottom: 100rem; */
}
.sect-1 {
  padding: 10vmin 11vmin;
}
.income{
  background: rgb(29, 149, 29) !important;
}
.expenses{
  background: rgb(231, 23, 23);
}

.income,
.expenses {
  font-size: 4vmin;
}

.income-container {
  padding: 1vmin 1vmin;
}

select {
  height: 2.3rem;
  /* width: 20vmin; */
  border-radius: 0.3rem;
}
.in-ex-selector input {
  margin-left: 5vmin;
  outline: none;
  height: 2.3rem;
  /* width: 60vmin; */
  border-radius: 0.3rem;
}

.in-ex-selector button {
  margin-left: 5vmin;
  outline: none;
  height: 2.3rem;
  /* width: 15vmin; */
  border-radius: 0.3rem;
}
.add{
    background: rgb(29, 149, 29);
    border: none;
    padding: 0 2.2vmin;
    color: #fff;
}
#item{
  border:3px solid rgb(245, 231, 231); 
  
  color: #000 !important;
}

.delete-btn{
  border: none;
  background: none;
  outline: none;
}

@media only screen and (max-width: 800px) {
  .in-ex-selector {
    display: block !important;
    text-align: center;
  }

  select {
    width: 100%;
    margin-bottom: 3vmin;
    height: unset;
  }
  .in-ex-selector input {
    margin-left: unset;
    width: 100%;
    margin-bottom: 3vmin;
    height: unset;
  }

  .in-ex-selector button {
    margin-left: unset;
    width: 50%;
    height: unset;
  }

  .expend {
    margin-top: 4vmin;
  }
  .nav-container {
  height: 95vmin;
}
.add{
    margin-top: 5vmin;
    padding: 1vmin 0;
}
}

@media only screen and (max-width: 600px) {
 .month h1 {
     margin: 5vmin;
}
}

</style>