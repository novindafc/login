<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
            <h2 class="title is-3 has-text-grey">
      Add Book 
    </h2>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Book Title">
              <b-form-input type="text" v-model="book.Title" :class="{ 'is-invalid': validation.Title }"
                placeholder="Enter Book Title">
              </b-form-input>
              <div v-if="validation.Title" class="mt-2">
                <b-alert show variant="danger">{{ validation.Title[0] }}</b-alert>
              </div>
            </b-form-group>

             <b-form-group label="Book Author">
              <b-form-input type="text" v-model="book.Author" :class="{ 'is-invalid': validation.Author }"
                placeholder="Enter Book Author">
              </b-form-input>
              <div v-if="validation.Author" class="mt-2">
                <b-alert show variant="danger">{{ validation.Author[0] }}</b-alert>
              </div>
            </b-form-group>

             <b-form-group label="Book Position">
              <b-form-input type="text" v-model="book.Position" :class="{ 'is-invalid': validation.Position }"
                placeholder="Enter Book Position">
              </b-form-input>
              <div v-if="validation.Position" class="mt-2">
                <b-alert show variant="danger">{{ validation.Position[0] }}</b-alert>
              </div>
            </b-form-group>

             <b-form-group label="Book Qty">
              <b-form-input type="text" v-model="book.Qty" :class="{ 'is-invalid': validation.Qty }"
                placeholder="Enter Book Qty">
              </b-form-input>
              <div v-if="validation.Qty" class="mt-2">
                <b-alert show variant="danger">{{ validation.Qty[0] }}</b-alert>
              </div>
            </b-form-group>

             <b-form-group label="Book Remains">
              <b-form-input type="text" v-model="book.Remains" :class="{ 'is-invalid': validation.Remains }"
                placeholder="Enter Book Remains">
              </b-form-input>
              <div v-if="validation.Remains" class="mt-2">
                <b-alert show variant="danger">{{ validation.Remains[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary" @click="Add()">ADD</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {
       name: 'CreateBook',
    data() {
      return {

        book: {
          Title: '',
          Author: '',
          Position: '',
          Qty: '',
          Remains: '',
        },
    
        validation: []
      }
    },
    
    methods: {
      
      async Add() {
     
        
        await this.$axios.post('/items/book', {
        
          Title: this.book.Title,
          Author: this.book.Author,
          Position: this.book.Position,
          Qty: this.book.Qty,
          Remains: this.book.Remains,
            
          })
          .then(() => {
            this.$buefy.toast.open('Add Book Success!')
            window.location = 'http://localhost:3000/book'
          })
          .catch(error => {
      
            this.validation = error.response.data
          })
      
    }
  }}
</script>

