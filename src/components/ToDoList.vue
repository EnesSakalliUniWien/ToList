<template>
  <main id="todolist">
    <h1>
      Todo List
      <span>Get things done, one item at a time.</span>
    </h1>
    <template v-if="todo.length">
      <transition-group name="todolist" tag="ul">
        <li
          v-for="item in todo"
          v-bind:class="item.done ? 'done' : ''"
          v-bind:key="item.id"
        >
          <span class="label">{{ item.label }}</span>
          <div class="actions">
            <div>
              <v-checkbox
                style="margin-left:45px; margin-right:45px;"
                color="red darken-3"
                v-model="item.done"
              ></v-checkbox>
            </div>
            <div>
              <v-btn
                x-small
                style="margin-left:45px; margin-right:45px;"
                v-on:click="deleteItemFromList(item)"
                aria-label="Delete"
                title="Delete"
                ><v-icon>{{ "mdi-delete" }}</v-icon></v-btn
              >
            </div>
          </div>
        </li>
      </transition-group>
    </template>
    <form name="newform" v-on:submit.prevent="addItem">
      <label for="newItem">Add to the todo list</label>
      <input type="text" name="newItem" id="newItem" v-model="newItem" />
      <v-btn large style="margin-left:1rem; margin-top:1px" type="submit"
        >Add item</v-btn
      >
    </form>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

export interface ToDoListItem {
  id: number;
  label: string;
  done: boolean;
}

@Component({})
export default class ToDoList extends Vue {
  public newItem = "";
  public sortByStatus = false;
  public isActive = true;
  public todo = [
    { id: 1, label: "Learn VueJs", done: true },
    { id: 2, label: "Code a todo list", done: false },
    { id: 3, label: "Learn something else", done: false },
  ];

  public onToogle() {
    this.$emit("clicked", this.isActive);
  }

  public addItem() {
    this.todo.push({
      id: Math.floor(Math.random() * 9999) + 10,
      label: this.newItem,
      done: false,
    });

    this.newItem = "";
  }

  public deleteItemFromList(item: ToDoListItem) {
    const index = this.todo.indexOf(item);
    this.todo.splice(index, 1);
  }

  public clickontoogle(active: boolean) {
    this.sortByStatus = active;
  }

  public todoByStatus() {
    if (!this.sortByStatus) {
      return this.todo;
    }

    let sortedArray = [];
    const doneArray = this.todo.filter(function(item) {
      return item.done;
    });
    const notDoneArray = this.todo.filter(function(item) {
      return !item.done;
    });

    sortedArray = [...notDoneArray, ...doneArray];
    return sortedArray;
  }

  public markAsDoneOrUndone(item: ToDoListItem) {
    item.done = !item.done;
    console.log("ass");
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html,
body {
  background: #f7f1f1;
  font-size: 1.1rem;
  font-family: "Quicksand", sans-serif;
  height: 100%;
}
@keyframes strikeitem {
  to {
    width: calc(100% + 1rem);
  }
}

#todolist {
  margin: 4rem auto;
  padding: 2rem 3rem 3rem;
  max-width: 500px;
  background: #ff6666;
  color: #fff;
  box-shadow: -20px -20px 0px 0px rgba(100, 100, 100, 0.1);
}
#todolist h1 {
  /*text-align:center;*/
  font-weight: normal;
  font-size: 2.6rem;
  letter-spacing: 0.05em;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
}
#todolist h1 span {
  display: block;
  font-size: 0.8rem;
  margin-bottom: 0.7rem;
  margin-left: 3px;
  margin-top: 0.2rem;
}

#todolist .emptylist {
  margin-top: 2.6rem;
  text-align: center;
  letter-spacing: 0.05em;
  font-style: italic;
  opacity: 0.8;
}
#todolist ul {
  margin-top: 2.6rem;
  list-style: none;
}
#todolist .todolist-move {
  transition: transform 1s;
}
#todolist li {
  display: flex;
  margin: 0 -3rem 4px;
  padding: 1.1rem 3rem;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
}

#todolist .actions {
  flex-shrink: 0;
  padding-left: 0.7em;
}
#todolist .label {
  position: relative;
  transition: opacity 0.2s linear;
}
#todolist .done .label {
  opacity: 0.6;
}
#todolist .done .label:before {
  content: "";
  position: absolute;
  top: 50%;
  left: -0.5rem;
  display: block;
  width: 0%;
  height: 1px;
  background: #fff;
  animation: strikeitem 0.3s ease-out 0s forwards;
}
#todolist .btn-picto {
  border: none;
  background: none;
  -webkit-appearance: none;
  cursor: pointer;
  color: #fff;
}

form {
  margin-top: 3rem;
  display: flex;
  flex-wrap: wrap;
}
form label {
  min-width: 100%;
  margin-bottom: 0.5rem;
  font-size: 1.3rem;
}
form input {
  flex-grow: 1;
  border: none;
  background: #f7f1f1;
  padding: 0 1.5em;
  font-size: initial;
}
form button {
  padding: 0 1.3rem;
  border: none;
  background: #ff6666;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
  border: 1px solid rgba(255, 255, 255, 0.3);
  margin-left: 5px;
  cursor: pointer;
  transition: background 0.2s ease-out;
}
form button:hover {
  background: #ff5e5e;
}
form input,
form button {
  font-family: "Quicksand", sans-serif;
  height: 3rem;
}
</style>
