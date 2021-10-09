<template>
  <div class="container-fluid p-0">
        <div class="all">
            <div class="nav-container">
                <nav>
                  <div class="image-container">
                    <img src="../assets/cashguard img.png" class="nav-image" alt="" />
                  </div>
                </nav>
         
                  <div class="writeup text-center">
                            
                    <div class="sect-1-writeup text-center mx-auto">
                      <h1>Best Budgeting App & Tools</h1>
                    </div>
                    
                    <div class="text-center text-light month">
                      <h1>
                        Budget for <span>{{ month }}</span> <span>{{ year }}</span>
                      </h1>
                      <h1 class="total-value">{{ balance }}</h1>
                    </div>
        
                    <div class="income-expenses container">
                      <div class="income d-flex justify-content-between income-container">
                        <h4 class="text-white">Income</h4>
                        <h4 class="text-white">{{ totalIncome }}</h4>
                      </div>
        
                      <div class=" expenses d-flex justify-content-between income-container">
                      <h4 class="text-white">Expenses</h4>
                      <h4 class="text-white">{{ totalExpenses }}</h4>
                    </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="jumbotron container">
            <form action="" @submit.prevent="addBudget()">
                  <div class="in-ex-selector">
                  <div class="input-sub">
                    <select name="" id="cont" v-model="budget.category">
                      <option value="income">Income</option>
                      <option value="expenses">Expenses</option>
                    </select>
                  </div>
          
                  <div class="input-sub">
                    <input type="text" name="" id="item" placeholder="Description" v-model="budget.desc" required> 
                  </div>
                  <div class="input-sub">
                    <input type="number" name="" id="item" v-model="budget.amount" required>
                  </div>
          
                  <div class="input-sub">
                    <button class="btn-btn add" type="submit">Add</button>
                  </div>
                </div>
            </form>
        
                <div class="container-md">
                  <div class="row">
                    <div class="col-md-6 col-sm-12">
                      <div class="bg-light rounded incomed">
                        <h5 class="text-center">Income</h5>
            
                        <div class="d-flex justify-content-between desc-value" v-for="(inc, index) in income" :key="index">
                          <div class="item-desc">{{ inc.desc }}</div>
                          <div class="item-value">
                            {{ inc.amount }}
                            <button class="delete-btn" v-on:click="deleteInc(index)">
                              <i class="fa fa-trash" aria-hidden="true"></i>
                            </button>
                          </div>
                        </div>
                      </div>
                    </div>
            
                    <div class="col-md-6 col-sm-12">
                      <div class="bg-light rounded expend">
                        <h5 class="text-center">Expenses</h5>
                        <div class="d-flex justify-content-between desc-value" v-for="(exp, index) in expense" :key="index">
                          <div class="item-desc">{{ exp.desc }}</div>
                          <div class="item-value">
                            {{ exp.amount }}
                            <button class="delete-btn" v-on:click="deleteExp(index)">
                              <i class="fa fa-trash" aria-hidden="true"></i>
                            </button>
                          </div>
                        </div>
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
        this.budget.category = "";
        this.budget.desc = "";
        this.budget.amount = "";
        this.getBudgets()
        this.getExpenses()
        this.getIncome()
        this.getBalance();
      },
      getIncome(){
          // Filtering All Budget List
          this.allBudgets = JSON.parse(localStorage.getItem("emmaaa"));
          this.income = this.allBudgets.filter((budget) => budget.category == 'income')
        if(this.totalIncome.length > 1) {
          this.totalIncome = this.income.map(item => +item.amount).reduce((prev, next) => prev + next);
        }
        else{
          this.totalIncome =  this.income.map(item => +item.amount)
        }
        this.unformattedIncome = this.totalIncome
        this.totalIncome = this.formatCurrency(this.totalIncome)
        this.getBudgets()
        this.getBalance()
        console.log('incomess');

      },
      getExpenses(){
          // Filtering All Budget List
          this.allBudgets = JSON.parse(localStorage.getItem("emmaaa"));
        this.expense = this.allBudgets.filter((budget) => budget.category == 'expenses');
        console.log(this.totalExpenses);
        if(this.expense.length > 1){
          this.totalExpenses = this.expense.map(item => +item.amount).reduce((prev, next) => prev + next);
        }
        this.unformattedExpenses = this.totalExpenses
        this.totalExpenses = this.formatCurrency(this.totalExpenses)
        this.getBudgets()
        this.getBalance()
        console.log('expenses')

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
      console.log('this.allBudgets');
      },

      deleteInc(index){
        if(confirm('Delete Item')) {
          let deletedItem = this.income.splice(index, 1)
        console.log(deletedItem)
        this.allBudgets.splice(index, 1)
        localStorage.setItem('emmaaa', JSON.stringify(this.allBudgets))
        }
        else{
          alert("Item not deleted")
        }
      },

      deleteExp(index){
        if(confirm('Delete Expense')) {
          let deletedItem = this.expense.splice(index, 1)
        console.log(deletedItem)
        this.allBudgets.splice(index, 1)
        localStorage.setItem('emmaaa', JSON.stringify(this.allBudgets))
        }
        else{
          alert("Item not deleted")
        }
      }
    },
    mounted(){
       
    },
    created(){
      localStorage.setItem('emmaaa', this.balance);
      // this.getBudgets()
      this.getMonth()
      // this.getIncome()
      // this.getExpenses()
    }
}
</script>


<style scoped>
</style>