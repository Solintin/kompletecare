<template>
  <div class="mt-12">
    <div class="flex justify-end items-center space-x-10">
      <h3 class="text-lg font-bold tour">Take a tour</h3>
      <img src="notification.svg " alt="" />
      <img src="bell.svg " alt="" />
      <img src="user.svg " alt="" />
    </div>
    <div class="mt-10">
      <h1 class="text-2xl text-blue-600 font-medium">
        Update Patient Medical Record
      </h1>
      <p class="text-gray-500 mt-3">
        Click the tabs to view and edit patient medical details
      </p>

      <div class="mt-10 rounded-md shadow box mb-10 bg-white">
        <div
          class="text-center my-40 font-medium text-xl"
          v-if="$apollo.queries.investigation_types.loading"
        >
          Loading.....
        </div>
        <div v-else>
          <h1 class="text-blue-700 font-semibold text-xl">
            {{ typeA.title }}
          </h1>
          <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6 mt-6">
            <div
              class="flex items-center gap-6"
              v-for="item in typeA.investigations"
              :key="item.id"
            >
              <input
                @change="handleInvestigations($event, item.id)"
                type="checkbox"
                class="transform scale-125"
                :name="item.id"
              />
              <label :for="item.id" class="font-medium">{{ item.title }}</label>
            </div>
          </div>
          <hr class="bg-gray-300 w-full mt-4 mb-8" />

          <h1 class="text-blue-700 font-semibold text-xl">
            {{ typeB.title }}
          </h1>

          <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6 mt-6">
            <div
              class="flex items-center gap-6"
              v-for="item in typeB.investigations"
              :key="item.id"
            >
              <input
                @change="handleInvestigations($event, item.id)"
                type="checkbox"
                class="transform scale-125"
                :name="item.id"
              />
              <label :for="item.id" class="font-medium">{{ item.title }}</label>
            </div>
          </div>
          <hr class="bg-gray-300 w-full my-4" />

          <div class="grid grid-cols-2 gap-6 mt-10">
            <div class="flex flex-col">
              <label for="investigation" class="text-gray-500 font-medium"
                >CT Scan</label
              >
              <select
                @change="handleCTscan($event)"
                class="w-full bg-white shadow border border-gray-400 rounded outline-none p-3 my-2"
              >
                <option value="">*Specify</option>
                <option value="Scan needed in chest">
                  Scan needed in chest
                </option>
                <option value="Scan needed in toes">Scan needed in toes</option>
                <option value="Scan needed in pelvis">
                  Scan needed in pelvis
                </option>
                <option value="Scan needed in ankle">
                  Scan needed in ankle
                </option>
                <option value="Scan needed in finger">
                  Scan needed in finger
                </option>
              </select>
            </div>
            <div class="flex flex-col">
              <label for="investigation" class="text-gray-500 font-medium"
                >MRI</label
              >
              <select
                @change="handleMRI($event)"
                class="w-full bg-white shadow border border-gray-400 rounded outline-none p-3 my-2"
              >
                <option value="">*Specify</option>
                <option value="Full Body">Full Body</option>
                <option value="Half Body">Half Body</option>
              </select>
            </div>
          </div>

          <div class="flex justify-end">
            <button
              @click="handleSubmit"
              class="px-5 flex justify-center mt-5 outline-none max-w-max py-3 bg-blue-800 text-white text-lg rounded-md hover:bg-blue-900 transform-translate duration-500"
            >
              <span v-if="!loading">Save and Send</span>
              <div v-else class="flex justify-center items-center space-x-4">
                <div
                  class="h-6 w-6 rounded-full border-4 border-t-green-600 border-r-green-600 border-b-green-100 border-l-green-100 animate-spin"
                ></div>
                <span> Submitting </span>
              </div>
            </button>
          </div>
          <h1 v-if="error" class="text-right text-red-500 font-medium">
            Submission failed: incomplete form.
          </h1>
        </div>
        
      </div>
      <SuccessModal
        :openModal="openModal"
        :message="'Form Submitted Successfully'"
        :id="submissionId"
      />
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag'
export default {
  name: 'main-tab',
  data() {
    return {
      //   typeA: null,
      //   typeB: null,
      ctscan: '',
      investigations: [],
      mri: '',
      error: false,
      openModal: false,
      submissionId: null,
      loading: false,
      data: false,
    }
  },
  apollo: {
    investigation_types: gql`
      query fetchDatabase {
        investigation_types {
          investigations {
            id
            title
          }
          title
        }
      }
    `,
  },
  computed: {
    typeA() {
      return this.investigation_types.find(
        (item) => item.title.toLowerCase() === 'x-ray'
      )
    },
    typeB() {
      return this.investigation_types.find(
        (item) => item.title.toLowerCase() === 'ultrasound scan'
      )
    },
  },

  mounted() {
    // if (!this.$apollo.queries.investigation_types.loading) {
    //   this.typeA =
    //   this.typeB = this.investigation_types.find(
    //     (item) => item.title.toLowerCase() === 'ultrasound scan'
    //   )
    //   this.data = true
    //   console.log(this.investigation_types)
    //   console.log(this.data)
    //   console.log(this.$apollo)
    // }
  },

  methods: {
    handleCTscan(event) {
      this.ctscan = event.target.value
    },
    handleMRI(event) {
      this.mri = event.target.value
    },
    handleInvestigations(event, value) {
      if (event.target.checked) {
        //Check if test id exist in the previous investigation array
        let isExist = false
        this.investigations.forEach((element) => {
          if (element === parseInt(value)) {
            isExist = true
          }
        })
        if (!isExist) {
          this.investigations.push(parseInt(value))
        }
      } else {
        //logic that handles removal un-checking investigation
        //that has been checked before

        const index = this.investigations.findIndex(
          (item) => item === parseInt(value)
        )
        this.investigations.splice(index, 1)
      }
    },
    handleFormValidation() {
      if (
        this.investigations.length == 0 ||
        this.ctscan == '' ||
        this.mri == ''
      ) {
        return true
      } else {
        return false
      }
    },
    handleSubmit() {
      if (!this.handleFormValidation()) {
        this.loading = true

        this.$apollo
          .mutate({
            mutation: gql`
              mutation add_medical_record(
                $ctscan: String!
                $developer: String!
                $investigations: [ID!]!
                $mri: String!
              ) {
                add_medical_record(
                  ctscan: $ctscan
                  developer: $developer
                  investigations: $investigations
                  mri: $mri
                ) {
                  id
                }
              }
            `,
            variables: {
              investigations: this.investigations,
              ctscan: this.ctscan,
              mri: this.mri,
              developer: 'Developer',
            },
          })
          .then((res) => {
            this.openModal = true
            this.loading = false

            this.submissionId = res.data.add_medical_record.id
          })
          .catch((err) => {
            this.error = err.message
            this.loading = false
          })
      } else {
        this.error = true
      }
    },
  },
}
</script>

<style scoped>
.tour {
  color: #382f9099;
}
.box {
  padding: 45px 60px;
}
</style>
