<template>
  <div class="py-4">
    <h1>{{ title }}</h1>
    <b-form @submit.prevent="submit">
      <b-row>
        <b-col cols="12" lg="6">
          <div class="form-card">
            <h4>Basic Information</h4>
            <b-row>
              <b-col >
                
                <b-form-group label="Title" label-for="title">
                  <b-form-input
                    id="title"
                    v-model="$v.formItem.title.$model"
                    :state="validateState('title')"
                  ></b-form-input>
                  <b-form-invalid-feedback>Title is required</b-form-invalid-feedback>
                </b-form-group>
              </b-col> 
            </b-row>
            <b-row>
              <b-col>
                <b-form-group label="Type" label-for="type">
                  <b-form-select 
                    variant="primary" 
                    id="type"
                    v-model="$v.formItem.type.$model"
                    :options="[
                      { value: 'raw', text: 'Raw Leads' },
                      { value: 'qualified', text: 'Qualified Leads' }
                    ]"
                    :state="validateState('type')"
                  ></b-form-select>
                  <b-form-invalid-feedback>Type is required</b-form-invalid-feedback>
                </b-form-group>
              </b-col> 
              <b-col>
                <b-form-group label="Status" label-for="status">
                  <b-form-select
                    id="status"
                    v-model="$v.formItem.status.$model"
                    :options="[
                      { value: 'active', text: 'Active' },
                      { value: 'stopped', text: 'Stopped' }
                    ]"
                    :state="validateState('status')"
                  ></b-form-select>
                  <b-form-invalid-feedback>Status is required</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>  
            <b-row>
                <b-col>
                  <b-form-group label="Cost per Lead (CPL)" label-for="cpl">
                    <b-form-input
                      id="cpl"
                      v-model="$v.formItem.cpl.$model"
                      type="number"
                      step="0.01"
                      :state="validateState('cpl')"
                    ></b-form-input>
                    <b-form-invalid-feedback>CPL must be a positive number</b-form-invalid-feedback>
                  </b-form-group>
                </b-col>
                <b-col>
                  <b-form-group label="Priority" label-for="priority">
                    <b-form-input
                      id="priority"
                      v-model="$v.formItem.priority.$model"
                      type="number"
                      :state="validateState('priority')"
                    ></b-form-input>
                    <b-form-invalid-feedback>Priority must be an integer</b-form-invalid-feedback>
                  </b-form-group>
                </b-col>
            </b-row>
          </div>
        </b-col>
        <b-col cols="12" lg="6">
          <div class="form-card">
            <h4>Lead Specifications</h4>
            <b-row>
              <b-col>
                <b-form-group label="Monthly Cap" label-for="monthly_cap">
                  <b-form-input
                    id="monthly_cap"
                    v-model="$v.formItem.monthly_cap.$model"
                    type="number"
                    :state="validateState('monthly_cap')"
                  ></b-form-input>
                  <b-form-invalid-feedback>Must be -1 or a positive integer</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
              <b-col>
                <b-form-group label="Total Cap" label-for="total_cap">
                  <b-form-input
                    id="total_cap"
                    v-model="$v.formItem.total_cap.$model"
                    type="number"
                    :state="validateState('total_cap')"
                  ></b-form-input>
                  <b-form-invalid-feedback>Must be -1 or a positive integer</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <b-form-group label="Max Devalidation (%)" label-for="max_devalidation">
                  <b-form-input
                    id="max_devalidation"
                    v-model="$v.formItem.max_devalidation.$model"
                    type="number"
                    :state="validateState('max_devalidation')"
                  ></b-form-input>
                  <b-form-invalid-feedback>Max devalidation must be between 0 and 100</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
              <b-col>
                <b-form-group label="Max Age for Raw Leads (days)" label-for="leads_max_age_raw">
                  <b-form-input
                    id="leads_max_age_raw"
                    v-model="$v.formItem.leads_max_age_raw.$model"
                    type="number"
                    :state="validateState('leads_max_age_raw')"
                  ></b-form-input>
                  <b-form-invalid-feedback>Must be a non-negative integer</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col cols="6">
                <b-form-group label="Max Age for Qualified Leads (days)" label-for="leads_max_age_qualified">
                  <b-form-input
                    id="leads_max_age_qualified"
                    v-model="$v.formItem.leads_max_age_qualified.$model"
                    type="number"
                    :state="validateState('leads_max_age_qualified')"
                  ></b-form-input>
                  <b-form-invalid-feedback>Must be a non-negative integer</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="form-card">
            <h4>Time Settings</h4>
            <b-row>
              <b-col>
                <b-form-group label="Start Date" label-for="starts_at">
                  <b-form-datepicker
                    id="starts_at"
                    v-model="$v.formItem.starts_at.$model"
                    :state="validateState('starts_at')"
                  ></b-form-datepicker>
                  <b-form-invalid-feedback>Start date is required</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
              <b-col>
                <b-form-group label="End Date" label-for="ends_at">
                  <b-form-datepicker
                    id="ends_at"
                    v-model="$v.formItem.ends_at.$model"
                    :state="validateState('ends_at')"
                  ></b-form-datepicker>
                  <b-form-invalid-feedback>End date is required and must be after start date</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
              <b-col>
                <b-form-group label="Timezone" label-for="timezone">
                  <b-form-select
                    id="timezone"
                    v-model="$v.formItem.timezone.$model"
                    :options="timezoneOptions"
                    :state="validateState('timezone')"
                  ></b-form-select>
                  <b-form-invalid-feedback>Timezone is required</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col class="dailyCapsSection">
                <b-row>
                  <h5>Daily Caps</h5>
                  <b-col v-for="day in 7" :key="day" md="1">
                    <b-form-group :label="`${getDayName(day)}`" :label-for="`dc${day}`">
                      <b-form-input
                        :id="`dc${day}`"
                        v-model="$v.formItem[`dc${day}`].$model"
                        type="number"
                        :state="validateState(`dc${day}`)"
                      ></b-form-input>
                      <b-form-invalid-feedback>Must be a non-negative integer</b-form-invalid-feedback>
                    </b-form-group>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
          </div>
        </b-col>
      </b-row>
      <b-row >
        <b-col cols="12" lg="6" >
          <div class="form-card"> 
            <h4>Geographic Settings</h4>
            <b-row>
                <b-col>
                    <b-form-group label="Country" label-for="country">
                      <b-form-select
                        id="country"
                        v-model="$v.formItem.country.$model"
                        :options="countryOptions"
                        :state="validateState('country')"
                      ></b-form-select>
                      <b-form-invalid-feedback>Country is required</b-form-invalid-feedback>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-row>
                <b-col>
                  <b-form-group label="Regions" label-for="regions">
                    <b-form-tags
                      id="regions"
                      v-model="$v.formItem.regions.$model"
                      :state="validateState('regions')"
                    ></b-form-tags>
                    <b-form-invalid-feedback>At least one region is required</b-form-invalid-feedback>
                  </b-form-group>
                </b-col>
                <b-col>
                  <b-form-group label="Excluded Regions" label-for="excluded_regions">
                    <b-form-tags
                      id="excluded_regions"
                      v-model="formItem.excluded_regions"
                    ></b-form-tags>
                  </b-form-group>
                </b-col>
            </b-row>
          </div>
        </b-col>
        <b-col cols="12" lg="6">
          <div class="form-card"> 
            <h4>Delivery Settings</h4>
            <b-row>
              <b-col>
                <b-form-group label="Delivery Type" label-for="delivery_type">
                  <b-form-select
                    id="delivery_type"
                    v-model="$v.formItem.delivery_type.$model"
                    :options="[
                      { value: 'api', text: 'API' },
                      { value: 'email', text: 'Email' }
                    ]"
                    :state="validateState('delivery_type')"
                  ></b-form-select>
                  <b-form-invalid-feedback>Delivery type is required</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <b-form-group
                  label="Delivery Emails"
                  label-for="delivery_emails"
                  :disabled="formItem.delivery_type !== 'email'"
                >
                  <b-form-tags
                    id="delivery_emails"
                    v-model="$v.formItem.delivery_emails.$model"
                    :state="validateState('delivery_emails')"
                    :disabled="formItem.delivery_type !== 'email'"
                  ></b-form-tags>
                  <b-form-invalid-feedback>At least one email is required for email delivery</b-form-invalid-feedback>
                </b-form-group>
              </b-col>
            </b-row>
          </div>
        </b-col>
      </b-row>
      <b-row class="saveButtonNav bg-dark">
        <b-form-group class="col-12" >
          <b-button pill type="submit" size="lg" variant="light" :disabled="$v.$invalid">Save Order</b-button>
        </b-form-group>
      </b-row>
    </b-form>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minValue, between, integer,minLength, email } from 'vuelidate/lib/validators'
