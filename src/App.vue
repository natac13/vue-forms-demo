<script lang="ts">
import { Vueform, useVueform } from "@vueform/vueform";
import { onMounted, ref } from "vue";

export default {
  mixins: [Vueform],
  setup(props, context) {
    const vueformSetup = useVueform(props, context);
    console.log(vueformSetup);

    const vueform = ref({});
    const loadingForm = ref(true);
    const form$ = ref(null);
    const formData = ref({});

    function handleSubmit(e) {
      console.log("form", e);
    }

    onMounted(() => {
      console.log("formData", form$.value?.data);
      setTimeout(() => {
        loadingForm.value = false;
        vueform.value = {
          schema: {
            email: {
              type: "text",
              label: "Email Address",
              inputType: "email",
              required: true,
            },
            password: {
              type: "text",
              label: "Password",
              inputType: "password",
            },
            rememberMe: {
              type: "toggle",
              label: "Remember me",
            },
            submit: {
              type: "button",
              buttonLabel: "Login",
              submits: true,
              addClass: "mt-4",
            },
          },
        };
      }, 1200);
    });

    return {
      ...vueformSetup,
      vueform,
      loadingForm,
      form$,
      formData,
      handleSubmit,
    };
  },
};
</script>
<template>
  <h1 class="text-5xl text-center my-6 font-bold text-teal-800">
    Hello Vite + Vue 3 + Tailwind CSS
  </h1>
  <div
    class="h-full shadow-2xl rounded-lg w-screen max-w-3xl mx-auto border border-teal-700 p-6 flex flex-col gap-10 items-center justify-center"
  >
    <div v-if="loadingForm">Loading form</div>
    <Vueform
      :endpoint="
        async function (data, form$) {
          const formData = data as FormData; // data returned by `formData` option
          console.log('formData', Object.fromEntries(formData.entries()));

          /* submit form data */
        }
      "
      v-model="formData"
      ref="form$"
      v-else
      :schema="vueform.schema"
    />
  </div>

  <div
    class="mx-auto max-w-md mt-10 border-dashed border-red-700 border-2 rounded-xl p-10"
  >
    <h3 class="text-2xl font-bold text-red-700">Form Data</h3>
    <pre class="mt-4">{{ formData }}</pre>
  </div>
</template>
