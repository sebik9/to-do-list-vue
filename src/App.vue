<template>
  <div class="header">
    <h2>To Do List</h2>
    <input id="myInput" v-model="inputValue" placeholder="Task..." />
    <span class="addBtn" id="addButton" @click="addItem">Add</span>
  </div>

  <div class="error-message">{{ showError }}</div>
  <ul>
    <li
      v-for="(item, i) in items"
      :key="i"
      @click="toggleStatus(i)"
      :class="{ checked: item.checked }"
    >
      {{ item.text }}
      <span @click="removeTask(i)" class="close"> &#215;</span>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      inputValue: '',
      showError: '',
      timer: null
    }
  },
  methods: {
    addItem() {
      if (this.inputValue.trim() === '') {
        this.showError = 'Must provide task'
        clearTimeout(this.timer)

        this.timer = setTimeout(() => {
          this.showError = ''
        }, 3000)
        return
      }
      this.items.push({ text: this.inputValue, checked: false })
      this.inputValue = ''
    },
    removeTask(i) {
      clearTimeout(this.timer);
      this.items.splice(i, 1);
      this.$nextTick(() => {
        if (i >= 0 && i < this.items.length) {
          this.items[i].checked = !this.items[i].checked;
        }
      })
    },
    toggleStatus(i) {
      if (i >= 0 && i < this.items.length) {
      this.items[i].checked = !this.items[i].checked
      }
    }
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

ul {
  margin: 0;
  padding: 0;
}

.header {
  background-color: #a7c7e7;
  padding: 30px 40px;
  text-align: center;
  width: 38%;
}

.header:after {
  content: '';
  display: table;
  clear: both;
}

h2 {
  text-align: center;
}

#myInput {
  margin: 0;
  border: none;
  border-radius: 0;
  width: 75%;
  padding: 10px;
  float: left;
  font-size: 16px;
}

.addBtn {
  padding: 10px;
  width: 25%;
  min-width: 40px;
  background: #d9d9d9;
  color: #555;
  float: left;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
  border-radius: 0;
}

.addBtn:hover {
  background-color: green;
  color: white;
  transition: 0.3s;
}

.addBtn:active {
  background: #d9d9d9;
  color: #555;
}

ul li:nth-child(odd) {
  background: #f9f9f9;
}

ul li:hover {
  background: #ddd;
}

ul li {
  cursor: pointer;
  position: relative;
  width: 38%;
  padding: 12px 8px 12px 40px;
  background: #eee;
  font-size: 18px;
  transition: 0.2s;

  /* make the list items unselectable */
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

ul li.checked {
  background: #888;
  color: #fff;
  text-decoration: line-through;
}

ul li.checked::before {
  content: '';
  position: absolute;
  border-color: #fff;
  border-style: solid;
  border-width: 0 2px 2px 0;
  top: 10px;
  left: 16px;
  transform: rotate(45deg);
  height: 15px;
  width: 7px;
}

.close {
  position: absolute;
  right: 0;
  top: 0;
  padding: 12px 16px 12px 16px;
  transition: 0.3s;
}

.close:hover {
  background-color: red;
  color: white;
  transition: 0.3s;
}
</style>
