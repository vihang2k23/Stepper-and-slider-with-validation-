<script lang="ts" setup>
import {
confirmedValidator,
emailValidator,
passwordValidator,
requiredValidator
} from "@validators";
import axios from "axios";
// import { ErrorMessage, Field, Form } from 'vee-validate';
import { ref } from "vue";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";
// import type { VForm } from "vuetify/components";

const form = ref();
const form1 = ref();
const form2 = ref();
const currentStep = ref(0);

// data fileds
const data = {
  name: ref(""),
  email: ref(""),
  password: ref(""),
  confirmPassword: ref(""),
  city: ref(""),
  country: ref(""),
  company: ref(""),
  textareaValue: ref(""),
  company1: ref(""),
};

// tabs details
const tabs = [
  {
    name: "Account",
    tab: 1,
    isActive: false,
  },
  {
    name: "Information",
    tab: 2,
    isActive: false,
  },
  {
    name: "Address",
    tab: 3,
    isActive: false,
  },
  {
    name: "Price",
    tab: 4,
    isActive: false,
  },
];

// Next button method
const nextStep = async () => {
  switch (currentStep.value) {
    case 0:
      let res = await form.value.validate();
      console.log(res);
      if (res.valid) {
        tabs[0].isActive = true;
        currentStep.value = 1;
        toast("First Step Completed !", {
          autoClose: 3000,
          pauseOnFocusLoss: true,
        });
      }
      break;
    case 1:
      let res1 = await form1.value.validate();
      if (res1.valid) {
        tabs[1].isActive = true;
        currentStep.value = 2;
        toast("Second Step Completed !", {
          autoClose: 3000,
          pauseOnFocusLoss: true,
        });
      }
      break;
    case 2:
      let res2 = await form2.value.validate();
      if (res2.valid) {
        tabs[2].isActive = true;
        currentStep.value = 3;
        toast("Third Step Completed !", {
          autoClose: 3000,
          pauseOnFocusLoss: true,
        });
        // Api calling using json server
        try {
          const response = await axios.post("http://localhost:3000/items", {
            name: data.name.value,
            email: data.email.value,
            password: data.password.value,
            confirmPassword: data.confirmPassword.value,
            city: data.city.value,
            country: data.country.value,
            company: data.company.value,
            textareaValue: data.textareaValue.value,
            company1: data.company1.value,
          });

          toast(response.status + response.statusText, {
            autoClose: 3000,
          });
        } catch (e) {
          toast(e.message);
          console.log(e.message, "hello");
        }
      }
      break;
    case 3:
      tabs[3].isActive = true;

      break;
    default:
      currentStep.value === 0;
  }
};

// card data for step 4
const pricingPlans = [
  {
    name: "Starter",
    tagLine: "A simple start for everyone",
    monthlyPrice: 49,
    yearlyPrice: 39,
    isPopular: false,
    current: true,
  },
  {
    name: "Professional",
    tagLine: "For small to medium businesses",
    monthlyPrice: 99,
    yearlyPrice: 79,
    isPopular: true,
    current: false,
  },
];

