<template>
    <main class="auth-wrapper">
        <form class="auth-form" @submit.prevent="handleSubmit">
            <h1>
                <span>Task</span>
                <strong class="text-success">Nest</strong>
            </h1>
            <h2 class="h3 mb-4 fw-normal">Please Register</h2>
            <div class="form-floating mb-2">
                <input
                    type="text"
                    class="form-control"
                    :class="{ 'is-invalid': errors.name && errors.name[0] }"
                    id="name"
                    v-model="form.name"
                    placeholder="Your name"
                />
                <label for="name">Name</label>
                <div
                    class="invalid-feedback"
                    v-if="errors.name && errors.name[0]"
                >
                    {{ errors.name && errors.name[0] }}
                </div>
            </div>
            <div class="form-floating mb-2">
                <input
                    type="email"
                    class="form-control"
                    :class="{ 'is-invalid': errors.email && errors.email[0] }"
                    id="email"
                    v-model="form.email"
                    placeholder="name@example.com"
                />
                <label for="email">Email</label>
                <div
                    class="invalid-feedback"
                    v-if="errors.email && errors.email[0]"
                >
                    {{ errors.email && errors.email[0] }}
                </div>
            </div>
            <div class="form-floating mb-3">
                <input
                    type="password"
                    class="form-control"
                    :class="{
                        'is-invalid': errors.password && errors.password[0],
                    }"
                    id="password"
                    v-model="form.password"
                    placeholder="Password"
                />
                <label for="password">Password</label>
                <div
                    class="invalid-feedback"
                    v-if="errors.password && errors.password[0]"
                >
                    {{ errors.password && errors.password[0] }}
                </div>
            </div>

            <div class="form-floating mb-3">
                <input
                    type="password"
                    class="form-control"
                    id="password_confirmation"
                    v-model="form.password_confirmation"
                    placeholder="Password Confirmation"
                />
                <label for="password_confirmation">Password Confirmation</label>
            </div>

            <button class="w-100 btn btn-lg btn-success" type="submit">
                Register
            </button>
        </form>
    </main>
</template>

<script setup>
import { reactive } from 'vue';
import { storeToRefs } from 'pinia';
import { useRouter } from 'vue-router';
import { userAuthStore } from '../stores/auth';
const router = useRouter();
const store = userAuthStore();
const { isLoggedIn, errors } = storeToRefs(store);
const { handleRegister } = store;

const form = reactive({
    name: '',
    email: '',
    password: '',
    password_confirmation: '',
});

const handleSubmit = async () => {
    await handleRegister(form);
    console.log('Registered 🏄‍♀️');
    if (isLoggedIn.value) {
        console.log('Registered step 2 🚀');
        router.push({ name: 'tasks' });
    }
};
</script>

<style scoped>
.auth-wrapper {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    min-height: 60vh;
    margin-top: 2rem;
}

.auth-form {
    width: 400px;
}
</style>
