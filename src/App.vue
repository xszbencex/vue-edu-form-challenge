<template>
  <div id="app" class="min-h-screen bg-gray-100 flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-md w-full space-y-8">
      <div>
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          Sign up for an account
        </h2>
      </div>
      <form class="mt-8 space-y-6" @submit.prevent="submitForm">
        <div class="flex flex-col space-y-3 rounded-md shadow-sm">
          <div>
            <label for="email-address" class="block text-sm font-medium text-gray-700">
              Email address
            </label>
            <input v-model="formData.email.value" @input="formData.email.dirty = true"
                   id="email-address" name="email" type="email" autocomplete="email"
                   class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                   placeholder="Email address" />
            <span v-if="formData.email.error && formData.email.dirty" class="error">{{ formData.email.error }}</span>
          </div>
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700">Full name</label>
            <input v-model="formData.name.value" @input="formData.name.dirty = true"
                   id="name" name="name" type="text" autocomplete="name"
                   class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                   placeholder="Full name" />
            <span v-if="formData.name.error && formData.name.dirty" class="error">{{ formData.name.error }}</span>
          </div>
          <div>
            <label for="password" class="block text-sm font-medium text-gray-700">
              Password
            </label>
            <input v-model="formData.password.value" @input="formData.password.dirty = true"
                   id="password" name="password" type="password" autocomplete="new-password"
                   class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                   placeholder="Password" />
            <span v-if="formData.password.error && formData.password.dirty" class="error">{{ formData.password.error }}</span>
          </div>
          <div>
            <label for="password-again" class="block text-sm font-medium text-gray-700">
              Password again
            </label>
            <input v-model="formData.confirmPassword.value" @input="formData.confirmPassword.dirty = true"
                   id="password-again" name="password-again" type="password" autocomplete="new-password"
                   class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                   placeholder="Password again" />
            <span v-if="formData.confirmPassword.error && formData.confirmPassword.dirty" class="error">{{ formData.confirmPassword.error }}</span>
          </div>
        </div>

        <div>
          <button type="submit"
                  :disabled="hasErrors"
                  class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                  :class="{'opacity-50 cursor-not-allowed': hasErrors}">
            Sign Up
          </button>
        </div>

        <div v-if="isSubmitted && !hasErrors" class="flex items-center flex-col justify-center">
          <p class="py-4">A regisztrációs adatok:</p>

          <p><strong>Név:</strong> {{ formValue.name }}</p>
          <p><strong>E-mail:</strong> {{ formValue.email }}</p>
          <p><strong>Jelszó:</strong> {{ formValue.password }}</p>

        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, reactive, ref, watch } from 'vue';

interface FormField<T = any> {
  value: T;
  dirty: boolean;
  error: string;
}

interface FormValue {
  name: string;
  email: string;
  password: string;
  confirmPassword: string;
}

interface FormData {
  name: FormField<string>;
  email: FormField<string>;
  password: FormField<string>;
  confirmPassword: FormField<string>;
}

const formData = reactive<FormData>({
  name: {value: '', dirty: false, error: ''},
  email: {value: '', dirty: false, error: ''},
  password: {value: '', dirty: false, error: ''},
  confirmPassword: {value: '', dirty: false, error: ''}
});

onMounted(() => {
  validateForm();
})

const isSubmitted = ref(false);

const hasErrors = computed(() => hasErrorUtil());
const formValue = computed<FormValue>(() => ({
  confirmPassword: formData.confirmPassword.value,
  email: formData.email.value,
  password: formData.password.value,
  name: formData.name.value
}));

watch(formValue,() => {
  validateForm();
});

const submitForm = () => {
  validateForm();
  if (!hasErrors.value) {
    isSubmitted.value = true;
  } else {
    console.log('Hibás adatok. Kérlek ellenőrizd a mezőket!');
  }
};

const validateForm = () => {
  formData.name.error = '';
  formData.email.error = '';
  formData.password.error = '';
  formData.confirmPassword.error = '';

  if (formData.name.value.length < 5 || formData.name.value.length > 250) {
    formData.name.error = 'A névnek 5 és 250 karakter között kell lennie.';
  }

  const emailPattern = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,})+$/;
  if (!emailPattern.test(formData.email.value)) {
    formData.email.error = 'Kérlek adj meg egy érvényes e-mail címet.';
  }

  if (formData.password.value.length < 8) {
    formData.password.error = 'A jelszónak legalább 8 karakterből kell állnia.';
  }

  if (formData.password.value !== formData.confirmPassword.value) {
    formData.confirmPassword.error = 'A jelszók nem egyeznek.';
  }
};

const hasErrorUtil = (): boolean => Object.values(formData).some((formField: FormField) => formField.error !== '');
</script>

<style>
.error {
  color: red;
}
</style>