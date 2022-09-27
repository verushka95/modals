<template>
    <modal
        :title="title"
        @close="onClose">
        <div slot="body">
            <form @submit.prevent="onSubmit">

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

                <div class="form-text">
                    <span @click="openAnotherModal">I have an account</span>
                </div>

                <button class="btn btnPrimary">Submit</button>
            </form>
            
        </div>
    </modal>
</template>

<script>
import {required, email, minLength, sameAs} from 'vuelidate/lib/validators';
import modal from '@/components/UI/Modal.vue';
export default {
    components: {modal},
    props: {
        title: {
            type: String,
            required: true
        }
    },
    data(){
        return {
            email: '',
            password: '',
            repeatPassword: ''
        }
    },
    computed: {
        AreFieldsValid(){
            return (!this.$v.$invalid)
        }
    },
    validations: {
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(6)
        },
        repeatPassword: {
            sameAsPassword: sameAs('password')
        }
    },
    methods: {
        openAnotherModal(){
            this.$emit('close');
            this.$emit('openModal', 'login');
        },

        onClose(){
            this.clearFields();
            this.$emit('close');
        },

        onSubmit(){
            this.$v.$touch();
            if(this.AreFieldsValid){
                const user = {
                    email: this.email,
                    password: this.password
                }
                console.log(user)

                this.clearFields()
                this.$v.$reset()
                this.$emit('close')
            }
        },
        
        clearFields(){
           Object.assign(this.$data, this.$options.data.apply(this))
        }
    }
}
</script>

<style lang="scss">
    .form-text{
        margin-bottom: 20px;
        cursor: pointer;
        text-decoration: underline;
        font-size: 16px;
    }
</style>