<script lang="ts">
import { Vueform, useVueform } from '@vueform/vueform'
import { onMounted, ref } from 'vue'

export default {
  mixins: [Vueform],
  setup(props, context) {
    const vueformSetup = useVueform(props, context)
    console.log(vueformSetup)

    const vueform = ref({})
    const loadingForm = ref(true)
    const form$ = ref(null)
    const formData = ref({})

    function handleSubmit(e) {
      console.log('form', e)
    }

    onMounted(() => {
      console.log('formData', form$.value?.data)
      setTimeout(() => {
        loadingForm.value = false
        vueform.value = {
          schema: {
            title: {
              type: 'static',
              label: 'Corrective Action Create'
            },
            projectId: {
              type: 'select',
              label: 'Project',
              required: true,
              addClass: 'w-full',
              items: [
                {
                  label: 'Project 1',
                  value: '1'
                },
                {
                  label: 'Project 2',
                  value: '2'
                },
                {
                  label: 'Project 3',
                  value: '3'
                }
              ]
            },
            group1: {
              type: 'group',
              label: '',
              schema: {
                company: {
                  columns: 6,
                  type: 'select',
                  label: 'Company',
                  items: [
                    {
                      label: 'Company 1',
                      value: '1'
                    },
                    {
                      label: 'Company 2',
                      value: '2'
                    },
                    {
                      label: 'Company 3',
                      value: '3'
                    }
                  ]
                },
                division: {
                  columns: 6,
                  type: 'select',
                  label: 'Division',
                  items: [
                    {
                      label: 'Division 1',
                      value: '1'
                    },
                    {
                      label: 'Division 2',
                      value: '2'
                    },
                    {
                      label: 'Division 3',
                      value: '3'
                    }
                  ]
                }
              }
            },
            deficiency: {
              type: 'select',
              label: 'Deficiency',
              required: true,
              items: [
                {
                  label: 'Deficiency 1',
                  value: '1'
                },
                {
                  label: 'Deficiency 2',
                  value: '2'
                },
                {
                  label: 'Deficiency 3',
                  value: '3'
                }
              ]
            },
            actionTaken: {
              type: 'textarea',
              label: 'Action Taken',
              required: true
            },
            deficiencyType: {
              type: 'select',
              label: 'Deficiency Type',
              required: true,
              items: [
                {
                  label: 'Deficiency Type 1',
                  value: '1'
                },
                {
                  label: 'Deficiency Type 2',
                  value: '2'
                },
                {
                  label: 'Deficiency Type 3',
                  value: '3'
                }
              ]
            },
            hazardSeverity: {
              type: 'radiogroup',
              label: 'Hazard Severity',
              required: true,
              view: 'tabs',
              items: [
                {
                  label: 'Low',
                  value: 'low'
                },
                {
                  label: 'Medium',
                  value: 'medium'
                },
                {
                  label: 'High',
                  value: 'high'
                }
              ]
            },
            photos: {
              type: 'file',
              label: 'Photos',
              drop: true,
              multiple: true,
              required: true,
              accept: 'image/*',
              view: 'image'
            },
            status: {
              type: 'radiogroup',
              label: 'Status',
              required: true,
              view: 'tabs',
              items: [
                {
                  label: 'Open',
                  value: 'open'
                },
                {
                  label: 'Assigned',
                  value: 'assigned'
                },
                {
                  label: 'Completed',
                  value: 'completed'
                }
              ]
            },

            submit: {
              type: 'button',
              buttonLabel: 'Create Corrective Action',
              submits: true,
              view: 'full',
              buttonClass: 'w-full mt-6 h-12 text-2xl font-bold'
            }
          }
        }
      }, 200)
    })

    return {
      ...vueformSetup,
      vueform,
      loadingForm,
      form$,
      formData,
      handleSubmit
    }
  }
}
</script>
<template>
  <h1 class="text-5xl text-center my-6 font-bold text-teal-800">
    Hello Vite + Vue 3 + Tailwind CSS
  </h1>
  <div
    class="h-full shadow-2xl rounded-lg w-screen max-w-6xl mx-auto border border-teal-700 p-6 flex flex-col gap-10 items-center justify-center"
  >
    <div v-if="loadingForm">Loading form</div>
    <Vueform
      class="w-full"
      :endpoint="
        async function (data, form$) {
          const formData = data as FormData // data returned by `formData` option
          console.log('formData', Object.fromEntries(formData.entries()))

          /* submit form data */
        }
      "
      v-model="formData"
      ref="form$"
      v-else
      :schema="vueform.schema"
    />
  </div>

  <div class="mx-auto max-w-md mt-10 border-dashed border-red-700 border-2 rounded-xl p-10">
    <h3 class="text-2xl font-bold text-red-700">Form Data</h3>
    <pre class="mt-4">{{ formData }}</pre>
  </div>
</template>