function prevStep() {
  if (currentStep.value >= 1) {
    currentStep.value -= 1;
  }
  return;
}
</script>
<template>
  <VCard>
    <div class="formclass">
      <VRow>
        <VCol v-for="(item, index) in tabs" :key="index" cols="6" md="3">
          <div v-if="currentStep === index">
            <span class="box">{{ item.tab }}</span><span class="boxtext">{{ item.name }} &gt;</span>
          </div>
          <div v-else>
            <span class="noboxshadow" :class="{ active: item.isActive }">
              {{ item.tab }}
            </span>
            <span class="noboxshadowtext">
              {{ item.name }}
              &gt;</span>
          </div>
        </VCol>
      </VRow>
      <br /><br />
      <VDivider></VDivider><br />

      <Form @submit="nextStep" keep-values Vslot="{ handleSubmit }">
        <template v-if="currentStep === 0">
          <VForm ref="form" @submit.prevent lazy-validation>
            <VRow>
              <VCol cols="12" md="6">
                <VTextField v-model="data.name.value" placeholder="Your Name" persistent-placeholder
                  :rules="[requiredValidator]" />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField v-model="data.email.value" placeholder="Your Email" persistent-placeholder
                  :rules="[requiredValidator, emailValidator]" />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField v-model="data.password.value" type="password" placeholder="Your Password"
                  persistent-placeholder :rules="[requiredValidator, passwordValidator]" autocomplete="on" />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField v-model="data.confirmPassword.value" type="password" placeholder="Confirm Password"
                  persistent-placeholder :rules="[
                    requiredValidator,
                    confirmedValidator(
                      data.confirmPassword.value,
                      data.password.value
                    ),
                  ]" autocomplete="on" />
              </VCol>
            </VRow>
          </VForm>
        </template>
        <template v-if="currentStep === 1">
          <VForm ref="form1" lazy-validation>
            <VRow>
              <VCol cols="12" md="6">
                <VTextField v-model="data.company.value" label="Company" placeholder="Company"
                  :rules="[requiredValidator]" />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField v-model="data.city.value" label="City" placeholder="City" :rules="[requiredValidator]" />
              </VCol>
              <VCol cols="12" md="6">
                <VTextField v-model="data.country.value" label="Country" placeholder="Country"
                  :rules="[requiredValidator]" />
              </VCol>
            </VRow>
          </VForm>
        </template>
        <template v-if="currentStep === 2">
          <VForm ref="form2" lazy-validation>
            <VRow>
              <VCol cols="12" md="6">
                <VTextField v-model="data.company1.value" label="Company1" placeholder="Company"
                  :rules="[requiredValidator]" />
              </VCol>
              <VCol cols="12" md="6">
                <VTextarea v-model="data.textareaValue.value" label="Address" rows="2" :rules="[requiredValidator]" />
              </VCol>
            </VRow>
          </VForm>
        </template>
        <VRow>
          <VCol cols="12" md="12">
            <template v-if="currentStep === 3">
              <VRow>
                <VCol cols="12" md="4" v-for="plan in pricingPlans" :key="plan">
                  <VCard flat border :class="
                    plan.isPopular ? 'border-error   border-opacity-100' : ''
                  ">
                    <VCardText style="height: 4.125rem" class="text-end">
                      <!-- :point_right: Popular -->
                      <VChip v-show="plan.isPopular" label color="primary" size="small">
                        Popular
                      </VChip>
                    </VCardText>
                    <!-- :point_right: Plan logo -->
                    <VCardText class="text-center">
                      <!-- :point_right: Plan name -->
                      <h5 class="text-h5 mb-2">
                        {{ plan.name }}
                      </h5>
                      <p class="mb-0">
                        {{ plan.tagLine }}
                      </p>
                    </VCardText>
                    <!-- :point_right: Plan price  -->
                    <VCardText class="position-relative text-center">
                      <div class="d-flex justify-center align-center">
                        <sup class="text-sm font-weight-medium me-1">$</sup>
                        {{ plan.monthlyPrice }}
                        <sub class="text-sm font-weight-medium ms-1 mt-4">/month</sub>
                      </div>
                      <!-- :point_right: Annual Price -->
                    </VCardText>
                    <!-- :point_right: Plan features -->
                    <VCardText class="mt-5">
                      <VList class="card-list">
                        <VListItem v-for="feature in plan.features" :key="feature">
                          <template #prepend>
                            <VIcon :size="14" icon="tabler-circle" class="me-3" />
                          </template>
                          <VListItemTitle>
                            {{ feature }}
                          </VListItemTitle>
                        </VListItem>
                      </VList>
                    </VCardText>
                    <!-- :point_right: Plan actions -->
                    <VCardActions>
                      <VBtn block :color="plan.current ? 'success' : 'error'"
                        :variant="plan.isPopular ? 'elevated' : 'tonal'">
                        {{
                          plan.yearlyPrice === 0
                          ? "Your Current Plan"
                          : "Upgrade"
                        }}
                      </VBtn>
                    </VCardActions>
                  </VCard>
                </VCol>
                <VCol cols="12" md="4">
                  <VCard flat border>
                    <VCardText style="height: 4.125rem" class="text-end">
                      <!-- :point_right: Popular -->
                    </VCardText>
                    <!-- :point_right: Plan logo -->
                    <VCardText class="text-center">
                      <!-- :point_right: Plan name -->
                      <h5 class="text-h5 mb-2">Custom</h5>
                      <p class="mb-0">Custom solution for big organizations</p>
                    </VCardText>
                    <!-- :point_right: Plan actions -->
                    <VCardActions>
                      <VBtn block> Request custom plan </VBtn>
                    </VCardActions>
                  </VCard>
                </VCol>
              </VRow>
            </template><br />
            <VBtn v-if="currentStep !== 0 && currentStep !== 3" type="button" :rounded="0" color="white" height="45px"
              class="previous" @click="prevStep">
              &lt; Previous
            </VBtn>
            <VBtn start :rounded="0" color="error" v-if="currentStep !== 3" @click="nextStep" type="button" height="45px"
              class="float-right">
              Next >
            </VBtn>
          </VCol>
        </VRow>
      </Form>
    </div>
  </VCard>
</template>
<style scoped>
.formclass {
  padding: 20px;
  margin: 20px;
}

.box {
  border: 1px solid red;
  background-color: red;
  box-shadow: 2px 2px 2px;
  margin-inline-end: 20px;
  padding-block: 8px 7px;
  padding-inline: 15px;
  text-align: center;
}

.noboxshadow {
  border: 1px solid #b8c2cc;
  background-color: #b8c2cc;
  margin-inline-end: 20px;
  padding-block: 8px 7px;
  padding-inline: 15px;
  text-align: center;
}

.right {
  float: inline-end;
}

.previous {
  border: 1px solid black;
}

.active {
  border: 1px solid #f7145414;
  background-color: #f7145414;
  color: red;
}

.boxtext {
  display: inline;
  color: red;
}

.noboxshadowtext {
  color: grey;
}
</style>
