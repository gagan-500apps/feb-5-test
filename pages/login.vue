
 <template>
    
    <FormComponent
      :head="form.Form_Heading"
      :fields="form.Form_Fields"
      :button="form.Form_button"
      @form-submit="login"
    />
  </template>   

<script setup>
import { userfromlogin } from '~/stores/forms';
import {validation} from '../stores/validation'
import ustore from '@/stores'
const formStore = userfromlogin()
  const form = formStore.login_form[0];
// console.log('form',form);   

 const callValidation = validation();

const login = (formData) => {
    
       const userData = {
       Email: formData.Email,
       Password: formData.Password
       }
       callValidation.loginstoredata(userData)

        // Check login credentials
        const loggedInUser = callValidation.check();
  
  if (loggedInUser) {
    console.log('userData',userData);
      alert('Login successful');
     
      navigateTo(`/home`)
      
  } else {
      alert('Invalid email or password');
      
  }
}
 
</script>

