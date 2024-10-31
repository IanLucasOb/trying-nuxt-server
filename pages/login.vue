<template>
  <div class="flex justify-center">
    <div
      class="relative flex flex-col bg-slate-900 shadow-sm w-96 rounded-lg my-6"
    >
      <div
        class="relative m-2 items-center flex justify-center text-white h-16 rounded-md bg-slate-800"
      >
        <h3 class="text-2xl">Sign In</h3>
      </div>

      <div class="m-4">
        <UForm :schema="schema" :state="form" class="space-y-4" @submit="login">
          <UFormGroup label="Email" required>
            <UInput
              v-model="form.email"
              name="email"
              placeholder="you@example.com"
              icon="i-heroicons-envelope"
            />
          </UFormGroup>

          <UFormGroup class="mt-3" label="Password" name="password" required>
            <UInput
              v-model="form.password"
              type="password"
              icon="i-material-symbols:lock"
            />
          </UFormGroup>

          <UButton class="mt-6" type="submit" size="md">Submit</UButton>
        </UForm>
      </div>
    </div>
  </div>

  <!-- <h1 class="text-center">Deveria ter um login aqui vlw</h1>

  <UFormGroup label="Email" required>
    <UInput placeholder="you@example.com" icon="i-heroicons-envelope" />
  </UFormGroup>
  
  <UFormGroup label="Password" required>
    <UInput  icon="i-material-symbols:lock" />
  </UFormGroup> -->
</template>

<script setup lang="ts">
import Joi from "joi";
import type { FormSubmitEvent } from "#ui/types";

const schema = Joi.object({
  email: Joi.string().required().max(100),
  password: Joi.string().required().min(8),
});

const form = reactive({
  email: "",
  password: "",
});

const supabase = useSupabaseClient();
async function login(event: FormSubmitEvent<any>) {
  const result = await supabase.auth.signInWithPassword({
    email: event.data.email,
    password: event.data.password,
  });

  if (result.error?.code === "invalid_credentials") {
    alert("Invalid credentials, please try again");
  }

  navigateTo("/todo");
}

onMounted(() => {
  supabase.auth.signOut();
});
</script>
