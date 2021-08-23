<template>
  <div id="app">
    <div class="top-bottom-bar header">
      <b-container>
        <b-row>
          <b-col>
            <span class="base-font-face">
              <span class="bold">vehiclerenew.com</span> is a privately owned website that is not affiliated with any government agencies.
            </span>
          </b-col>
        </b-row>
      </b-container>
    </div>
    <b-container class="main">
      <b-row class="mt-20">
        <b-col>
          <img alt="Vue logo" src="./assets/images/logo.png">
        </b-col>
      </b-row>

      <b-row class="mt-30">
        <b-col>
          <div class="d-flex flex-wrap align-items-center base-font-face breadcrumbs">
            <span 
              v-for="(item, index) in breadcrumbs"
              :key="item"
              class="d-flex align-items-center"
              :class="{ ['breadcrumbs-active']: index == 0 }"
            >
              <span>{{ item }}</span> 
              <b-icon 
                v-if="index != breadcrumbs.length - 1" 
                icon="chevron-right" font-scale="0.9" class="icon"
              />
            </span>
          </div>
        </b-col>
      </b-row>

      <b-row class="mt-40">
        <b-col>
          <h1 class="base-font-face black">Renew your vehicle online</h1>
          <p class="base-font-face base-color mb-0">If you don’t know your vehicle information - <a href="#" class="link">look it up.</a></p>
        </b-col>
      </b-row>

      <b-row class="mt-30">
        <b-col md="5">
          <b-form class="reg-form">
            <b-form-group
              id="vehicletype-group"
              label-for="license"
              class="inputGroup"
            >
              <b-form-input id="license" v-model="license"></b-form-input>
              <label for="license" :class="{ ['not-empty']: license.length }">License plate number</label>
            </b-form-group>

            <div class="inputGroup">
              <div class="select">
                <v-select 
                  v-model="vehicleType"
                  :options="vehicleTypes"
                  :label="'title'"
                  :clearable="false"
                  :reduce="option => option.title"
                >
                  <template #option="{ title, text }">
                    <p class="select-title mb-0">{{ title }}</p>
                    <p class="select-text mb-0">{{ text }}</p>
                    <hr>
                  </template>

                  <template #open-indicator="{ attributes }">
                    <span v-bind="attributes">
                      <b-icon icon="caret-down-fill"/>
                    </span>
                  </template>
                </v-select>
                <label :class="{ ['not-empty']: vehicleType.length }">Select vehicle type</label>
              </div>
              <transition-expand>
                <b-form-group 
                  v-if="vehicleType == vehicleTypes[2].title"
                  v-slot="{ ariaDescribedby }" 
                  class="vehicleTypeSub-wrap mb-2"
                >
                  <b-form-radio 
                    v-model="vehicleTypeSub" 
                    :aria-describedby="ariaDescribedby" 
                    name="some-radios" 
                    value="interstate"
                    class="vehicleTypeSub"
                    :class="{ coloredGroup: vehicleTypeSub == 'interstate' }"
                  >
                    This vehicle is used for interstate commerce and I certify that this vehicle has a curently valid certification of safety inspection.
                  </b-form-radio>
                  <b-form-radio 
                    v-model="vehicleTypeSub" 
                    :aria-describedby="ariaDescribedby" 
                    name="some-radios" 
                    value="not-interstate"
                    class="vehicleTypeSub"
                    :class="{ coloredGroup: vehicleTypeSub == 'not-interstate' }"
                  >
                    This vehicle is not used fo interstate commerce.
                  </b-form-radio>
                </b-form-group>
              </transition-expand>
            </div>

            <b-form-group
              id="vehiclemake-group"
              label-for="vehiclemake"
              class="inputGroup"
            >
              <b-form-input id="vehiclemake" v-model="vehicleMake"></b-form-input>
              <label for="vehiclemake" :class="{ ['not-empty']: vehicleMake.length }">Vehicle make</label>
            </b-form-group>

            <b-form-group
              id="vehicleyear-group"
              label-for="vehicleyear"
              class="inputGroup"
            >
              <b-form-input id="vehicleyear" v-model="vehicleYear" @blur="showProvider"></b-form-input>
              <label for="vehicleyear" :class="{ ['not-empty']: vehicleYear.length }">Vehicle year</label>
            </b-form-group>

            <transition-expand>
              <div v-if="isShowProvider" class="select inputGroup">
                <v-select 
                  v-model="insuranceProvider"
                  :options="insuranceProviderTypes"
                  :label="'title'"
                  :clearable="false"
                  :reduce="option => option.title"
                  @input="showPolicy"
                >
                  <template #option="{ title, text }">
                    <p class="select-title mb-0">{{ title }}</p>
                    <p class="select-text mb-0">{{ text }}</p>
                    <hr>
                  </template>

                  <template #open-indicator="{ attributes }">
                    <span v-bind="attributes">
                      <b-icon icon="caret-down-fill"/>
                    </span>
                  </template>
                </v-select>
                <label :class="{ ['not-empty']: insuranceProvider.length }">Insurance provider</label>
              </div>
            </transition-expand>

            <transition-expand>
              <b-form-group
                v-if="isShowPolicy"
                id="policynumber-group"
                label-for="policynumber"
                class="inputGroup"
              >
                <b-form-input id="policynumber" v-model="policyNumber" @blur="showAddMore"></b-form-input>
                <label for="policynumber" :class="{ ['not-empty']: policyNumber.length }">Policy number</label>
              </b-form-group>
            </transition-expand>
            <transition-expand>
              <div 
                v-if="isShowAddMore"
                class="addMore-wrap d-flex align-items-center"
              >
                <b-icon icon="plus" font-scale="1" class="icon" />
                <a href="#" class="link">Add another vehicle to this renewal</a>
              </div>
            </transition-expand>

            <div class="d-flex justify-content-end">
              <b-button squared variant="success" class="d-flex align-items-center continue">
                <span class="mr-1">Continue</span>
                <b-icon icon="chevron-right" font-scale="0.85" class="icon"/>
              </b-button>
            </div>
          </b-form>
        </b-col>
      </b-row>
    </b-container>
    <div class="top-bottom-bar footer">
      <b-container>
        <b-row>
          <b-col sm="2" md="2">
            <span class="base-font-face">
              Terms & Conditions
            </span>
          </b-col>
          <b-col sm="2" md="2">
            <span class="base-font-face">
              Privacy Policy
            </span>
          </b-col>
        </b-row>
      </b-container>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      isShowAddMore: false,
      isShowPolicy: false,
      isShowProvider: false,
      breadcrumbs: [ 'Vehicle', 'Renewal type', 'Owner', 'Lorem ipsum', 'Priority', 'Payment' ],
      vehicleTypes: [
        {
          title: 'Car or Sedan',
          text: 'Sedans, small cars and station wagons.'
        },
        {
          title: 'Small SUV or Truck',
          text: 'Most small SUVs, most small pickups.'
        },
        {
          title: 'Large SUV or Truck',
          text: 'All minivans, "compact" SUVs (e.g., Ford Explorer), most Dakota and T100 pickups.'
        },
        {
          title: 'Motorcycle',
          text: 'Any vehicle with less than four wheels.'
        },
        {
          title: 'Boat or Vessel',
          text: 'A boat is a watercraft of a large range of types and sizes.'
        },
        {
          title: 'Trailer',
          text: 'A trailer is a wheeled vehicle that can`t move on its own — it needs to be pulled by a car, truck, or other vehicle.'
        },
        {
          title: 'RV or Motorhome',
          text: 'A recreational vehicle, often abbreviated as RV, is a motor vehicle or trailer which includes living quarters designed for accommodation.'
        }
      ],
      license: '',
      vehicleType: '',
      vehicleTypeSub: '',
      vehicleMake: '',
      vehicleYear: '',
      insuranceProvider: '',
      insuranceProviderTypes: [
        {
          title: 'All Lines Insurance Agency',
          text: 'Lorem Ipsum text'
        },
      ],
      policyNumber: ''
    }
  },
  methods: {
    showProvider(){
      this.isShowProvider = Boolean(this.vehicleYear.length)
      if(!this.isShowProvider) this.insuranceProvider = ''
    },
    showPolicy(){
      this.isShowPolicy = Boolean(this.insuranceProvider.length)
    },
    showAddMore(){
      this.isShowAddMore = Boolean(this.policyNumber.length)
    }
  }
}
</script>

<style lang="scss">
@import url('./assets/scss/general.scss');
@import "vue-select/src/scss/vue-select.scss";
</style>
