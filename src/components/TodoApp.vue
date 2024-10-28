<script>

export default{
  name: "",
  props: {
    msg:String,
  
},

// массив задач
data(){
  return {
    // очищать поле перед новой задачей
    task: '', 

    //для редактирования , обнуленный элемент 
    editedTask:null,

    tasks: [{
      name:'task1',
      status:'to-do'
    },
    {
      name:'task2',
      status:'in progress'
    }
  
  ]
  }
},


//функции или метод 
methods: {

  // при нажатии на кнопку Отправить
  submitTask(){
    // console.log('salam')
    // если ничего не набрано в поле, то ничего не вренется
    if(this.task.length ===0) return;

   
    // Если редактируемая задача null то есть, юзер 
    //  не нажал на Редактирование, тогда добавляется Новая Задача
    if(this.editedTask===null){
       //ввод (push) новой задачи в массив data..tasks
    this.tasks.push({
      name: this.task,
      //по умолчанию статус новой задачи
      status: 'в процессе'
    });
  } else { 
  //редактирование элемента и после задание его значения на null
    this.tasks[this.editedTask].name = this.task;
    this.editedTask = null;
  }
  this.task = '';
  },



  //удаление задачи через splice и ключ index, что определили 
  //до этого в выборке данных  <tr v-for="(task, index) in tasks" :key="index">
 
    deleteTask(index){
      //1 - только один элемент удаляем
this.tasks.splice(index, 1);
    },


//редактирование задачи

editTask(index){
  this.task = this.tasks[index].name;
//при нажатии на Редактирование на editedTask передаем index 
// изменяемого элемента для дальнейшего дествия с ним
this.editedTask = index;
  
}

}
};


</script>


<template>
  <div class="container">
    <h2 class="text-center" mt-5> my todo-app</h2>
<!-- для ввода задачи в поле -->
  <div class="d-flex">
    <input v-model="task" type="text" placeholder="введите задачу" class="form-control">
  <button @click="submitTask" class="btn btn-warning rounded-0">отправить</button>
  </div>
  
  <!-- шаблон таблицы из https://getbootstrap.com/docs/5.0/content/tables/ -->

  <table class="table table-dark table-striped mt-4">
  <thead>
    <tr>
      <th scope="col">Задача</th>
      <th scope="col">Статус</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>

  <tbody>
    <!-- //вытаскиваем данные для таблицы -->
    <tr v-for="(task, index) in tasks" :key="index">
      <th>{{task.name}}</th>
      <td>{{task.status}}</td>

<!-- кнопки 
 npm install --save-dev @fortawesome/fontawesome-free
js:
import '@fortawesome/fontawesome-free/css/all.css'
import '@fortawesome/fontawesome-free/js/all.js'-->
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>

       <td>
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash" ></span>
        </div>
      </td>

    </tr>
   </tbody>
</table>
  
  </div>
</template>



<style scoped>

</style>
