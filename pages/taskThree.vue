<template>
  <div>
    <div class="p-4">task3</div>
    <button class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded" @click="openAddModal">Add Employees</button>

    <div v-if="showAddModal" class="modal">
      <div class="modal-content bg-white p-6 mx-auto mt-16 w-80 rounded-lg shadow-lg">
        <h2 class="text-xl font-bold mb-4">Add Item</h2>
        <label class="block mb-2">
          <span class="text-gray-700">Name:</span>
          <input v-model="newItem.name" required class="form-input mt-1 block w-full" />
        </label>
        <label class="block mb-2">
          <span class="text-gray-700">Phone Number:</span>
          <input v-model="newItem.phoneNumber" required class="form-input mt-1 block w-full" />
        </label>
        <label class="block mb-2">
          <span class="text-gray-700">Gender:</span>
          <select v-model="newItem.gender" required class="form-select mt-1 block w-full">
            <option value="Male">Male</option>
            <option value="Female">Female</option>
            <option value="other">Other</option>
          </select>
        </label>
        <div class="flex justify-end">
          <button @click="saveNewItem" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded mr-2">Save</button>
          <button @click="closeAddModal" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded">Cancel</button>
        </div>
      </div>
    </div>

    <table class="w-full mt-8">
      <thead>
        <tr>
          <th class="border px-4 py-2">Name</th>
          <th class="border px-4 py-2">Phone Number</th>
          <th class="border px-4 py-2">Action</th>
        </tr>
      </thead>
      <!-- ---------output table data ----------------->
      <tr v-for="(item, index) in items" :key="index" class="border-t">
        <td class="border px-4 py-2">{{ item.name }}</td>
        <td class="border px-4 py-2">{{ item.phoneNumber }}</td>
        <td class="border px-4 py-2">
          <button @click="openEditModal(index)" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-1 px-2 rounded mr-1">Edit</button>
          <button @click="deleteItem(index)" class="bg-red-500 hover:bg-red-600 text-white font-bold py-1 px-2 rounded">Delete</button>
        </td>
      </tr>
    </table>

    <div v-if="showEditModal" class="modal">
      <div class="modal-content bg-white p-6 mx-auto mt-16 w-80 rounded-lg shadow-lg">
        <h2 class="text-xl font-bold mb-4">Edit Item</h2>
        <label class="block mb-2">
          <span class="text-gray-700">Name:</span>
          <input v-model="editedItem.name" required class="form-input mt-1 block w-full" />
        </label>
        <label class="block mb-2">
          <span class="text-gray-700">Phone Number:</span>
          <input v-model="editedItem.phoneNumber" required class="form-input mt-1 block w-full" />
        </label>
        <label class="block mb-2">
          <span class="text-gray-700">Gender:</span>
          <select v-model="editedItem.gender" required class="form-select mt-1 block w-full">
            <option value="male">Male</option>
            <option value="female">Female</option>
            <option value="other">Other</option>
          </select>
        </label>
        <div class="flex justify-end">
          <button @click="saveEditedItem" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded mr-2">Save</button>
          <button @click="closeEditModal" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

definePageMeta({
layout: 'name'
});

const flag=ref(false)

const newItem = ref({
name: '',
phoneNumber: '',
gender: 'male',
});

const editedItem = ref({
name: '',
phoneNumber: '',
gender: 'male',
});

const items = ref([]);
const localStorageKey = 'taskTree';

const showAddModal = ref(false);
const showEditModal = ref(false);
let editedItemIndex = null;

const openAddModal = () => {
newItem.value = { name: '', phoneNumber: '', gender: 'male' };
showAddModal.value = true;
};

const saveNewItem = () => {
if (items.value === null) {
  items.value = [];
}
items.value.push({ ...newItem.value });
console.log(items.value);
flag.value=true;
closeAddModal();
saveItemsToLocalStorage();
};

const closeAddModal = () => {
showAddModal.value = false;
};

const openEditModal = (index) => {
editedItemIndex = index;
editedItem.value = { ...items.value[index] };
showEditModal.value = true;
};

const closeEditModal = () => {
showEditModal.value = false;
};

const saveEditedItem = () => {
items.value[editedItemIndex] = { ...editedItem.value };
closeEditModal();
saveItemsToLocalStorage();
};

const deleteItem = (index) => {
items.value.splice(index, 1);
saveItemsToLocalStorage();
};

const saveItemsToLocalStorage = () => {
localStorage.setItem(localStorageKey, JSON.stringify(items.value));
};

// Load data from local storage when the component is mounted
onMounted(() => {
const savedItems = localStorage.getItem(localStorageKey);
console.log('savedItems', savedItems);
items.value = JSON.parse(savedItems);
});
</script>

<style scoped>

.b{
  color: white;
  background-color: rgb(39, 138, 39);
  border: rgb(13, 13, 13) solid 1px;
  padding-right: 30px;
  padding-left: 30px;

}
.d{
  margin-top: 10%;
  /* color: red; */
}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.modal {
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background: #fff;
  padding: 20px;
  width: 300px;
  margin: 100px auto;
}
</style>
  