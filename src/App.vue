<template>
<div class="wrapper">
  <div class="wrapper-content">
    <section>
      <div class="container">

        <!-- first modal -->
        <button class="btn btnPrimary" @click="toggleModalFirst()">Show First Modal</button>
        <modal
          title="First Modal"
          v-show="modalFirst"
          @close="modalFirst = false">
          <div slot="body">
            <p>Text Text Text </p>
            <button class="btn btnPrimary" @click="toggleModalFirst()">Well done</button>
          </div>
        </modal>

        <!-- second modal -->
        <button class="btn btnPrimary" @click="toggleModalSecond()">Show modal with form</button>
        <modal 
          title="Modal with form"
          v-show="modalSecond.show"
          @close="modalSecond.show = false">
          <div slot="body">
            <form @submit.prevent="submitSecondForm">
              <label>Name:</label>
              <input type="text" required v-model="modalSecond.name">
               <label>Email:</label>
              <input type="email" required v-model="modalSecond.email">
              <button class="btn btnPrimary">Submit</button>
            </form>
          </div>
        </modal>

        <!-- modal with validate  -->
        <button class="btn btnPrimary" @click="toggleModalValidate()">Show modal with form + validate</button>
        <modalValidate v-show="modalValidate" @close="modalValidate = false" />

        <!-- Log In / Registration -->
        <button class="btn btnPrimary" @click="authModal = 'login'">Log in</button>
        <button class="btn btnPrimary" @click="authModal='registration'">Registration</button>
        <component 
          :title="setAuthModalTitle"
          :is="componentAuth" 
          @close="authModal = null"
          @openModal="toggleAuthModal($event)" />
      </div>
    </section>
  </div>
</div>
  
</template>

<script>
import modal from '@/components/UI/Modal.vue';
import modalValidate from '@/components/ModalValidate.vue';
import Login from '@/components/modals/auth/Login.vue';
import Registration from '@/components/modals/auth/Registration.vue';
export default {
  components: {modal, modalValidate, Login, Registration},
  data () {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: ''
      },
      modalValidate: false,
      authModal: null
    }
  },
  computed: {
    componentAuth(){
      if(this.authModal == 'login'){
        return 'Login'
      }
      else if (this.authModal == 'registration'){
        return 'Registration'
      }
    },
    setAuthModalTitle(){
      return this.authModal == 'login' ? 'Log in' : 'Registration'
    }
  },
  methods: {
    submitSecondForm(){
      console.log(
        {
          name: this.modalSecond.name,
          email: this.modalSecond.email
        }
      );
      this.modalSecond.name = '';
      this.modalSecond.email = '';
      this.modalSecond.show = false
    },
    toggleModalFirst(){
      this.modalFirst = !this.modalFirst
    },

    toggleModalSecond(){
      this.modalSecond.show = !this.modalSecond.show
    },

    toggleModalValidate(){
      this.modalValidate = !this.modalValidate
    },
    
    toggleAuthModal(modal){
      this.authModal = modal
    }
  }
}
</script>

<style>

</style>
