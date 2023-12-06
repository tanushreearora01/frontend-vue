<template>
  <article>
    <div class="article-wrapper">
      <figure>
        <img :src="property.link&&property.link[0]" alt="Property Image" @click="showNextImage" style="cursor: pointer" />
      </figure>
      <div class="article-body">
        <div v-if="!isEditing && mode != 'add'">
          <h3>{{ property.type }} - {{ property.size }}</h3>
          <p><strong>Price:</strong> {{ property.price }}</p>
          <p><strong>Address:</strong> {{ property.address }}</p>
          <p><strong>Room Distribution:</strong> {{ property.roomDistribution }}</p>
          <p><strong>Broker Name:</strong> {{ property.brokerName }}</p>
          <p><strong>Broker Number:</strong> {{ property.brokerNumber }}</p>
          <p><strong>Booked:</strong> {{ property.booked ? "Yes" : "No" }}</p>
        </div>
        <div v-if="isEditing" class="input-container">
          <div>
            <label>Type:</label>
            <select v-model="editedProperty.type">
              <option value="House">House</option>
              <option value="Flat">Flat</option>
            </select>
          </div>
          <div>
            <label>Size:</label>
            <input v-model="editedProperty.size" />
          </div>
          <div>
            <label>Price:</label>
            <input v-model="editedProperty.price" />
          </div>
          <div>
            <label>Address:</label>
            <input v-model="editedProperty.address" />
          </div>
          <div>
          <label>Room Distribution:</label>
          <input v-model="editedProperty.roomDistribution"/>
          </div>
          <div>
          <label>Photos Link:</label>
          <input v-model="editedProperty.link"/>
          </div>
          <div>
          <label>Broker Name:</label>
          <input v-model="editedProperty.brokerName"/>
          </div>
          <div>
          <label>Broker Number:</label>
          <input v-model="editedProperty.brokerNumber"/>
          </div>
          <div>
          <label>Booked:</label>
          <select v-model="editedProperty.booked">
            <option value="true">Yes</option>
            <option value="false">No</option>
          </select>
          </div>
          <!-- Add other input fields as needed -->
        </div>
        <button v-if="!isEditing" @click="editProperty">Edit</button>
        <button v-if="isEditing && mode != 'add'" @click="submitChanges">
          Submit Changes
        </button>
        <button v-if="mode === 'add'" @click="addProperty">Add Property</button>
        <button v-if="isEditing && mode != 'add'" @click="discardChanges">
          Discard Changes
        </button>
        <button v-if="mode === 'add'" @click="discardAddChange">
          Discard Changes
        </button>
        <button v-if="mode != 'add'" @click="deleteProperty">Delete</button>
        <div v-if="showSuccessMessage">{{ successMessage }}</div>
        <div v-if="showErrorMessage">{{ errorMessage }}</div>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  props: {
    property: Object,
    onDelete: Function,
    onEdit: Function,
    onSubmit: Function,
    onAddDiscard: Function,
    mode: String,
  },
  data() {
    return {
      isEditing: this.mode === "add",
      editedProperty: {
        type: this.property.type,
        size: this.property.size,
        price: this.property.price,
        address: this.property.address,
        roomDistribution: this.property.roomDistribution,
        link: this.property.link,
        brokerName: this.property.brokerName,
        brokerNumber: this.property.brokerNumber,
        booked: this.property.booked,
        // Add other fields as needed
      },
      showSuccessMessage: false,
      showErrorMessage: false,
      successMessage: "",
      errorMessage: "",
    };
  },
  methods: {
    editProperty() {
      this.isEditing = true;
    },
    discardChanges() {
      this.isEditing = false;
    },
    addProperty() {
      if (this.isValid()) {
        this.$emit("onSubmit", this.editedProperty);
        this.clearForm();
      } else {
        this.showErrorMessage = true;
        this.errorMessage = "Please fill in all fields";
        // You can hide the error message after a certain duration if needed
      }
    },
    discardAddChange() {
      this.$emit("onAddDiscard");
      this.clearForm();
    },
    async submitChanges() {
      try {
        await this.onEdit(this.property._id, this.editedProperty);
        this.isEditing = false;
        this.showSuccessMessage = true;
        this.onSubmit();
        this.successMessage = "Property updated successfully";
        // You can hide the success message after a certain duration if needed
      } catch (error) {
        console.error("Error updating property:", error);
        this.showErrorMessage = true;
        this.errorMessage = "Failed to update property";
        // You can hide the error message after a certain duration if needed
      }
    },
    deleteProperty() {
      this.onDelete(this.property._id);
      this.onSubmit();
    },
    clearForm() {
      this.newProperty = {
        type: '',
        size: '',
        size: '',
        price: 0,
        address: '',
        roomDistribution: '',
        link: [],
        brokerName: '',
        brokerNumber: '',
        booked: false
      };
      this.showSuccessMessage = false;
      this.showErrorMessage = false;
      this.isEditing = this.mode === "add"; // Reset isEditing based on the mode
    },
    isValid() {
      // Add validation logic if needed
      return (
        this.editedProperty.type.trim() &&
        this.editedProperty.size.trim() &&
        this.editedProperty.price &&
        this.editedProperty.address.trim() &&
        this.editedProperty.roomDistribution.trim() !== '' &&
        this.editedProperty.brokerName.trim() !== '' &&
        this.editedProperty.brokerNumber.trim() !== ''
      );
    },
    showNextImage() {
      // Increment the image index to display the next image
      if (this.newProperty.link && this.newProperty.link.length > 1) {
        this.imageIndex = (this.imageIndex + 1) % this.newProperty.link.length;
        this.imageUrl = this.newProperty.link[this.imageIndex];
      }
    },
  },
};
</script>

<style scoped>
article {
  --img-scale: 1.001;
  --title-color: black;
  --link-icon-translate: -20px;
  --link-icon-opacity: 0;
  position: relative;
  border-radius: 16px;
  box-shadow: none;
  background: #fff;
  transform-origin: center;
  transition: all 0.4s ease-in-out;
  overflow: hidden;
}

article a::after {
  position: absolute;
  inset-block: 0;
  inset-inline: 0;
  cursor: pointer;
  content: "";
}

/* basic article elements styling */
article h2 {
  margin: 0 0 18px 0;
  font-family: "Bebas Neue", cursive;
  font-size: 1.9rem;
  letter-spacing: 0.06em;
  color: var(--title-color);
  transition: color 0.3s ease-out;
}

figure {
  margin: 0;
  padding: 0;
  aspect-ratio: 16 / 9;
  overflow: hidden;
}

article img {
  max-width: 100%;
  transform-origin: center;
  transform: scale(var(--img-scale));
  transition: transform 0.4s ease-in-out;
}

.article-body {
  padding: 24px;
}

article a {
  display: inline-flex;
  align-items: center;
  text-decoration: none;
  color: #28666e;
}

article a:focus {
  outline: 1px dotted #28666e;
}

article a .icon {
  min-width: 24px;
  width: 24px;
  height: 24px;
  margin-left: 5px;
  transform: translateX(var(--link-icon-translate));
  opacity: var(--link-icon-opacity);
  transition: all 0.3s;
}

/* using the has() relational pseudo selector to update our custom properties */
article:has(:hover, :focus) {
  --img-scale: 1.1;
  --title-color: #28666e;
  --link-icon-translate: 0;
  --link-icon-opacity: 1;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 10px 36px 0px,
    rgba(0, 0, 0, 0.06) 0px 0px 0px 1px;
}

.input-container {
  display: flex;
  flex-direction: column;
}

.input-container input {
  margin-bottom: 5px;
}
</style>
