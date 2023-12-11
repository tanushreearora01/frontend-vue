<template>
    <div>
        <h3>{{ property.type }} - {{ property.size }}</h3>
        <p><strong>Price:</strong> {{ property.price }}</p>
        <p><strong>Address:</strong> {{ property.address }}</p>
        <p><strong>Room Distribution:</strong> {{ property.roomDistribution }}</p>
        <p><strong>Broker Name:</strong> {{ property.brokerName }}</p>
        <p><strong>Broker Number:</strong> {{ property.brokerNumber }}</p>
        <p><strong>Booked:</strong> {{ property.booked ? "Yes" : "No" }}</p>
    </div>
</template>

<script>
export default {
    props: {
        property: Object,
    },
};
</script>
