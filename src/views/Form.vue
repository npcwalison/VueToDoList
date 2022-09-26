<template>
  <div class="container mt-2">
    <b-form>
      <b-form-group label="Title" label-for="subject">
        <b-form-input
          id="subject"
          v-model="form.subject"
          type="text"
          placeholder="Ex: lava carro"
          required
          autocomplete="off"
        >
        </b-form-input>
      </b-form-group>

      <b-form-group label="Descrição" label-for="description">
        <b-form-textarea
          id="description"
          v-model="form.description"
          type="text"
          placeholder="Ex: levar o carro para lavar no fim de semana"
          required
          autocomplete="off"
        >
        </b-form-textarea>
      </b-form-group>

      <b-button type="submit" variant="outline-primary" @click="saveTask"> Salvar </b-button>
    </b-form>
  </div>
</template>

<script>
  export default {
    name: "Form",
    data() {
      return {
        form: {
          subject: "",
          description: "",
        },
        methodsSave: 'new'
      };
    },
    created() {
      if(this.$route.params.index === 0 || this.$route.params.index !== undefined) {
        this.methodsSave = 'update'
        let tasks = JSON.parse(localStorage.getItem("tasks"))
        this.form = tasks[this.$route.params.index]
      }
    },
    methods: {
      saveTask() {
        if(this.methodsSave === "update"){
          let tasks = JSON.parse(localStorage.getItem("tasks"))
          tasks[this.$route.params.index] = this.form;
          localStorage.setItem("tasks", JSON.stringify(tasks))
          this.$router.push({ name: "list" })
          return;
        }
        //verifica se há alguma task presente no localStorage, caso não haja nenhuma, ele retorna um array vazio
        let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
        //adicionando tarefas do form no tasks, do tasks no localStorage
        tasks.push(this.form)
        //converte as tasks em string
        localStorage.setItem("tasks", JSON.stringify(tasks))
        //redireciona para rota de lista
        this.$router.push({ name: "list" })
      }
    }
  };
</script>
