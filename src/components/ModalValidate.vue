<template>
    <modal 
        title="Modal with form"
        @close="onClose()">
        <div slot="body">
            <form @submit.prevent="onSubmit">
                <!-- name -->
                <div class="form-item" :class="{ 'errorInput': $v.name.$error}">
                    <label>Name:</label>
                    <p class="errorText" v-if="!$v.name.required && $v.name.$error">Field is required</p>
                    <p class="errorText" v-if="!$v.name.minLength && $v.name.$error">Name must have at least {{ $v.name.$params.minLength.min }}</p>
                    <input 
                        v-model="name"
                        :class="{ 'error': $v.name.$error}"
                        @change="$v.name.$touch()">
                </div>
                <!-- email -->
                <div class="form-item" :class="{ 'errorInput': $v.email.$error}">
                    <label>Email:</label>
                    <p class="errorText" v-if="!$v.email.required && $v.email.$error">Field is required</p>
                    <p class="errorText" v-if="!$v.email.email && $v.email.$error">Email is not correct</p>
                    <input 
                        v-model="email"
                        :class="{ 'error': $v.email.$error}"
                        @change="$v.email.$touch()">
                </div>

                <!-- password -->
                <div class="form-item" :class="{ 'errorInput': $v.password.$error}">
                    <label>Password:</label>
                    <p class="errorText" v-if="!$v.password.required && $v.password.$error">Field is required</p>
                    <p class="errorText" v-if="!$v.password.minLength && $v.password.$error">Name must have at least {{ $v.password.$params.minLength.min }}</p>
                    <input 
                        v-model="password"
                        :class="{ 'error': $v.password.$error}"
                        @change="$v.password.$touch()">
                </div>

                <!-- repeat password -->
                <div class="form-item" :class="{ 'errorInput': $v.repeatPassword.$error}">
                    <label>Repeat Password:</label>
                    <p class="errorText" v-if="!$v.repeatPassword.required && $v.repeatPassword.$error">Field is required</p>
                    <p class="errorText" v-if="!$v.repeatPassword.minLength && $v.repeatPassword.$error">Password is not correct</p>
                    <input 
                        v-model="repeatPassword"
                        :class="{ 'error': $v.repeatPassword.$error}"
                        @change="$v.repeatPassword.$touch()">
                </div>

                <button class="btn btnPrimary">Submit</button>
            </form>
            
        </div>
    </modal>
</template>
<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
import modal from '@/components/UI/Modal.vue';
export default {
    components: {modal},
    data(){
        return {
            name: '',
            email: '',
            password: '',
            repeatPassword: '',
        }
    },
    validations: {
        name: {
            required,
            minLength: minLength(4)
        },
        email: {
            required,
            email
        },
        password: {
            required, 
            minLength: minLength(8)
        },
        repeatPassword: {
            sameAsPassword: sameAs('password')
        }
    },
    methods:{
        onSubmit(){
            this.$v.$touch()
            if(!this.$v.$invalid){
                const user = {
                    name: this.name,
                    email: this.email,
                    password: this.password
                }
                console.log(user)

                this.clearFields()
                this.$v.$reset()
                this.$emit('close')
            }
        },
        onClose(){
            this.clearFields()
            this.$emit('close');
        },
        clearFields(){
           Object.assign(this.$data, this.$options.data.apply(this))
        }
    }
}
</script>

<style lang="scss">
    .form-item{
        .errorText{
            margin-bottom: 8px;
            font-size: 13px;
            color: red
        }
    }
    input.error{
        border-color: red;
    }
</style>