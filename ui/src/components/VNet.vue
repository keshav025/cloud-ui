<template>
  <div>
    <v-app>
      <v-container>
        <h1>VNet Service</h1>

       <!-- Toggle Button to Show/Hide Create Form -->
    <v-btn color="primary" @click="showCreateForm = !showCreateForm">
      {{ showCreateForm ? 'Hide Create Form' : 'Show Create Form' }}
    </v-btn>

    <!-- Create VNet Form -->
    <v-form v-if="showCreateForm" @submit.prevent="createVNet">
      <v-text-field v-model="newVNet.name" label="Name" required></v-text-field>
      <v-text-field v-model="newVNet.cidr" label="CIDR" required></v-text-field>
      <v-text-field v-model="newVNet.platform" label="Platform" required></v-text-field>
      <!-- <v-text-field v-model="newVNet.location" label="Location" required></v-text-field> -->
      <v-btn color="primary" type="submit">Create VNet</v-btn>
    </v-form>

        <!-- List of VNets -->
        <v-data-table
        
          :headers="headers" 
          :items="vnets"
          item-key="id"
          class="elevation-8"
          height="100%"
        >
          <template v-slot:[`item.id`]="{ item }">
            {{ item.id }}
          </template>
          <template v-slot:[`item.name`]="{ item }">
            {{ item.name }}
          </template>
          <template v-slot:[`item.createdAt`]="{ item }">
            {{ item.createdAt }}
          </template>
          <template v-slot:[`item.cidr`]="{ item }">
            {{ item.cidr }}
          </template>
          <template v-slot:[`item.platform`]="{ item }">
            {{ item.platform }}
          </template>
          <template v-slot:[`item.location`]="{ item }">
            {{ item.location }}
          </template>
          <template v-slot:[`item.status`]="{ item }">
            {{ item.status }}
          </template>
          <template v-slot:[`item.actions`]="{ item }">
            <!-- <v-btn @click="getVNet(item.id)">View</v-btn> -->
            <v-btn @click="updateVNet(item.id)">Recreate</v-btn>
            <v-btn v-if = "item.status === 'completed' " @click="deleteVNet(item.id)">Delete</v-btn>
          </template>
        </v-data-table>
      </v-container>
    </v-app>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "VNet",
  data() {
    return {
      showCreateForm: false, 
      newVNet: {
        name: '',
        cidr: '',
        platform: '',
        status: '',
        location: '',
      },
      vnets: [],
      headers: [
        { title:'ID', text: 'ID', value: 'id' , align: 'left'},
        {title: 'Name', text: 'Name', value: 'name' , align: 'left'},
        { title: 'Created At',text: 'Created At', value: 'createdAt', align: 'left' },
        { title: 'CIDR', text: 'CIDR', value: 'cidr' , align: 'left'},
        { title: 'Platform', value: 'platform' , align: 'left'},
        {title: 'Location', text: 'Location', value: 'location' , align: 'left'},
        {title: 'Status', text: 'Status', value: 'status' , align: 'left'},
        {title: 'Actions', text: 'Actions', value: 'actions', sortable: false , align: 'left'},
      ],
    };
  },
  mounted() {
    this.listVNets();
  },
  methods: {
    // Create VNet
    async createVNet() {
      try {
        await axios.post('https://vnetsvc.happysky-9fb8abef.centralindia.azurecontainerapps.io/api/v1/build/vnet', this.newVNet);
        this.listVNets(); // Refresh the list after creating
        this.showCreateForm = false; // Hide the form after submission
        // Clear form fields if needed
        this.clearForm();
      } catch (error) {
        console.error('Error creating VNet:', error);
      }
    },

    // List VNets
    async listVNets() {
      try {
        const response = await axios.get('https://vnetsvc.happysky-9fb8abef.centralindia.azurecontainerapps.io/api/v1/build/vnet');
        this.vnets = response.data;
      } catch (error) {
        console.error('Error listing VNets:', error);
      }
    },

    // Get VNet by ID
    async getVNet(id) {
      try {
        const response = await axios.get(`https://vnetsvc.happysky-9fb8abef.centralindia.azurecontainerapps.io/api/v1/build/vnet/${id}`);
        console.log('VNet Details:', response.data);
      } catch (error) {
        console.error('Error getting VNet:', error);
      }
    },

    // Update VNet by ID
    async updateVNet(id) {
      try {
        const response = await axios.put(`https://vnetsvc.happysky-9fb8abef.centralindia.azurecontainerapps.io/api/v1/build/vnet/${id}`);
        console.log('VNet Details:', response.data);
      } catch (error) {
        console.error('Error getting VNet:', error);
      }
    },

    // Delete VNet by ID
    async deleteVNet(id) {
      try {
        await axios.delete(`https://vnetsvc.happysky-9fb8abef.centralindia.azurecontainerapps.io/api/v1/build/vnet/${id}`);
        this.listVNets(); // Refresh the list after deleting
      } catch (error) {
        console.error('Error deleting VNet:', error);
      }
    },

     // Clear form fields
     clearForm() {
      this.newVNet = {
        name: '',
        cidr: '',
        platform: '',
        deployed: '',
      };
    },
  },
};
</script>

<style>
/* Add any custom styles here */
</style>
