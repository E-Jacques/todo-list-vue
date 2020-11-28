<template>
  <div class="todo">
    <div class="effective-app">
      <header>
        <h1>Todo List</h1>
      </header>
      <div class="todo-app">
        <div class="input-section">
          <input
            type="text"
            class="input-text-todo"
            placeholder="Ajouter une tache à la liste"
            v-model="currentTask"
            @keyup.enter="addCurrentTodo"
          />
          <textarea
            class="input-text-desc"
            cols="30"
            rows="10"
            placeholder="Insérer une description pour votre tache"
            v-model="currentDesc"
            @keyup.enter="addCurrentTodo"
          ></textarea>
        </div>
        <div class="main-part">
          <ul class="task-list">
            <li class="task-ele" v-for="task in taskListShow" :key="task">
              <div class="task-view">
                <div
                  class="task-statue action"
                  @click="changeFinishState(task)"
                  :class="{ finishedStatue: task.finished }"
                ></div>
                <h1 class="task-label">{{ task.name }}</h1>
                <div class="control-set">
                  <p
                    class="extend_desc action"
                    @click="changeDeployement(task)"
                    v-if="asDescription(task) && !task.desc_deployed"
                  >
                    V
                  </p>
                  <p
                    class="extend_desc action"
                    @click="changeDeployement(task)"
                    v-if="asDescription(task) && task.desc_deployed"
                  >
                    ^
                  </p>
                  <p class="task-delete action" @click="deleteTask(task)">X</p>
                </div>
              </div>
              <div
                class="task-desc"
                :class="{ taskDescFinished: task.finished }"
                v-if="task.desc_deployed"
              >
                <div class="text-field">
                  <p>{{ task.desc }}</p>
                </div>
              </div>
            </li>
          </ul>
        </div>
        <div class="bottom-part">
          <p class="remaining-display">{{ remain }}</p>
          <div class="action-button">
            <p
              class="show-remaining action-text"
              :class="{ selection: mode === 'remain' }"
              @click="showRemain"
            >
              Remain
            </p>
            <p
              class="show-finished action-text"
              :class="{ selection: mode === 'finished' }"
              @click="showFinished"
            >
              Finished
            </p>
            <p
              class="show-all action-text"
              :class="{ selection: mode === 'all' }"
              @click="showAll"
            >
              All
            </p>
          </div>
        </div>
      </div>
    </div>
    <footer id="my-name">Emilien JACQUES</footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      taskList: [],
      currentTask: "",
      currentDesc: "",
      taskListShow: [],
      mode: "all",
    };
  },
  methods: {
    addCurrentTodo() {
      if (this.currentTask != "") {
        this.taskList.push({
          name: this.currentTask,
          desc: this.currentDesc,
          finished: false,
          desc_deployed: false,
        });
        this.currentTask = "";
        this.currentDesc = "";
      }

      this.updateTaskListShow();
    },

    updateTaskListShow() {
      if (this.mode == "all") {
        this.showAll();
      } else if (this.mode == "remain") {
        this.showRemain();
      } else {
        this.showFinished();
      }
    },

    showAll() {
      this.taskListShow = this.taskList;
      this.mode = "all";
    },

    showRemain() {
      this.taskListShow = this.taskList.filter((ele) => !ele.finished);
      this.mode = "remain";
    },

    showFinished() {
      this.taskListShow = this.taskList.filter((ele) => ele.finished);
      this.mode = "finished";
    },

    asDescription(task) {
      return task.desc != "";
    },

    deleteTask(task) {
      this.taskList = this.taskList.filter((ele) => ele !== task);
      this.updateTaskListShow();
    },

    changeFinishState(task) {
      task.finished = !task.finished;
    },

    changeDeployement(task) {
      task.desc_deployed = !task.desc_deployed;
    },
  },
  computed: {
    remain() {
      let nb = this.taskList.filter((ele) => !ele.finished).length;

      if (nb == 0) {
        return "Vous avez fini toutes vos taches !";
      }

      return `Il reste ${nb} taches à accomplir`;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src='../style.css' scoped></style>
