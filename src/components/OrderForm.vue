<template>
  <div>
    <h1>{{title}}</h1>
    <b-form @submit.prevent="submit">
      <b-row class="mb-2">
        <b-form-group class="col-12" label="Title">
          <b-form-input v-model="formItem.title"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Type">
          <b-form-select v-model="formItem.type">
            <option value="raw">Raw Leads</option>
            <option value="qualified">Qualified Leads</option>
          </b-form-select>
        </b-form-group>
        <b-form-group class="col-12" label="CPL">
          <b-form-input v-model="formItem.cpl" type="number"></b-form-input>
        </b-form-group>
      </b-row>
      <hr>
      <b-row class="mb-2">
        <b-form-group class="col-12" label="Daily Cap Monday">
          <b-form-input v-model="formItem.dc1" type="number"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Daily Cap Tuesday">
          <b-form-input v-model="formItem.dc2" type="number"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Daily Cap Wednesday">
          <b-form-input v-model="formItem.dc3" type="number"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Daily Cap Thursday">
          <b-form-input v-model="formItem.dc4" type="number"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Daily Cap Friday">
          <b-form-input v-model="formItem.dc5" type="number"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Daily Cap Saturday">
          <b-form-input v-model="formItem.dc6" type="number"></b-form-input>
        </b-form-group>
        <b-form-group class="col-12" label="Daily Cap Sunday">
          <b-form-input v-model="formItem.dc7" type="number"></b-form-input>
        </b-form-group>
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
import orders from '@/api/orders'
export default {
  name: 'OrderForm',
  computed: {
    title () {
      if (this.loading) {
        return 'Loading...'
      }
      return this.formItem.title
    }
  },
  data () {
    return {
      loading: true,
      formItem: {}
    }
  },
  created () {
    this.formItem = orders.getOrder(1)
    this.loading = false
  },
  methods: {
    submit () {
      alert('submitted')
    }
  }
}
</script>