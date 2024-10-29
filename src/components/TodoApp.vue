<template>
  <div class="container">
    <h2 class="text-center" mt-7>ПланиРовщик Задач</h2>
<!-- для ввода задачи в поле -->
  <div class="d-flex">
    <input v-model="task" type="text" placeholder="введите задачу" class="form-control">
  <button @click="submitTask" class="btn btn-warning rounded-0">отправить</button>
  </div>
  
  <!-- шаблон таблицы из https://getbootstrap.com/docs/5.0/content/tables/ -->

  <table class="table table-dark table-striped mt-4">
  <thead>
    <tr   style="width: 160px;">
      <th scope="col">Задача</th>
      <th scope="col">Статус</th>
      <th scope="col" class="text-center">изменить</th>
      <th scope="col" class="text-center">удалить</th>
    </tr>
  </thead>

  <tbody>
    <!-- //вытаскиваем данные для таблицы -->
    <tr v-for="(task, index) in tasks" :key="index" >
    
    <td>
       <!-- перечекнуть при Закончено class="{'finished':  'Закончено' -->
         <span :class="{'finished': task.status === 'Закончено'}"> 
       {{task.name}}
        </span>
   </td>
     
      <td >
         <!-- курсор  меняется на pointer в статусе, для восприятия что можно менять Статусы  -->
         <!-- changeStatus(index) меняет Статусы -->
         <span @click="changeStatus(index)" class="pointer"> 
          {{task.status}}
          <!-- {{firstCharUpper(task.status)}} -->
        </span>

      </td>


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


<!-- -------------------------------------------------------------------------- -->


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
//масссив статусов
availeableStatusses: ['Сделать-то', 'В процессе', 'Закончено'],

//примеры в начале
tasks: [{
      name:'task1',
      status:'В процессе'
    },
    {
      name:'task2',
      status:'Сделать то'
    }
  
  ]
  }
},


//функции или метод 
methods: {

  // при нажатии на кнопку Отправить
  submitTask(){
    // console.log('salam')
    // если ничего не набрано в поле, то ничего не вернется
    if(this.task.length ===0) return;

   
    // Если редактируемая задача null,  
    //  тогда понимаем , что  юзер не Нажал на Редактирование,
    // тогда добавляется Новая Задача

    if(this.editedTask===null){
       //ввод (push) новой задачи в массив data..tasks
    this.tasks.push({
      name: this.task,
      //по умолчанию статус новой задачи
      status: 'Сделать-то'
    });
  } else { 
  // и если Редактирование элемента то после  изменения
  //  его  заново значение для editedTask  задаем null
    this.tasks[this.editedTask].name = this.task;
    this.editedTask = null;
  }
  this.task = '';
  },



  //удаление задачи через встроен. функ. Splice и ключ index, что определили 
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
},


//изменение статуса. Сначала находи текущий статус через indexOf, потом увеличаем его через инкремент ++,
// меняя на след статус (to-do на inprogress, inporg-finish// finish обнуляется опять)
//

changeStatus(index){
  //newIndex текущ статус
   let newIndex = this.availeableStatusses.indexOf(this.tasks[index].status);
   // если индекс больше чем на 2 (всего 3 статуса, после 2  это значить конец масссива занчении статусов, 
   // то обнуляем его.. на to-do то есть возврат, все сначала опять)
   if(++newIndex > 2) newIndex = 0;
   //изменение статуса
   this.tasks[index].status = this.availeableStatusses[newIndex];
   
}

}
};


</script>




<style scoped>
/* курсор статуса */
.pointer { 
cursor: pointer;}

/* декор для Статуса Закончено */
.finished {
  text-decoration:line-through;
}

</style>
