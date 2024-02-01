
<script setup>
import { ref, computed } from "vue";


let count = ref(0);
let tasks = ref([]);
let newRow = ref({});
let taskCompleted = ref([]);
let task = ref('');
let taskEditValue = ref('');
let editIndex = ref(-1);


const addTasks = () => {

  if (!tasks.value.includes(task.value)) {
    tasks.value.push(task.value);
    task.value = '';
  }

}

const updateTask = (content) => {

  task.value = content

}

const addCompletedTasks = (task) => {

  const validated = confirm('¿ completaste la tarea ?')

  if (validated) {
    let newTasks = tasks.value.filter((t) => t != task)
    tasks.value = [...newTasks]

    taskCompleted.value.push(task);
  }

}

const deleteCompletedTasks = (task) => {

  const validated = confirm('¿ deseas elimar la tarea ?')

  if (validated) {
    let newTasks = taskCompleted.value.filter((t) => t != task)
    taskCompleted.value = [...newTasks]
  }

}


const resetTasks = () => {
  tasks.value = [];
}

const classCounter = computed(() => {

  if (count.value === 0) {
    return 'zero'
  }

  if (count.value > 0) {
    return 'positive'
  }

  if (count.value < 0) {
    return 'negative'
  }

})

const existTask = computed(() => {

  if (!tasks.value.includes(task.value)) {
    return false
  }

  return true

})


</script>

<template>
  <div class="text-3xl font-bold underline bg-red-500 text-center py-5 text-white">
    Todo List
  </div>

  <div>
    <div class="mt-10 w-1/2 mx-auto">
      <!-- <label for="large-input" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Into task</label>
      <input type="text" id="large-input" v-model="task"
        class="block w-full p-4 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-md focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700
         dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
      <div class="mt-5 flex justify-center">
        <button type="button" @click="addTasks" :disabled="existTask"
          class="focus:outline-none text-white bg-green-500 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900">Agregar</button>
        <button type="button" @click="resetTasks"
          class="focus:outline-none text-white bg-purple-700 hover:bg-purple-800 focus:ring-4 focus:ring-purple-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-purple-600 dark:hover:bg-purple-700 dark:focus:ring-purple-900">Limpiar</button>
      </div> -->

      <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
        <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
              <th scope="col" class="px-6 py-3">
                Task
              </th>
              <th scope="col" align="end" class="px-6 py-3">
                Actions
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700">
              <td class="font-medium text-gray-900 whitespace-nowrap dark:text-white pa-0">
                <input placeholder="Describe yoiur task..." v-model="task" class="outline-none bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" style="box-sizing: border-box"/>
              </td>
              <td class="pr-4">
                <div class="inline-flex w-full justify-end rounded-md shadow-sm" role="group">
                  <button :disabled="task == ''" @click="task = ''" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    CLEAR
                  </button>
                  <button :disabled="task == ''" @click="editIndex = -1;tasks.push(task); task = ''" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-e-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    ADD
                  </button>
                </div>
              </td>
            </tr>            
            <tr v-for="taskValue, i in tasks" class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700">
              <td class="font-medium text-gray-900 whitespace-nowrap dark:text-white pa-0">
                <input v-if="i == editIndex" placeholder="Describe yoiur task..." v-model="taskEditValue" class="outline-none bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" style="box-sizing: border-box"/>
                <div class="ml-3 py-3" v-else>{{taskValue}}</div>               
              </td>
              <td class="pr-4">
                <div class="inline-flex w-full justify-end rounded-md shadow-sm" role="group">
                  <button v-if="i != editIndex" :disabled="i == editIndex" @click="editIndex = i; taskEditValue = taskValue" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    EDIT
                  </button>
                  <button v-else @click="tasks[editIndex] = taskEditValue; editIndex = -1" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    UPDATE
                  </button>
                  <button @click="tasks.splice(tasks.indexOf(taskValue), 1)" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-e-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    DELETE
                  </button>
                </div>
              </td>
            </tr>
            <!-- <tr v-for="taskValue, i in tasks"
              class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700">
              <td class="px-6 py-2 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                <input v-if="i == editIndex" placeholder="Describe yoiur task..." v-model="taskEditValue" class="outline-none bg-transparent border-none text-gray-900 text-sm block w-full" style="box-sizing: border-box"/>
                <span v-else>{{taskValue}}</span>               
              </td>
              <td class="pr-4">
                <div class="inline-flex w-full justify-end rounded-md shadow-sm ma-0" role="group">
                  <button v-if="i != editIndex" :disabled="i == editIndex" @click="editIndex = i; taskEditValue = taskValue" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    EDIT
                  </button>
                  <button v-else @click="tasks[editIndex] = taskEditValue; editIndex = -1" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-s-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    UPDATE
                  </button>
                  <button @click="tasks.splice(tasks.indexOf(taskValue), 1)" type="button" class="px-4 py-2 text-xs font-medium text-gray-900 bg-white border border-gray-200 rounded-e-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-2 focus:ring-blue-700 focus:text-blue-700 dark:bg-gray-700 dark:border-gray-600 dark:text-white dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-blue-500 dark:focus:text-white">
                    DELETE
                  </button>
                </div>                
              </td>
            </tr> -->
          </tbody>
        </table>
      </div>
    </div>
    <!-- Main modal -->
    <div id="default-modal" tabindex="-1" aria-hidden="true"
      class="hidden overflow-y-auto overflow-x-hidden fixed top-0 right-0 left-0 z-50 justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full">
      <div class="relative p-4 w-full max-w-2xl max-h-full">
        <!-- Modal content -->
        <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
          <!-- Modal header -->
          <div class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600">
            <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
              Edit Task
            </h3>
            <button type="button"
              class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
              data-modal-hide="default-modal">
              <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
              </svg>
              <span class="sr-only">Close modal</span>
            </button>
          </div>
          <!-- Modal body -->
          <div class="p-4 md:p-5 space-y-4">
            <p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
              With less than a month to go before the European Union enacts new consumer privacy laws for its citizens,
              companies around the world are updating their terms of service agreements to comply.
            </p>
          </div>
          <!-- Modal footer -->
          <div class="flex items-center p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600">
            <button data-modal-hide="default-modal" type="button"
              class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">I
              accept</button>
            <button data-modal-hide="default-modal" type="button"
              class="ms-3 text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600">Decline</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