import orders from '@/api/orders'

export default {
  name: 'OrderForm',
  mixins: [validationMixin],
  data() {
    return {
      loading: true,
      formItem: {},
      countryOptions: [
        { value: 'es', text: 'Spain' },
        { value: 'fr', text: 'France' },
        { value: 'de', text: 'Germany' },
        // Adding countries
      ],
      timezoneOptions: [
        { value: 'Europe/Paris', text: 'Europe/Paris' },
        { value: 'Europe/Madrid', text: 'Europe/Madrid' },
        // Adding timezones
      ],
      typeOptions: [
        { value: 'raw', text: 'Raw Leads' },
        { value: 'qualified', text: 'Qualified Leads' }
      ]
    }
  },
  validations: {
    formItem: {
      title: { required },
      type: { required },
      cpl: { required, minValue: minValue(0) },
      max_devalidation: { required, between: between(0, 100) },
      priority: { required, integer },
      leads_max_age_raw: { required, minValue: minValue(0), integer },
      leads_max_age_qualified: { required, minValue: minValue(0), integer },
      country: { required },
      regions: { required, minLength: minLength(1) },
      status: { required },
      timezone: { required },
      starts_at: { required },
      ends_at: { required },
      dc1: { required, minValue: minValue(0), integer },
      dc2: { required, minValue: minValue(0), integer },
      dc3: { required, minValue: minValue(0), integer },
      dc4: { required, minValue: minValue(0), integer },
      dc5: { required, minValue: minValue(0), integer },
      dc6: { required, minValue: minValue(0), integer },
      dc7: { required, minValue: minValue(0), integer },
      monthly_cap: { required, minValue: minValue(-1) },
      total_cap: { required, minValue: minValue(-1) },
      delivery_type: { required },
      delivery_emails: {
        required: function(value) {
          return this.formItem.delivery_type !== 'email' || (Array.isArray(value) && value.length > 0)
        },
        $each: { email }
      }
    }
  },
  computed: {
    title() {
      return this.loading ? 'Loading...' : this.formItem.title
    }
  },
  created() {
    this.fetchOrder()
  },
  methods: {
    async fetchOrder() {
      try {
        this.formItem = await orders.getOrder(1)
      } catch (error) {
        console.error('Error fetching order:', error)
        // Handle error (e.g., show error message to user)
      } finally {
        this.loading = false
      }
    },
    validateState(name) {
      const { $dirty, $error } = this.$v.formItem[name]
      return $dirty ? !$error : null
    },
    submit() {
      this.$v.$touch()
      if (this.$v.$invalid) {
        alert('Please fix the form errors before submitting.')
      } else {
        // Submit the form data
        console.log('Form submitted:', this.formItem)
        alert('Form submitted successfully!')
      }
    },
    getDayName(day) {
      const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
      return days[day - 1]
    }
  }
}
</script>