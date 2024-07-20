<template>
  <div class="py-4">
    <h1>{{title}}</h1>
    <b-form @submit.prevent="submit">
      <b-row>
        <b-col class="form-card" >
          <h4>Basic Information</h4>
          <b-row>
            <b-col>
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
            <b-col>
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
            </b-col>
          </b-row>
        </b-col>
        <b-col class="form-card" >
          <h4>Lead Specifications</h4>
        </b-col>
      </b-row>
      <b-row class="mb-2">
        <b-form-group class="col-12">
          <b-button type="submit">Save</b-button>
        </b-form-group>
      </b-row>
    </b-form>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required,integer,minValue } from 'vuelidate/lib/validators'
import orders from '@/api/orders'

export default {
  name: 'OrderForm',
  mixins: [validationMixin],
  data () {
    return {
      loading: true,
      formItem: {}
    }
  },
  validations: {
    title: { required },
    type: { required },
    status: { required },
    cpl: { required, minValue: minValue(0) },
    priority: { required, integer }
  },
  created () {
    this.formItem = orders.getOrder(1)
    this.loading = false
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
      getDayName(day) {
        const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
        return days[day - 1]
      },
      submit () {
        alert('submitted')
      }
    }
  }

</script>