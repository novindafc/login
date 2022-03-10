<template>
<section class="section">
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h2 class="title is-3 has-text-grey">
      Data Book
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
           <!-- <div class="control is-flex">
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

            <b-table-column field="Title" label="Title" sortable v-slot="props">
                {{ props.row.Title }}
            </b-table-column>

            <b-table-column field="Author" label="Author" sortable v-slot="props">
                {{ props.row.Author }}
            </b-table-column>

            <b-table-column field="Position" label="Position" sortable centered v-slot="props">
                {{ props.row.Position }}
            </b-table-column>
             <b-table-column field="Qty" label="Qty" sortable centered v-slot="props">
                {{ props.row.Qty }}
            </b-table-column>
            <b-table-column field="Remains" label="Remains" sortable centered v-slot="props">
                {{ props.row.Remains }}
            </b-table-column>

            <b-table-column label="Actions" v-slot="props" >
             <button class="button is-small is-light" @click="scrollToElement(props.row.Id)" >
    <b-icon icon="update" size="is-small"></b-icon>
  </button> 
  <button class="button is-small is-light" @click="deleteBook(props.row.Id)">
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
      Edit Book 
    </h2>
          <hr>
          <b-form >
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
    name: 'BookPage',
    
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
        book: {
          Id:'',
          Title: '',
          Author: '',
          Position: '',
          Qty: '',
          Remains: '',
        },
        
        validation: [],
      }
    },
    mounted() {
     this.onload()
    },
     methods: {
       onload(){
          this.$axios.get('/items/book')
        .then(response => {
          this.data = response.data.data

        })
        .catch(error => {
          console.log(error.response.data)
        })
       },
       async deleteBook(row) {    
        await this.$axios.delete(`/items/book/${row}`)
          .then((res) => {
         
            const index = this.data.findIndex( item => item.Id === row );
            this.data.splice(index, 1) 
            this.$buefy.toast.open('Delete Book Success!')
            window.location = 'http://localhost:3000/book'  
            
          })
      },
      add(){
        this.$router.push({
              name: 'createbook'
            })
      },
      scrollToElement(row) {
        this.$axios.get(`/items/book/${row}`)
        .then(response => {
            this.book.Id   = response.data.data.Id;
            this.book.Title   = response.data.data.Title;
            this.book.Author   = response.data.data.Author;
            this.book.Position   = response.data.data.Position;
            this.book.Qty   = response.data.data.Qty;
            this.book.Remains   = response.data.data.Remains;
            
        })

    const el = this.$refs.scrollToMe;

    if (el) {
      // Use el.scrollIntoView() to instantly scroll to the element
      el.scrollIntoView({behavior: 'smooth'});
    }
  },

  async edit(){
        await this.$axios.patch('/items/book/'+this.book.Id, {
          Title: this.book.Title,
          Author: this.book.Author,
          Position: this.book.Position,
          Qty: this.book.Qty,
          Remains: this.book.Remains,
          })
          .then((res) => {
            this.onload()
            this.$buefy.toast.open('Update Book Success!')
            window.location = 'http://localhost:3000/book'
          })
          .catch(error => {
      
            this.validation = error.response.data
          })
      
      },
    }

  }
</script>

