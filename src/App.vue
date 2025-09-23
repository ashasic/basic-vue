<script setup>
</script>

<template>
  <div class="container">

    <div class="row">
      <div class="col">
        <h1>{{ title }}</h1>
      </div>
    </div>

    <hr />

    <div class="row">
      <div class="col">
          <input 
            v-model="message" 
            type="text" 
            placeholder="Type something..." 
            class="border rounded p-2 w-64"
          />
          <select v-model="selectedClass" class="border rounded p-2 w-32">
            <option disabled value="">-- Choose a course --</option>
            <option v-for="option in classList" :key="option" :value="option">
              {{ option }}
            </option>
          </select>
          <input 
            v-model="selectedDate" 
            type="date" 
            class="border rounded p-2 w-64"
          />
          <!-- Submit button -->
          <button @click="addEntry">
            Add
          </button> 
          <p>{{ message }}</p>
          <p>{{ selectedClass }}</p>
          <p>{{ selectedDate }}</p>
      </div>
      <div class="mt-6">
        <h3 class="font-bold mb-2">Entries:</h3>
        <ul class="list-disc pl-6">
          <li v-for="(item, index) in todoList" :key="index">
            {{ item.Desc }} — {{ item.Class }} — {{ item.Date }}
          </li>
        </ul>
      </div>
    </div>


    <hr />

    <div class="row">
      <div class="col">
        <p>This could be a footer</p>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: "InputExample",
    data() {
      return {
        message: "",
        selectedClass: "",
        selectedDate: "",
        todoList: [],
        classList: ["Class 1", "Class 2", "Class 3"],
        title: "To-do List"

      };
    },
    methods: {
      addEntry() {
        console.log("hello")
        if (this.message && this.selectedClass && this.selectedDate) {
          this.todoList.push({"Desc": this.message, "Class": this.selectedClass, "Date": this.selectedDate, "IsComplete": 0});
          
        } else {
          alert("Please fill all fields!");
        }
      }
    }
  };
</script>

