<template>
  <div>
    <div>ToDoList</div>
    <div id="to-do-list">
      <table>
        <thead>
          <tr><td>課題名</td><td>ステータス</td><td>完了</td></tr>
        </thead>
        <tbody>
          <tr 
            class="task" 
            :class="{ completed: task.isCompleted }"
            v-for="task in tasks"
            :key="task.name"
          >
            <td>{{ task.name }}</td>
            <td v-if="task.isCompleted">完了済</td>
            <td v-else>未完了</td>
            <td><button @click="task.isCompleted = true">完了</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <label>
        課題名
        <input v-model="newTaskName" type="text" />
      </label>
      <button @click="addtask">add</button>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const tasks = ref([]);
    
    const newTaskName = ref("");

    const addtask = () => {
      if (newTaskName.value === "") {
        return;
      }
      tasks.value.push({
        name: newTaskName.value,
        isCompleted: false,
      });
      newTaskName.value = "";
    };

    return { tasks, newTaskName, addtask };
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