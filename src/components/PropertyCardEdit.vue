<template>
    <div class="input-container">
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
            <input v-model="editedProperty.roomDistribution" />
        </div>
        <div>
            <label>Photos Link:</label>
            <input v-model="editedProperty.link" />
        </div>
        <div>
            <label>Broker Name:</label>
            <input v-model="editedProperty.brokerName" />
        </div>
        <div>
            <label>Broker Number:</label>
            <input v-model="editedProperty.brokerNumber" />
        </div>
        <div>
            <label>Booked:</label>
            <select v-model="editedProperty.booked">
                <option value="true">Yes</option>
                <option value="false">No</option>
            </select>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        editedProperty: Object,
    },
};
</script>

<style scoped>
.input-container {
    display: flex;
    flex-direction: column;
}

.input-container input {
    margin-bottom: 5px;
}
</style>