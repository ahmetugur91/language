<template>
    <div class="login row justify-content-center">
        <div class="col-md-4">
            <div class="card">
                <div class="card-header">Register</div>
                <div class="card-body">

                    <Validation :errors="validationErrors" v-if="validationErrors"></Validation>

                    <form @submit.prevent="registerForm">
                        <div class="form-group row">
                            <label for="name">Name:</label>
                            <input type="text" v-model="form.name" class="form-control" placeholder="Name">
                        </div>
                        <div class="form-group row">
                            <label for="email">Email:</label>
                            <input type="email" v-model="form.email" class="form-control" placeholder="Email Address">
                        </div>
                        <div class="form-group row">
                            <label for="password">Password:</label>
                            <input type="password" v-model="form.password" class="form-control" placeholder="Password">
                        </div>
                        <div class="form-group row">
                            <input type="submit" value="Login">
                        </div>
                        <div class="form-group row" v-if="authError">

                            <p class="error">
                                {{ authError }}
                            </p>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {register} from '../../helpers/auth';
    import Validation from '../Validation.vue';

    export default {
        name: "register",
        components: {
            Validation
        },
        data() {
            return {
                form: {
                    name: '',
                    email: '',
                    password: ''
                },
                error: null,
                validationErrors: ''
            };
        },
        methods: {
            registerForm() {
                this.$store.dispatch('register');

                register(this.$data.form)
                    .then((res) => {
                        this.$store.commit("registerSuccess", res);
                        this.$router.push({path: '/'});
                    })
                    .catch((error) => {
                        console.log(error);

                        this.validationErrors = error;

                        //this.$store.commit("registerFailed", {error});
                    });
            }
        },
        computed: {
            authError() {
                return this.$store.getters.authError;
            }
        }
    }
</script>

<style scoped>
.error {
    text-align: center;
    color: red;
}
</style>

