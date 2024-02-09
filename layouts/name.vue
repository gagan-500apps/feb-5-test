<template>
    <div>
      <header>
        <nav>
          <ul class="navbar">
            <li><NuxtLink  to="/taskONE">TaskONE</NuxtLink></li>
            <li><NuxtLink  to="/taskThree">TaskThree</NuxtLink></li> 
            <li><NuxtLink  to="/taskFour">TaskFour</NuxtLink></li> 
            

          <li>UserName: {{ userName }}</li>
          </ul>
        </nav>
      </header>
      <slot />
    </div>
</template>

<script setup> 

import {validation} from '../stores/validation'
let userName=ref()
userName = validation().loginUser_data[0]?.Email;
console.log('LOGINNAME',userName);




// onMounted(()=> {
//   const storeLocalStorage=()=>{
//   localStorage.setItem(localStorageKey, JSON.stringify(userName));
// };
// storeLocalStorage()
// })


// const loginData={}
// onMounted(()=>{
//    loginData=localStorage.getItem(loginUser_data);
//     // userName = JSON.parse(name);
//     // console.log('userName', userName);
//     // console.log('loginData',loginData);
//     userName=loginData.Email
// })


</script>



<style>

.navbar {
    list-style: none;
    padding: 3%;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50px; /* Set a fixed height for the navbar */
    background-color: #0c3478; /* Set a background color for the navbar */
    color: #ebe9e9;
  }

  /* Style for each list item in the navbar */
  .navbar li {
    margin: 0 25px; /* Adjust the spacing between the list items */
  }

  /* Optional: Add some styling for the links */
  #home-link,
  #about-link {
    text-decoration: none;
    color: #fffbfb; /* Set the text color */
    padding: 8px 12px; /* Add some padding to the links */
    border-radius: 4px; /* Add rounded corners */
    font-weight: bold; /* Make the letters bold */
  }

  #home-link:hover,
  #about-link:hover {
    background-color: #007BFF; /* Change the background color on hover */
    color: #fff; /* Change the text color on hover */
  }
</style>