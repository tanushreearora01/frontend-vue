<template>
  <div class="container">
    <h1>Housing Recommendation System</h1>
    <button @click="showAddPropertyForm = true">Add Property</button>
    <div class="property-container">
      <div v-for="property in properties" :key="property._id" class="property-list-container">
        <property-card :property="property" :onDelete="deleteProperty" :onEdit="editProperty" :onSubmit="getProperties" mode="edit" />
      </div>

      <property-card
      v-if="showAddPropertyForm"
      @onSubmit="addProperty"
      :property="newProperty"
      @onAddDiscard="discardAddChanges"
      mode="add"
    />
    </div>
  </div>
</template>
  
<script>
import axios from 'axios';
import PropertyCard from '@/components/PropertyCard.vue';

export default {
  components: {
    PropertyCard,
  },
  data() {
    return {
      properties: [],
      newProperty: {
        type: 'Room',
        size: '',
        price: 0,
        address: '',
        roomDistribution: '',
        link: [],
        brokerName: '',
        brokerNumber: '',
        booked: false
      },
      showAddPropertyForm: false,
    };
  },
  mounted() {
    this.getProperties();
  },
  methods: {
    async getProperties() {
      try {
        const response = await axios.get('https://backend-server-housing-4b8bba9dad56.herokuapp.com/api/properties');
        this.properties = response.data;
      } catch (error) {
        console.error('Error fetching properties:', error);
      }
    },
    async addProperty(newProperty) {
      try {
        const response = await axios.post('https://backend-server-housing-4b8bba9dad56.herokuapp.com/api/properties', newProperty);
        // this.properties.push(response.data);
        this.showAddPropertyForm = false;
        this.getProperties();
      } catch (error) {
        console.error('Error adding property:', error);
      }
    },
    async deleteProperty(propertyId) {
      try {
        await axios.delete(`https://backend-server-housing-4b8bba9dad56.herokuapp.com/api/properties/${propertyId}`);
        //this.properties = this.properties.filter((property) => property._id !== propertyId); 
        this.getProperties();
      } catch (error) {
        console.error('Error deleting property:', error);
      }
    },
    async editProperty(propertyId, updatedData) {
      try {
        await axios.put(`https://backend-server-housing-4b8bba9dad56.herokuapp.com/api/properties/${propertyId}`, updatedData);
      } catch (error) {
        console.error('Error updating property:', error);
        throw error; 
      }
    },
    discardAddChanges() {
      this.showAddPropertyForm = false;
    },
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  text-align: center;
}

.container h1 {
  margin: 2.5rem;
}

.property-container {
  display: grid;
  max-width: 1200px;
  margin-inline: auto;
  padding-inline: 24px;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 24px;
}

.property-list-container {
  display: flex;
}
</style>