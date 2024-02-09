<template>
  <div class="container mx-auto py-8">
    <div class="text-3xl font-bold mb-4">TASK4</div>
    <div class="relative">
      <button @click="openForm" class="absolute top-0 right-0 bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded">Add Contact</button>
      <div v-if="isFormOpen" class="modal">
        <div class="modal-content bg-white p-6 mx-auto mt-16 w-80 rounded-lg shadow-lg">
          <span class="close absolute top-0 right-0 text-gray-700 cursor-pointer" @click="closeForm">&times;</span>
          <form @submit.prevent="saveContact" class="space-y-4">
            <label for="name" class="block">
              Name:
              <input v-model="name" type="text" required class="form-input mt-1 w-full" />
            </label>
            <label for="phone" class="block">
              Phone Number:
              <input v-model="phone" type="text" required class="form-input mt-1 w-full" />
            </label>
            <label for="country" class="block">
              Country:
              <select v-model="country" required class="form-select mt-1 w-full">
                <option value="India">India</option>
                <option value="USA">USA</option>
                <option value="Canada">Canada</option>
              </select>
            </label>
            <button type="submit" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded">Save Contact</button>
          </form>
        </div>
      </div>
    </div>

    <!-- //search -->
    <div class="mt-4 mb-8">
    <input v-model="searchQuery" type="text" placeholder="Search Contacts" class="form-input px-4 py-2 rounded-md border border-gray-300 mr-28 w-full" />
  </div>
    <div class="mt-8 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
      <div v-for="contact in filteredContacts" :key="contact.id" class="contact-box relative">
        <div class="square-box border p-4">
          <span class="close-box absolute top-0 right-0 text-gray-700 cursor-pointer" @click="confirmRemove(contact.id)">&times;</span>
          <p class="font-bold">Name: {{ contact.name }}</p>
          <p>Phone Number: {{ contact.phone }}</p>
          <p>Country: {{ contact.country }}</p>
        </div>
      </div>
    </div>

    <div v-if="confirmBox.isOpen" class="confirmation-box flex items-center justify-center fixed top-0 left-0 w-full h-full bg-black bg-opacity-50">
      <div class="confirmation-content bg-white p-4 rounded border">
        <p>Are you sure you want to delete this contact?</p>
        <div class="mt-4">
          <button @click="removeConfirmed" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded mr-2">Yes</button>
          <button @click="cancelRemove" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded">No</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const isFormOpen = ref(false);
const confirmBox = ref({ isOpen: false, contactId: null });
const name = ref('');
const phone = ref('');
const country = ref('');
const savedContacts = ref([]);
const searchQuery = ref('');

const openForm = () => {
  isFormOpen.value = true;
};

const closeForm = () => {
  resetForm();
};

const saveContact = () => {
  const newContact = {
    id: Date.now(),
    name: name.value,
    phone: phone.value,
    country: country.value,
  };

  savedContacts.value.push(newContact);
  storeLaocal();
  resetForm();
};

const resetForm = () => {
  isFormOpen.value = false;
  name.value = '';
  phone.value = '';
  country.value = '';
};

const confirmRemove = (id) => {
  confirmBox.value.isOpen = true;
  confirmBox.value.contactId = id;
};

const removeConfirmed = () => {
  const idToRemove = confirmBox.value.contactId;
  savedContacts.value = savedContacts.value.filter((contact) => contact.id !== idToRemove);
  confirmBox.value.isOpen = false;
  confirmBox.value.contactId = null;
};

const cancelRemove = () => {
  confirmBox.value.isOpen = false;
  confirmBox.value.contactId = null;
};

const filteredContacts = computed(() => {
  return savedContacts.value.filter((contact) =>
    contact.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const key = 'ContactDetails';

const storeLaocal = () => {
  localStorage.setItem(key, JSON.stringify(savedContacts.value));
};

onMounted(() => {
const savedItems = localStorage.getItem(key);
console.log('savedItems', savedItems);
savedContacts.value = JSON.parse(savedItems);
});
definePageMeta({
        layout: 'name'
    })
</script>

<style scoped>
/* Your existing scoped styles */
</style>
