<template>
<section class="section">
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h2 class="title is-3 has-text-grey">
      Data Member 
    </h2>
          <hr>
          <b-button variant="primary" class="mb-3" @click="add()">ADD</b-button>
          <b-field grouped group-multiline>
            <b-select v-model="defaultSortDirection">
                <option value="asc">Default sort direction: ASC</option>
                <option value="desc">Default sort direction: DESC</option>
            </b-select>
            <b-select v-model="perPage" :disabled="!isPaginated">
                <option value="5">5 per page</option>
                <option value="10">10 per page</option>
                <option value="15">15 per page</option>
                <option value="20">20 per page</option>
            </b-select>
            <div class="control">
                <b-button
                    label="Set page to 2"
                    :disabled="!isPaginated"
                    @click="currentPage = 2" />
            </div>
            <div class="control is-flex">
                <b-switch v-model="isPaginated">Paginated</b-switch>
            </div>
            <div class="control is-flex">
                <b-switch v-model="isPaginationSimple" :disabled="!isPaginated">Simple pagination</b-switch>
            </div>
            <!--<div class="control is-flex">
                <b-switch v-model="isPaginationRounded" :disabled="!isPaginated">Rounded pagination</b-switch>
            </div>
            <b-select v-model="paginationPosition" :disabled="!isPaginated">
                <option value="bottom">bottom pagination</option>
                <option value="top">top pagination</option>
                <option value="both">both</option>
            </b-select>
            <b-select v-model="sortIcon">
                <option value="arrow-up">Arrow sort icon</option>
                <option value="menu-up">Caret sort icon</option>
                <option value="chevron-up">Chevron sort icon </option>
            </b-select>
            <b-select v-model="sortIconSize">
                <option value="is-small">Small sort icon</option>
                <option value="">Regular sort icon</option>
                <option value="is-medium">Medium sort icon</option>
                <option value="is-large">Large sort icon</option>
            </b-select>
            <b-select v-model="paginationOrder">
                <option value="">default pagination order</option>
                <option value="is-centered">is-centered pagination order</option>
                <option value="is-right">is-right pagination order</option>
            </b-select>
            <div class="control is-flex">
                <b-switch v-model="hasInput">Input</b-switch>
            </div>
            <b-select v-model="inputPosition">
                <option value="">default input position</option>
                <option value="is-input-right">is-input-right</option>
                <option value="is-input-left">is-input-left</option>
            </b-select>
             <b-input type="number" placeholder="debounce (milliseconds)" v-model="inputDebounce" min="0"></b-input>-->
        </b-field>

        <b-table
            :data="data"
            :paginated="isPaginated"
            :per-page="perPage"
            :current-page.sync="currentPage"
            :pagination-simple="isPaginationSimple"
            :pagination-position="paginationPosition"
            :default-sort-direction="defaultSortDirection"
            :pagination-rounded="isPaginationRounded"
            :sort-icon="sortIcon"
            :sort-icon-size="sortIconSize"
            default-sort="user.first_name"
            aria-next-label="Next page"
            aria-previous-label="Previous page"
            aria-page-label="Page"
            aria-current-label="Current page"
            :page-input="hasInput"
            :pagination-order="paginationOrder"
            :page-input-position="inputPosition"
            :debounce-page-input="inputDebounce">

            <b-table-column field="Id" label="ID" width="40" sortable numeric v-slot="props">
                {{ props.row.Id }}
            </b-table-column>

            <b-table-column field="Name" label="Name" sortable v-slot="props">
                {{ props.row.Name }}
            </b-table-column>

            <b-table-column field="Gender" label="Gender" sortable v-slot="props">
                {{ props.row.Gender }}
            </b-table-column>

            <b-table-column field="Phone" label="Phone" sortable centered v-slot="props">
                {{ props.row.Phone }}
            </b-table-column>
             <b-table-column field="Occupation" label="Occupation" sortable centered v-slot="props">
                {{ props.row.Occupation}}
            </b-table-column>
            <b-table-column field="Email" label="Email" sortable centered v-slot="props">
                {{ props.row.Email }}
            </b-table-column>

            <b-table-column label="Actions" v-slot="props" >
              <button class="button is-small is-light" @click="scrollToElement(props.row.Id)">
    <b-icon icon="update" size="is-small"></b-icon>
  </button>
  <button class="button is-small is-light" @click="deleteMember(props.row.Id)">
    <b-icon icon="delete" size="is-small"></b-icon>
  </button>
            </b-table-column>
        </b-table>
        </b-card>
      </b-col>
    </b-row>
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
           <h2 class="title is-3 has-text-grey">
      Edit Member 
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
            </b-form-group>
              <b-form-group label="Member Gender">
              <b-form-input type="text" v-model="member.Gender" :class="{ 'is-invalid': validation.Gender }"
                placeholder="Enter Member Gender">
              </b-form-input>
              <div v-if="validation.Gender" class="mt-2">
                <b-alert show variant="danger">{{ validation.Gender[0] }}</b-alert>
              </div>
            </b-form-group>
         
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
            <b-button type="submit" variant="primary" @click="edit()">UPDATE</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</section>
</template>

<script>
  export default {
    name: 'MemberPage',
    
    data() {
      return {
        
        data: [],
        isPaginated: true,
                isPaginationSimple: false,
                isPaginationRounded: false,
                paginationPosition: 'bottom',
                defaultSortDirection: 'asc',
                sortIcon: 'arrow-up',
                sortIconSize: 'is-small',
                currentPage: 1,
                perPage: 5,
                hasInput: false,
                paginationOrder: '',
                inputPosition: '',
                inputDebounce: '',
        member: {
          Id: '',
          Name: '',
          Gender: '',
          Phone: '',
          Occupation: '',
          Email: '',
        },
        
        validation: [],
      }
    },
    mounted() {
      this.onload()
    },
     methods: {
       onload(){
         this.$axios.get('/items/member')
        .then(response => {
          this.data = response.data.data

        })
        .catch(error => {
          console.log(error.response.data)
        })
       },
       async deleteMember(row) {
        
     
        await this.$axios.delete(`/items/member/${row}`)
          .then((res) => {
         
            const index = this.data.findIndex( item => item.Id === row );
            this.data.splice(index, 1) 
            this.$buefy.toast.open('Delete Member Success!')
            window.location = 'http://localhost:3000/Member'  
          })
      },
      add(){
        this.$router.push({
              name: 'createmember'
            })
      },
       scrollToElement(row) {
         
      this.$axios.get(`/items/member/${row}`)
        .then(response => {
            this.member.Id = response.data.data.Id;
            this.member.Name   = response.data.data.Name;
            this.member.Gender   = response.data.data.Gender;
            this.member.Phone   = response.data.data.Phone;
            this.member.Occupation   = response.data.data.Occupation;
            this.member.Email   = response.data.data.Email;
        })
         const el = this.$refs.scrollToMe;

    if (el) {
      // Use el.scrollIntoView() to instantly scroll to the element
      el.scrollIntoView({behavior: 'smooth'});
    }
       },
       async edit() {
        await this.$axios.patch('/items/member/'+this.member.Id, {
            Name: this.member.Name ,
            Gender: this.member.Gender ,
            Phone: this.member.Phone ,
            Occupation: this.member.Occupation,
            Email: this.member.Email,
          })
          .then((res) => {
            this.onload()
            this.$buefy.toast.open('Update Member Success!')
            window.location = 'http://localhost:3000/member'
            
          })
          .catch(error => {
      
            this.validation = error.response.data
          })
      }
    }

  }
</script>

