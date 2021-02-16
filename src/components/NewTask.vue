<template>
  <div class="new-task">
    <input
      v-model="name"
      type="text"
      @keydown.enter="add"
      class="form-element"
      placeholder="Nova tarefa?"
      maxlength="50"
    />
    <button @click="add" class="form-element">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
    };
  },
  methods: {
    add() {
      if (this.name !== '') {
        this.$emit('taskAdded', { name: this.name });
        this.name = '';
      }
    },
  },
};
</script>

<style>
.new-task {
  margin: 35px;
}

.form-element {
  outline: none;
  font-size: 2rem;
  border: 1px solid #fff;
  padding: 5px 10px 8px;
  color: #fff;
}

input.form-element {
  width: 500px;
  background-color: #fff2;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}

button.form-element {
  border-left: none;
  background-color: #2196f3;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
}

@media only screen and (max-width: 540px) {
  .new-task {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  input.form-element {
    max-width: 70vw;
    max-height: 20px;
    padding: 10px;
    border-radius: 8px;
    font-size: 20px;
  }

  button.form-element {
    border-radius: 8px;
    margin-top: 20px;
    height: 50px;
    width: 50px;
    border-radius: 100%;
    display: flex;
    align-self: center;
    justify-content: center;
  }
}
</style>
