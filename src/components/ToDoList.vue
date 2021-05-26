<template>
  <div class="container">
    <h1>ToDoList</h1>
    <div id="to-do-list">
      <table style="margin: auto">
        <thead>
          <tr><th>課題名</th><th>ステータス</th><th>完了</th><th>削除</th></tr>
        </thead>
        <tbody>
          <tr 
            class="task" 
            :class="{ completed: task.isCompleted }"
            v-for="task in sortedTasks"
            :key="task.id"
          >
            <td>{{ task.name }}</td>
            <td v-if="task.isCompleted">完了済</td>
            <td v-else>未完了</td>
            <td><button @click="task.isCompleted = true">完了</button></td>
            <td><button @click="removeTask(task.id)">削除</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <label>
        課題名
        <input v-model="newTaskName" v-on:keyup.enter="addTask" type="text" />
      </label>
      <button @click="addTask">add</button>
    </div>
  </div>
</template>

<script>
  import { ref, computed } from "vue";

  export default {
    setup() {
      const currentId = ref(0);
      const tasks = ref([]);
      
      const newTaskName = ref("");

      const addTask = () => {
        if (newTaskName.value === "") {
          return;
        }
        tasks.value.push({
          id: currentId.value++,
          name: newTaskName.value,
          isCompleted: false,
        });
        newTaskName.value = "";
      };

      const removeTask = taskId => {
        const targetTask = tasks.value.find(task => task.id === taskId);
        tasks.value.splice(tasks.value.indexOf(targetTask), 1);
      };

      const sortedTasks = computed(() => {
        return tasks.value.sort((a, b) => {
          if (a.isCompleted && !b.isCompleted) {
            return 1;
          } else if(!a.isCompleted && b.isCompleted) {
            return -1;
          } else {
            return a.id - b.id;
          }
        });
      });

      return { tasks, newTaskName, addTask, removeTask, sortedTasks };
    },
  };
</script>

<style>
#to-do-list {

}

.completed {
  color: red;
}

.disable {
  display: none;
}
</style>