<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
            <h2 class="title is-3 has-text-grey">
      Add Member 
    </h2>
          <hr>
          <b-form >
            <b-form-group label="Member Name">
              <b-form-input type="text" v-model="member.Name" :class="{ 'is-invalid': validation.Name }"
                placeholder="Enter Member Name">
              </b-form-input>
              <div v-if="validation.Name" class="mt-2">
                <b-alert show variant="danger">{{ validation.Name[0] }}</b-alert>
              </div>
            <p class="content"><b>Gender</b>: {{ member.Gender }}</p>
            </b-form-group >
        <b-dropdown
            v-model="member.Gender"
            aria-role="list">
            <template #trigger>
                <b-button
                    type="is-primary"
                    icon-right="menu-down">
                    Selected : {{ member.Gender}}
                </b-button>
            </template>


            <b-dropdown-item value="M" aria-role="listitem">
                <span>Male</span>
            </b-dropdown-item>

            <b-dropdown-item value="F" aria-role="listitem">
                <span>Female</span>
            </b-dropdown-item>
        </b-dropdown>
         
              <b-form-group label="Member Phone">
              <b-form-input type="text" v-model="member.Phone" :class="{ 'is-invalid': validation.Phone }"
                placeholder="Enter Member Phone">
              </b-form-input>
              <div v-if="validation.Phone" class="mt-2">
                <b-alert show variant="danger">{{ validation.Phone[0] }}</b-alert>
              </div>
            </b-form-group>
            
              <b-form-group label="Member Occupation">
              <b-form-input type="text" v-model="member.Occupation" :class="{ 'is-invalid': validation.Occupation }"
                placeholder="Enter Member Occupation">
              </b-form-input>
              <div v-if="validation.Occupation" class="mt-2">
                <b-alert show variant="danger">{{ validation.Occupation[0] }}</b-alert>
              </div>
            </b-form-group>

             <b-form-group label="Member Email">
              <b-form-input type="text" v-model="member.Email" :class="{ 'is-invalid': validation.Email }"
                placeholder="Enter Member Occupation">
              </b-form-input>
              <div v-if="validation.Email" class="mt-2">
                <b-alert show variant="danger">{{ validation.Email[0] }}</b-alert>
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
       name: 'CreateMember',
    data() {
      return {

        member: {
          Name: '',
          Gender: '',
          Phone: '',
          Occupation: '',
          Email: '',
        },
    
        validation: []
      }
    },
    methods: {
      
    async Add(){
        
        await this.$axios.post('/items/member', {
        
            Name: this.member.Name ,
            Gender: this.member.Gender ,
            Phone: this.member.Phone ,
            Occupation: this.member.Occupation,
            Email: this.member.Email,
            
          })
          .then(() => {
            
        
             window.location = 'http://localhost:3000/member'
          })
          .catch(error => {
      
            this.validation = error.response.data
          })
      }
    }
  }
</script>

