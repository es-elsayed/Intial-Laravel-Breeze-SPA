<script setup>
import Checkbox from '@/admin/Components/Checkbox.vue';
import GuestLayout from '@/admin/Layouts/GuestLayout.vue';
import Error from '@/admin/Components/Form/Error.vue';
import Label from '@/admin/Components/Form/Label.vue';
import Button from '@/admin/Components/Base/Button.vue';
import Input from '@/admin/Components/Form/Input.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';

defineProps({
    canResetPassword: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const form = useForm({
    email: '',
    password: '',
    remember: false,
});

const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};
</script>

<template>
    <GuestLayout>

        <Head title="Log in" />

        <div v-if="status" class="mb-4 text-sm font-medium text-green-600">
            {{ status }}
        </div>

        <form @submit.prevent="submit">
            <div>
                <Label for="email" value="Email" />

                <Input id="email" type="email" class="block w-full mt-1" v-model="form.email" required autofocus
                    autocomplete="username" />

                <Error class="mt-2" :message="form.errors.email" />
            </div>

            <div class="mt-4">
                <Label for="password" value="Password" />

                <Input id="password" type="password" class="block w-full mt-1" v-model="form.password" required
                    autocomplete="current-password" />

                <Error class="mt-2" :message="form.errors.password" />
            </div>

            <div class="block mt-4">
                <label class="flex items-center">
                    <Checkbox name="remember" v-model:checked="form.remember" />
                    <span class="ml-2 text-sm text-gray-600">Remember me</span>
                </label>
            </div>

            <div class="flex items-center justify-end mt-4">
                <Link v-if="canResetPassword" :href="route('password.request')"
                    class="text-sm text-gray-600 underline rounded-md hover:text-gray-900 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                Forgot your password?
                </Link>
                <span>&#160; or &#160;</span>
                <Link :href="route('register')"
                    class="text-sm text-gray-600 underline rounded-md hover:text-gray-900 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                Register?
                </Link>

                <Button color="black" type="submit" class="ml-4" :class="{ 'opacity-25': form.processing }"
                    :disabled="form.processing">
                    Log in
                </Button>
            </div>
        </form>
    </GuestLayout>
</template>
