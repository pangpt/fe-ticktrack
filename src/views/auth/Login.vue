<script setup>
// TODO: Import necessary dependencies
// Hint: You'll need to import from vue, pinia, and your auth store
import { ref } from 'vue';
import { useRouter } from 'vue-router'
import { useAuthStore } from '@/stores/auth';
import { storeToRefs } from 'pinia';

// TODO: Initialize auth store and get necessary refs
// Hint: Use useAuthStore() and storeToRefs()
const router = useRouter()
const authStore = useAuthStore();
const { loading, error } = storeToRefs(authStore);
const auth = authStore;

// TODO: Create form ref with login fields
// Hint: You'll need email and password
const form = ref({
    // Your form fields here
    email: null,
    password: null,
})

// TODO: Implement handleSubmit function
// Hint: This should call the login function from auth store
// and handle any errors
const handleSubmit = async () => {
    try {
        const user = await auth.login({
            email: form.value.email,
            password: form.value.password
        });

        // ✅ Pastikan user tidak undefined
        if (user?.role === 'admin') {
            router.push({ name: 'admin.dashboard' });
        } else {
            router.push({ name: 'app.dashboard' });
        }
    } catch (err) {
        console.error('Login gagal:', err);
    }
}



// TODO: Implement togglePassword function
// Hint: This should toggle password visibility
const togglePassword = () => {
    // Your code here
}
</script>

<template>
    <form class="space-y-6" @submit.prevent="handleSubmit">
        <!-- Email -->
        <div>
            <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
            <div class="mt-1 relative">
                <!-- TODO: Add v-model binding for email -->
                <input v-model="form.email" type="email" id="email" name="email" 
                    class="w-full px-4 py-2 border border-gray-200 rounded-lg text-sm focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500"
                    placeholder="nama@perusahaan.com" :class="{ 'border-red-500 ring-red-500' : error?.email }"> 
                <div class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
                    <i data-feather="mail" class="w-4 h-4 text-gray-400"></i>
                </div>

                <p class="mt-1 text-xs text-red-500" v-if="error?.email">
                    {{  error?.email?.join(', ')}}
                </p>
            </div>
        </div>

        <!-- Password -->
        <div>
            <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
            <div class="mt-1 relative">
                <!-- TODO: Add v-model binding for password -->
                <input v-model="form.password" type="password" id="password" name="password" required
                    class="w-full px-4 py-2 border border-gray-200 rounded-lg text-sm focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500"
                    placeholder="••••••••" :class="{ 'border-red-500 ring-red-500' : error?.password }">
                <div class="absolute inset-y-0 right-0 pr-3 flex items-center">
                    <!-- TODO: Add click handler for password toggle -->
                    <button type="button" class="text-gray-400 hover:text-gray-600 focus:outline-none">
                        <i data-feather="eye" class="w-4 h-4" id="password-toggle"></i>
                    </button>
                </div>

                <p class="mt-1 text-xs text-red-500" v-if="error?.password">
                    {{  error?.password?.join(', ')}}
                </p>
            </div>
        </div>

        <!-- Remember Me & Forgot Password -->
        <div class="flex items-center justify-between">
            <div class="flex items-center">
                <input type="checkbox" id="remember" name="remember"
                    class="h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300 rounded">
                <label for="remember" class="ml-2 block text-sm text-gray-700">Ingat saya</label>
            </div>
            <a href="#" class="text-sm text-blue-600 hover:text-blue-800">Lupa password?</a>
        </div>

        <!-- Submit Button -->
        <div>
            <!-- TODO: Add loading state to button -->
            <button type="submit"
                class="w-full flex justify-center py-2 px-4 border border-transparent rounded-lg shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
                <span v-if="!loading">
                    Masuk
                </span>
                <span v-else>
                    <i class="fas fa-spinner fa-spin"></i> Memproses...
                </span>
            </button>
        </div>
    </form>

    <!-- Divider -->
    <div class="mt-6">
        <div class="relative">
            <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-gray-200"></div>
            </div>
            <div class="relative flex justify-center text-sm">
                <span class="px-2 bg-white text-gray-500">Atau</span>
            </div>
        </div>
    </div>

    <!-- Register Link -->
    <div class="mt-6 text-center">
        <p class="text-sm text-gray-600">
            Belum punya akun?
            <RouterLink :to="{ name: 'register' }" class="font-medium text-blue-600 hover:text-blue-800">Daftar
                sekarang</RouterLink>
        </p>
    </div>
</template>
