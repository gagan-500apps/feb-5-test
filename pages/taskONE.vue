<template>
    <div>
      <button @click="openForm" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Add Employee</button>
  
      <!-- Add Employee Form Modal -->
      <div v-if="flag" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
        <div class="bg-white p-8 rounded border border-gray-300">
          <form @submit.prevent="saveEmp">
            <div class="mb-4">
              <label for="name" class="block">Name:</label>
              <input type="text" v-model="name" id="name" class="block w-full border border-gray-300 rounded-md py-2 px-3">
            </div>
            <div class="mb-4">
              <label for="age" class="block">Age:</label>
              <input type="number" v-model="age" id="age" class="block w-full border border-gray-300 rounded-md py-2 px-3">
            </div>
            <div class="mb-4">
              <label for="gender" class="block">Gender:</label>
              <select v-model="gender" id="gender" class="block w-full border border-gray-300 rounded-md py-2 px-3">
                <option value="male">Male</option>
                <option value="female">Female</option>
              </select>
            </div>
            <div class="mb-4">
              <label for="date" class="block">Date:</label>
              <input type="date" v-model="date" id="date" class="block w-full border border-gray-300 rounded-md py-2 px-3">
            </div>
            <div class="mb-4">
              <label for="designation" class="block">Designation:</label>
              <select v-model="designaton" id="designation" class="block w-full border border-gray-300 rounded-md py-2 px-3">
                <option value="Developer">Developer</option>
                <option value="Tester">Tester</option>
              </select>
            </div>
            <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Save</button>
          </form>
        </div>
      </div>
  
      <!-- Edit Employee Form Modal -->
      <div v-if="edithflag" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
        <div class="bg-white p-8 rounded border border-gray-300">
          <form @submit.prevent="saveedithEmp">
            <div class="mb-4">
              <label for="editedName" class="block">Name:</label>
              <input type="text" v-model="editedItem.name" id="editedName" class="block w-full border border-gray-300 rounded-md py-2 px-3">
            </div>
            <div class="mb-4">
              <label for="editedAge" class="block">Age:</label>
              <input type="number" v-model="editedItem.age" id="editedAge" class="block w-full border border-gray-300 rounded-md py-2 px-3">
            </div>
            <div class="mb-4">
              <label for="editedGender" class="block">Gender:</label>
              <select v-model="editedItem.gender" id="editedGender" class="block w-full border border-gray-300 rounded-md py-2 px-3">
                <option value="male">Male</option>
                <option value="female">Female</option>
              </select>
            </div>
            <div class="mb-4">
              <label for="editedDate" class="block">Date:</label>
              <input type="date" v-model="editedItem.date" id="editedDate" class="block w-full border border-gray-300 rounded-md py-2 px-3">
            </div>
            <div class="mb-4">
              <label for="editedDesignation" class="block">Designation:</label>
              <select v-model="editedItem.designaton" id="editedDesignation" class="block w-full border border-gray-300 rounded-md py-2 px-3">
                <option value="Developer">Developer</option>
                <option value="Tester">Tester</option>
              </select>
            </div>
            <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Save</button>
          </form>
        </div>
      </div>
  
      <!-- Employee Table -->
      <table  v-if="flag1" class="w-full border border-gray-300">
        <thead>
          <tr>
            <th class="border border-gray-300 px-4 py-2">Name</th>
            <th class="border border-gray-300 px-4 py-2">Age</th>
            <th class="border border-gray-300 px-4 py-2">Gender</th>
            <th class="border border-gray-300 px-4 py-2">Date Of Joining</th>
            <th class="border border-gray-300 px-4 py-2">Designation</th>
            <th class="border border-gray-300 px-4 py-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in storeData" :key="index">
            <td class="border border-gray-300 px-4 py-2">{{ item.name }}</td>
            <td class="border border-gray-300 px-4 py-2">{{ item.age }}</td>
            <td class="border border-gray-300 px-4 py-2">{{ item.gender }}</td>
            <td class="border border-gray-300 px-4 py-2">{{ item.date }}</td>
            <td class="border border-gray-300 px-4 py-2">{{ item.designaton }}</td>
            <td class="border border-gray-300 px-4 py-2">
              <button @click="edith(index)" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Edit</button>
              <button @click="deletedata(index)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const flag = ref(false);
  const name = ref('');
  const age = ref('');
  const gender = ref('');
  const date = ref('');
  const designaton = ref('');
  const storeData = ref([]);
  const flag1=ref(false)

  const openForm = () => {
    flag.value = true;
  };
  
  const saveEmp = () => {
    const newData = {
      name: name.value,
      age: age.value,
      gender: gender.value,
      date: date.value,
      designaton: designaton.value
    };
  console.log('newdata',newData);
    // storeData.value.push({...newData});
    storeData.value.push(newData);
    console.log('sd',storeData.value);
    saveItemsToLocalStorage()
    flag1.value=true;
    resetForm();
  };
  
  const resetForm = () => {
    flag.value = false;
    name.value = '';
    age.value = '';
    gender.value = '';
    designaton.value = '';
    // saveItemsToLocalStorage()
  };
  
  let edithIndex = null;
  const editedItem = ref({
    name: '',
    age: '',
    gender: '',
    designaton: ''
  });
  const edithflag = ref(false);
  
  const edith = (index) => {
    edithIndex = index;
    editedItem.value = { ...storeData.value[index] };
    edithflag.value = true;
  };
  
  const saveedithEmp = () => {
    storeData.value[edithIndex] = { ...editedItem.value };
    saveItemsToLocalStorage()
    edithflag.value = false;
  };
  

  const localStorageKey='taskOne'
  const saveItemsToLocalStorage = () => {
    console.log("ls");
localStorage.setItem(localStorageKey, JSON.stringify(storeData.value));
};

// Load data from local storage when the component is mounted
onMounted(() => {
const savedItems = localStorage.getItem(localStorageKey);
console.log('savedItems', savedItems);
storeData.value = JSON.parse(savedItems);
});

  const deletedata = (index) => {
    storeData.value.splice(index, 1);
    saveItemsToLocalStorage()
  };

  definePageMeta({
        layout: 'name'
    })
  </script>
  