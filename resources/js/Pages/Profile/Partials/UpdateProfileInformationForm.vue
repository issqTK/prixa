<template>
    <section>
        <header>
            <h2 class="text-lg font-medium text-gray-900">{{__('Profile Information')}}</h2>
        </header>

        <form @submit.prevent="form.patch(route('profile.update'), { preserveScroll: true })" class="mt-6 space-y-6">
            <div>
                <InputLabel for="name" :value="__('Name')" />

                <TextInput
                    id="name"
                    type="text"
                    class="mt-1 block w-full"
                    v-model="form.name"
                    required
                    autofocus
                    autocomplete="name"
                />

                <InputError class="mt-2" :message="form.errors.name" />
            </div>

            <div>
                <InputLabel for="email" :value="__('Email')" />

                <TextInput
                    id="email"
                    type="email"
                    class="mt-1 block w-full"
                    v-model="form.email"
                    required
                    autocomplete="username"
                />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>
            
            <div>
                <InputLabel for="phone" :value="__('Phone')" />

                <TextInput
                    id="phone"
                    type="number"
                    class="mt-1 block w-full"
                    v-model="form.phone"
                    required
                />

                <InputError class="mt-2" :message="form.errors.phone" />
            </div>
            
            <div>
                <InputLabel for="whatsapp" :value="__('Whatsapp')" />

                <TextInput
                    id="whatsapp"
                    type="number"
                    class="mt-1 block w-full"
                    v-model="form.whatsapp"
                />

                <InputError class="mt-2" :message="form.errors.whatsapp" />
            </div>
            
            <div>
                <InputLabel for="shop" :value="__('Shop Address')" />

                <TextInput
                    id="shop"
                    type="url"
                    class="mt-1 block w-full"
                    v-model="form.store_address"
                />

                <InputError class="mt-2" :message="form.errors.store_address" />
            </div>

            <div v-if="mustVerifyEmail && user.email_verified_at === null">
                <p class="text-sm mt-2 text-gray-800">
                    {{ __('Your email address is unverified.') }}
                    <Link
                        :href="route('verification.send')"
                        method="post"
                        as="button"
                        class="underline text-sm text-gray-600 hover:text-gray-900 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                    >
                        {{ __('Click here to re-send the verification email.') }}
                    </Link>
                </p>

                <div
                    v-show="status === 'verification-link-sent'"
                    class="mt-2 font-medium text-sm text-green-600"
                >
                    {{__('A new verification link has been sent to your email address.')}}
                </div>
            </div>

            <div class="flex items-center gap-4">
                <PrimaryButton :disabled="form.processing">{{ __('save') }}</PrimaryButton>

                <Transition
                    enter-active-class="transition ease-in-out"
                    enter-from-class="opacity-0"
                    leave-active-class="transition ease-in-out"
                    leave-to-class="opacity-0"
                >
                    <p v-if="form.recentlySuccessful" class="text-sm text-gray-600">{{__('Saved.')}}</p>
                </Transition>
            </div>
        </form>
    </section>
</template>

<script setup>
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Link, useForm, usePage } from '@inertiajs/vue3';

defineProps({
    mustVerifyEmail: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const user = usePage().props.auth.user;

const form = useForm({
    name: user.name,
    email: user.email,
    phone: user.phone,
    whatsapp: user.whatsapp ??= '',
    store_address: user.store_address ??= '',
});
</script>