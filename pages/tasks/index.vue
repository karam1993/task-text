<template>
  <v-container>
    <v-card max-width="550" class="mx-auto">
      <div class="text-center py-3" v-if="loadingData">
        <v-progress-circular
          indeterminate
          color="primary"
        ></v-progress-circular>
      </div>
      <v-toolbar v-else color="cyan" dark>
        <v-menu
            bottom
            left
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                dark
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item @click="filterTimeStatus(null)">
                <v-list-item-title>All</v-list-item-title>
              </v-list-item>
              <v-list-item @click="filterTimeStatus('daily')">
                <v-list-item-title>Daily</v-list-item-title>
              </v-list-item>
              <v-list-item @click="filterTimeStatus('weekly')">
                <v-list-item-title>Weekly</v-list-item-title>
              </v-list-item>
              <v-list-item @click="filterTimeStatus('monthly')">
                <v-list-item-title>Monthly</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>



        <v-toolbar-title>My Tasks</v-toolbar-title> 
        <v-spacer></v-spacer>

        <v-text-field
            v-if="showSearch"
            v-model="searchQuery"
            label="Search"
            class="d-inline-block mt-7 mx-2"
            placeholder="Search"
            @input="searchTask"
            solo
          ></v-text-field>
          
       <div class="">
          <v-btn icon @click="showSearch = !showSearch">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
       </div>
      </v-toolbar>

      <v-list three-line>
        <v-list-item v-for="(item, index) in filteredTasks" :key="index">
          <v-list-item-icon>
            <v-checkbox v-model="item.isCompleted"></v-checkbox>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }} <v-chip small>{{ item.time }}</v-chip> </v-list-item-title>
            <v-list-item-subtitle class="my-2">Name : {{ item.lastCompletedEvent ?  item.lastCompletedEvent.user.name :' - '}}</v-list-item-subtitle>
            <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-card>
  </v-container>
</template>



<script>
import FilterTable from "@/components/FilterTable.vue";
export default {
  data() {
    return {
      mainTasks: [],
      filteredTasks : [] ,
      loadingData: false,
      showSearch : false ,
      searchQuery : ''
      
    };
  },
  components: {
    FilterTable,
  },
  created() {
    this.getMyTasksList();
  },
  methods: {
    getMyTasksList() {
      this.loadingData = true;
      this.$axios
        .get("/api/dummy_data.json")
        .then((res) => {
          this.mainTasks = res.data.data;
          this.filteredTasks = [... this.mainTasks]
        })
        .catch((err) => {
          console.log(err);
        })
        .finally(() => {
          this.loadingData = false;
        });
    },
    searchTask(){
      const query = this.searchQuery.toLowerCase();
      const item =  this.mainTasks.filter(item => {
        return item.title.toLowerCase().includes(query);
      });
      this.filteredTasks = item
    },
    checkIsCompleted(item) {
      return 1;
    },
    filterTimeStatus(state = null){
      if(state == null) {
        this.filteredTasks = this.mainTasks
        return
      }
      const query = state;
      const item =  this.mainTasks.filter(item => {
        return item.occurrence == state
      });
      this.filteredTasks = item
    }
  },
};
</script>