<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h2 class="title is-3 has-text-grey">
      Add Book Log 
    </h2>
          <router-view></router-view>
          <hr>
          <b-form >
            <b-field label="Select datetime start">
        <b-datetimepicker v-model="booklog.StartTime"
            placeholder="Click to select...">

            <template #left>
                <b-button
                    label="Now"
                    type="is-primary"
                    icon-left="clock"
                    @click="datetime = new Date()" />
            </template>

            <template #right>
                <b-button
                    label="Clear"
                    type="is-danger"
                    icon-left="close"
                    outlined
                    @click="datetime = null" />
            </template>
        </b-datetimepicker>
    </b-field>
    <b-field label="Select datetime End">
        <b-datetimepicker v-model="booklog.EndTime"
            placeholder="Click to select...">

            <template #left>
                <b-button
                    label="Now"
                    type="is-primary"
                    icon-left="clock"
                    @click="datetime = new Date()" />
            </template>

            <template #right>
                <b-button
                    label="Clear"
                    type="is-danger"
                    icon-left="close"
                    outlined
                    @click="datetime = null" />
            </template>
        </b-datetimepicker>
    </b-field>
            <b-form-group label="Book Id">
              <b-form-input type="text" v-model="booklog.BookId" :class="{ 'is-invalid': validation.BookId }"
                placeholder="enter Book Id">
              </b-form-input>
              <div v-if="validation.BookId" class="mt-2">
                <b-alert show variant="danger">{{ validation.BookId[0] }}</b-alert>
              </div>
            </b-form-group>
             <b-form-group label="Member Id">
              <b-form-input type="text" v-model="booklog.MemberId" :class="{ 'is-invalid': validation.MemberId }"
                placeholder="Enter Member Id">
              </b-form-input>
              <div v-if="validation.MemberId" class="mt-2">
                <b-alert show variant="danger">{{ validation.MemberId[0] }}</b-alert>
              </div>
            </b-form-group>
          <p class="content"><b>Status</b>: {{ booklog.Status }}</p>
           <b-form-group>
        <b-dropdown
            v-model="booklog.Status"
            aria-role="list">
            <template #trigger>
                <b-button
                    type="is-primary"
                    icon-right="menu-down">
                    Selected : {{ booklog.Status}}
                </b-button>
            </template>


            <b-dropdown-item value="alerted" aria-role="listitem">
                <span>alerted</span>
            </b-dropdown-item>

            <b-dropdown-item value="on process" aria-role="listitem">
                <span>on process</span>
            </b-dropdown-item>

            <b-dropdown-item value="done" aria-role="listitem">
                <span>done</span>
            </b-dropdown-item>
        </b-dropdown>
             </b-form-group>
            <b-button type="submit" variant="primary"  @click=Add()>ADD</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {
       name: 'createBooklog',
    data() {
      return {

        booklog: {
          StartTime: new Date(),
          EndTime: new Date(),
          BookId: '',
          MemberId: '',
          Status: '',
          
        },
    
        validation: []
      }
    },
    methods: {
      
      async Add() {
       
        
        await this.$axios.post('/items/booklog', {
          StartTime: this.booklog.StartTime,
          EndTime: this.booklog.EndTime,
          BookId: this.booklog.BookId,
          MemberId: this.booklog.MemberId,
          Status: this.booklog.Status,
            
          })
          .then(() => {
            window.location = 'http://localhost:3000/booklog'
          })
          .catch(error => {
      
            this.validation = error.response.data
          })
      }
    }
  }
</script>
<style scoped>
    .tag {
        cursor: pointer;
    }
</style>
