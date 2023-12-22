<template>
    <h2>Создать заметку</h2>
    <form>
        <div class="row">
            <div class="col-md-5 mb-3">
                <label for="task_name">Задача:</label>
                <input type="text" class="form-control" id="task_name" v-model="task_name" placeholder="Введите задачу"/>
            </div>
            <div class="col-md-2 mb-3">
                <label for="type">Тип:</label>
                <select class="form-control" id="type" v-model="type">
                    <option>Личное</option>
                    <option>Работа</option>
                    <option>Мероприятие</option>
                    <option>Другое</option>
                </select>
            </div>
        </div>
            <div class="form-check">
            <input class="form-check-input" type="radio" name="high_priority" id="high_priority" v-model="priority" value="Срочно" checked>
            <label class="form-check-label" for="high_priority">
                Срочно
            </label>
        </div>
            <div class="form-check">
            <input class="form-check-input" type="radio" name="low_priority" id="low_priority" v-model="priority" value="Не срочно">
            <label class="form-check-label" for="low_priority">
                Не срочно
            </label>
        </div>

        <div class="row">
            <div class="col-md-1">
                <button @click="add" class="btn btn-primary">Создать</button>
            </div>
            <div class="col-md-1">
                <button @click="clean" class="btn btn-secondary">Очистить</button>
            </div>
        </div>

    </form> 
    <h2>Список заметок</h2>
    <div class="card">
        <div class="card-body">
            <ul>
                <li v-for="note in this.notes">
                    <div>
                        <p><input type="checkbox" v-model="note.check" id="check"  class="form-check-input"/><b>{{ note.task_name }} </b>: {{ note.type }} ({{ note.priority }})</p>
                    </div>
                </li>
            </ul>
        </div>
    </div> 

    <div class="button-reset">
        <div class="col-md-1">
            <button @click="reset" class="btn btn-dark">Удалить все заметки</button>
        </div>
    </div>

</template>

<style>

    h2, form, .button-reset{
        margin-left: 20px
    }
    ul {
        list-style: none;
    }
    b{
        margin-left: 5px;
    }
</style>

<script>
export default {
  data() {
    return {
      task_name: '',
      type: 'Личное',
      priority: 'Срочно',
      check: false,
      notes: JSON.parse(localStorage.getItem('notes')) || [],
    };
  },

  methods: {
    add() {
      if(this.task_name !== ''){
        const newNote = {
            task_name: this.task_name,
            type: this.type,
            priority: this.priority,
            check: this.check,
        };
        this.notes.push(newNote);
        this.save();
        this.clean();
      };
    },
    save() {
      localStorage.setItem('notes', JSON.stringify(this.notes));
    },
    clean() {
      this.task_name = '';
      this.type = 'Личное';
      this.priority = 'Срочно';
      this.check = false;
    },
    reset() {
      localStorage.removeItem('notes');
      this.notes = [];
    },
  },
};
</script>