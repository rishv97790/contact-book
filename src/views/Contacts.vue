<script setup>
import { RouterLink } from "vue-router";
import { ref, computed, onMounted } from "vue";
import contactsData from "@/contacts.json";
import Search from "../components/Search.vue";

// Load contacts from localStorage or use default contacts
const storedContacts = localStorage.getItem("contacts");
const contacts = ref(storedContacts ? JSON.parse(storedContacts) : contactsData.contacts);

// Search value
const searchValue = ref("");

// Filtered contacts based on search value
const filteredContacts = computed(() => {
  if (searchValue.value.trim() === "") {
    return [];
  } else {
    return contacts.value.filter((contact) => {
      const fullName = `${contact.firstName} ${contact.lastName}`.toLowerCase();
      return fullName.includes(searchValue.value.toLowerCase());
    });
  }
});

// Display contacts based on search value
const displayContacts = computed(() => {
  const contactsToDisplay = searchValue.value.trim() === "" ? contacts.value : filteredContacts.value;
  return contactsToDisplay.slice().sort((a, b) => a.lastName.localeCompare(b.lastName));
});

// Handle search input
const handleSearch = (value) => {
  searchValue.value = value;
};

// Save contacts in local storage on change
onMounted(() => {
  localStorage.setItem("contacts", JSON.stringify(contacts.value));
});
</script>

<template>
  <div class="container my-5">
    <h2 class="text-center mb-4">Contact List</h2>

    <!-- Search Component -->
    <Search :handleSearch="handleSearch" />

    <!-- Contacts List -->
    <div v-if="displayContacts.length" class="list-group my-4">
      <RouterLink
        v-for="contact in displayContacts"
        :key="contact.id"
        :to="'/details/' + contact.id"
        class="list-group-item list-group-item-action d-flex justify-content-between align-items-center"
      >
        <span>{{ contact.firstName }} {{ contact.lastName }}</span>
        <small class="text-muted">{{ contact.company }}</small>
      </RouterLink>
    </div>

    <!-- No Contacts Found Message -->
    <div v-else class="text-center pt-4">
      <p class="mb-2">No contacts found.</p>
      <RouterLink :to="'/new'" class="btn btn-primary">Add a New Contact</RouterLink>
    </div>
  </div>
</template>

<style scoped>
/* Container styling */
.container {
  max-width: 600px;
}

/* Title styling */
h2 {
  color: #007bff;
}

/* List item hover and active states */
.list-group-item {
  transition: background-color 0.2s;
}
.list-group-item:hover,
.list-group-item:focus {
  background-color: #f8f9fa;
  color: #0056b3;
}

/* Spacing around small text */
.list-group-item small {
  margin-left: auto;
}

/* Button styling */
.btn-primary {
  background-color: #007bff;
  border-color: #007bff;
  transition: background-color 0.2s, border-color 0.2s;
}
.btn-primary:hover {
  background-color: #0056b3;
  border-color: #0056b3;
}
</style>
