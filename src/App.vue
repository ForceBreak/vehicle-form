<template>
  <div id="app">
    <div class="top-header">
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
    <b-container>
      <b-row class="mt-20">
        <b-col>
          <img alt="Vue logo" src="./assets/images/logo.png">
        </b-col>
      </b-row>

      <b-row class="mt-30">
        <b-col>
          <div class="d-flex flew-wrap align-items-center base-font-face breadcrumbs">
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
          <p class="base-font-face base-color mb-0">If you don’t know your vehicle information - <span class="link">look it up.</span></p>
        </b-col>
      </b-row>

      <b-row class="mt-30">
        <b-col>
          <b-form class="reg-form">
            <b-form-group
              id="vehicletype-group"
              label-for="license"
            >
              <b-form-input id="license" v-model="license"></b-form-input>
              <label for="license" :class="{ ['not-empty']: license.length }">License</label>
            </b-form-group>

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

          </b-form>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
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
      vehicleType: ''
    }
  }
}
</script>

<style lang="scss">
@import url('./assets/scss/general.scss');
@import "vue-select/src/scss/vue-select.scss"
</style>
