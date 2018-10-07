<template>




<div id="app" class="container my-3  text-light bg-dark">

  <h1 class="mt-5 d-flex justify-content-center">{{todolistH1}}</h1>


  <div class="input-group mb-3"  @click="canceledit()"> <!--點擊外圍取消編輯-->
    <div class="input-group-prepend" >
      <span class="input-group-text" id="basic-addon1">待辦事項</span>
    </div>
    <input type="text" class="form-control bg-dark text-light" placeholder="準備要做的任務" v-model="newtodo" @keyup.enter="addtodo">
    <div class="input-group-append"  >
      <button class="btn btn-light text-dark" @click="addtodo" type="button" >新增</button>
    </div>
  </div>
  <div class="card text-center " >
    <div class="card-header text-light bg-dark">
      <ul class="nav nav-tabs card-header-tabs "  @click="canceledit()"> <!--點擊外圍取消編輯-->
        <li class="nav-item"  >
          <a class="nav-link "  :class="{'active':visibility=='all'}" @click="visibility='all'" href="#">全部</a>
        </li>
        <li class="nav-item" >
          <a class="nav-link " :class="{'active':visibility=='active'}" @click="visibility='active'" href="#">進行中</a>
        </li>
        <li class="nav-item" >
          <a class="nav-link "  :class="{'active':visibility=='completed'}" @click="visibility='completed'"href="#">已完成</a>
        </li>
      </ul>
    </div>
    <ul class="list-group list-group-flush text-left text-dark ">
      <li class="list-group-item bg-light" v-for="(todo,key) in filtertodo"   >
        <div class="d-flex"  v-if="todo.id!==receivingTodo.id"  >
          <div class="form-check"  >
            <input type="checkbox" class="form-check-input " v-model="todo.completed">
            <div class="ml-5" @dblclick="edit(todo)"> 
            <label class="form-check-label " :for="todo.completed" :class="{'completed':todo.completed}" > 
              {{todo.title}}
            </label>

            </div>
          </div>
          <button type="button" class="close ml-auto" aria-label="Close" @click="removetodo(todo)">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
                          <input type="text" class="form-control" v-if="todo.id===receivingTodo.id" v-model="receivingTitle" @keyup.enter="edited(todo)">
      </li>
            <li class="list-group-item bg-light" v-for="todo in virtualTodos" @click="canceledit()"> <!--點擊外圍取消編輯-->
                <div class="d-flex">
                <div class="form-check">
                </div class="close ml-auto" >
                    <span aria-hidden="true" style="visibility:hidden;">&times;</span>
                </div>
            </li>
    </ul>
    <div class="card-footer d-flex justify-content-between" @click="canceledit()"> <!--點擊外圍取消編輯-->
      <span class="text-dark">還有  {{NotCompleterYet()}}  筆任務未完成</span>
      <a class="text-primary" href="#" @click="deleteall()">清除所有任務</a>
    </div>
  </div>
</div>
</template>
<script>
export default {
  name: 'app',
  data () {
      return{
        newtodo:"",
        todos:[
          {id:"",
          title:"(範例)我的任務",
          completed:false,
          }
        ],
        receivingTodo:{},
        receivingTitle:'',
        visibility:"all",
        NotCompleterYet:function(){
          return this.todos.filter(function (todo) {
          return todo.completed == false
          }).length;
          },
        todolistH1:"MY TO DO LIST"
}
  },
  methods:{
    addtodo:function(){
      var value= this.newtodo.trim();
      var timestamp = Math.floor(Date.now());
      console.log(value,timestamp);
      this.newtodo="";
      if(value==""){window.alert('請輸入文字') ;return}  //提醒 預防 使用者寫入無效待辦
      else{this.todos.push({
          id:timestamp,
          title:value,
          completed:false})
      };
    },
    removetodo:function(todo){
          var index='';
          this.todos.forEach(function(item,key){; if(item.id===todo.id){index=key}})
          this.todos.splice(index,1);
    },

    edit:function(todo){
      this.receivingTodo=todo;
      this.receivingTitle=todo.title.trim();
    },
    edited:function(todo){
      if(this.receivingTitle.trim()==''){
        window.alert('請輸入文字');  //提醒 預防 使用者寫入無效待辦
      }
      else
      {todo.title=this.receivingTitle;}
      this.receivingTitle='';
      this.receivingTodo={};
    },
    canceledit:function(){
      this.receivingTodo={};
    },
    deleteall:function(){
      var retVal = confirm("您確定要 清除 所有 代辦事項嗎?" ); 
          if( retVal == true ){
              this.visibility="all";
              console.log(this);
              this.todos.length=0;
              var emptys = []
                  for (var i=this.filtertodo.length; i<10; i++) {
                  emptys.push({})
              }

              return emptys
	            return true;

            }
          else{
	            return false;
            }

        },
 
  },

    computed:{
        filtertodo:function(){

            switch(this.visibility){
              case 'all': 
                return this.todos;
              case 'active':
                var newtodos =[];
                this.todos.forEach(function(todo){
                   if(!todo.completed){newtodos.push(todo);}})
                return newtodos;
              case 'completed':
                var newtodos =[];
                this.todos.forEach(function(todo){
                  if(todo.completed){newtodos.push(todo);}})
                return newtodos;
            }//用switch代替if省效能
        },
        virtualTodos: function() {
          var emptys = []
          for (var i=this.filtertodo.length; i<10; i++) {emptys.push({})}
          return emptys
        },
    }
}
</script>


<style>
.completed {
  text-decoration: line-through
}
a{
  color: #FFFFFF
  }
.nav-link:hover{
  color: #FFFFFF
  }

</style>
