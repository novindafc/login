<template>
<section class="section">
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h2 class="title is-3 has-text-grey">
      Data Book Log 
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

            <b-table-column field="StartTime" label="Start Time" sortable v-slot="props">
                {{ props.row.StartTime }}
            </b-table-column>

            <b-table-column field="EndTime" label="End Time" sortable v-slot="props">
                {{ props.row.EndTime }}
            </b-table-column>

            <b-table-column field="BookId" label="Book Id" sortable centered v-slot="props">
                {{ props.row.BookId }}
            </b-table-column>
             <b-table-column field="MemberId" label="Member Id" sortable centered v-slot="props">
                {{ props.row.MemberId }}
            </b-table-column>
            <b-table-column field="Status" label="Status" sortable centered v-slot="props">
                {{ props.row.Status }}
            </b-table-column>

            <b-table-column label="Actions" v-slot="props">
              <button class="button is-small is-light" @click=" scrollToElement(props.row.Id)">
    <b-icon icon="update" size="is-small"></b-icon>
  </button>
  <button class="button is-small is-light" @click="deleteBookLog(props.row.Id)">
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
      Edit Book Log 
    </h2>
          <hr>
          <b-form >
            <b-field label="Select datetime start">
        <b-datetimepicker type="datetime-local"  v-model="booklog.StartTime"
        placeholder="Click to select..."
                testID="dateTimePicker"
                value={values.date}
                mode="datetime"
                is24Hour={true}
                display="default"
               onChange={booklog.StartTime.getTime()}
            >

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
        <b-datetimepicker type="datetime-local"  v-model="booklog.EndTime"
            placeholder="Click to select..."
            testID="dateTimePicker"
                value={values.date}
                mode="datetime"
                is24Hour={true}
                display="default"
               onChange={booklog.StartTime.getTime()}>

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
            <b-dropdown disabled aria-role="list">
            <template #trigger>
                <b-button
                    label="Disabled"
                    icon-right="menu-down"
                    v-model="booklog.Status" />
            </template>


            <b-dropdown-item aria-role="listitem">alerted</b-dropdown-item>
            <b-dropdown-item aria-role="listitem">on process</b-dropdown-item>
            <b-dropdown-item aria-role="listitem">done</b-dropdown-item>
        </b-dropdown>
            
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
    name: 'booklogPage',
    
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
        booklog: {
          Id:null,
          StartTime: new Date(),
          EndTime: new Date(),
          BookId: '',
          MemberId: '',
          Status: '',
        },
        
        validation: [],
      }
    },
    mounted() {
      this.$axios.get('/items/booklog')
        .then(response => {
          this.data = response.data.data

        })
        .catch(error => {
          console.log(error.response.data)
        })
    },
     methods: {
       async deleteBookLog(row) {
        
     
        await this.$axios.delete(`/items/booklog/${row}`)
          .then((res) => {
         
            const index = this.data.findIndex( item => item.Id === row );
            this.data.splice(index, 1) 
            this.$buefy.toast.open('Delete Book Log Success!')
            window.location = 'http://localhost:3000/booklog'  
          })
      },
      Add(){
        this.$router.push({
              name: 'createbooklog'
            })
      },
       scrollToElement(row) {
         this.$axios.get(`/items/booklog/${row}`)
        .then(response => {
            this.booklog.Id = response.data.data.Id;
            this.booklog.StartTime = Date.parse(response.data.data.StartTime);
            // this.booklog.StartTime   = response.data.data.StartTime;
            this.booklog.EndTime = Date.parse(response.data.data.EndTime);
            // this.booklog.EndTime   = response.data.data.EndTime;
            this.booklog.BookId   = response.data.data.BookId;
            this.booklog.MemberId   = response.data.data.MemberId;
            this.booklog.Status   = response.data.data.Status;
        })
        const el = this.$refs.scrollToMe;

    if (el) {
      // Use el.scrollIntoView() to instantly scroll to the element
      el.scrollIntoView({behavior: 'smooth'});
    }
       },

       async update() {
      
        await this.$axios.patch('/items/booklog/'+this.booklog.Id, {
          StartTime: this.booklog.StartTime,
          EndTime: this.booklog.EndTime,
          BookId: this.booklog.BookId,
          MemberId: this.booklog.MemberId,
          Status: this.booklog.Status,
          })
          .then(() => {
            
        
             this.$buefy.toast.open('Update Book Log Success!')
            window.location = 'http://localhost:3000/booklog'
          })
          .catch(error => {
      
            this.validation = error.response.data
          })
      },
      
    }
     

  }
</script>
