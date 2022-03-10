<template>
<section class="section">
  <div class="columns is-mobile">
      <card title="Member" icon="account">
      7
      </card>

      <card title="Visitor" icon="history">
        17
      </card>

     
    </div>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h2 class="title is-3 has-text-grey">
      Data Book Log 
    </h2>
          <hr>
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
            :debounce-page-input="inputDebounce"
            >

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

         
        </b-table>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</section>
</template>

<script>
  export default {
    name: 'IndexPage',
    
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
          .then(() => {
         
            this.posts.splice(row.index, 1);
          })
      },
     
    }

  }
</script>

