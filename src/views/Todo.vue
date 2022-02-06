<template>
  <div class="container" style="max-width: 600px ">
    <!-- Heading -->
    <h2 class="text-center mt-5">การบ้านเยอะจังโว๊ย!!!</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      
      <input
        type="text"
        v-model="task"
        placeholder="Add todo"
        class="w-100 form-control"
      />
      <button class="btn btn-warning rounded-0" @click="submitTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead >
        <tr>
          <th scope="col">งานต่างๆ นาๆ</th>
          <th scope="col" style="width: 120px">สถานะ</th>
          <th scope="col" class="text-center">แก้ไข</th>
          <th scope="col" class="text-center">ลบ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'doing',
                'text-success': task.status === 'finished',
                
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
            
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <button class="btn btn-primary">Edit</button>
            </div>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <button class="btn btn-danger">Delete</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["doing", "finished"],

      /* Status could be: 'doing'  / 'finished' */
      tasks: [
        {
          name: "Thai",
          status: "doing",
        },
        {
          name: "English",
          status: "doing",
        },
        {
          name: "CS313",
          status: "finished",
        },
      ],
    };
  },

  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}


</style>