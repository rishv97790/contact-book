<script setup>
import { useRoute, useRouter } from 'vue-router';
import contactsData from "@/contacts.json";

const router = useRouter();

// Retrieve the contact ID from the route parameters
const route = useRoute();
const id = route.params.id;

// Load contacts from localStorage or use default contacts
const storedContacts = localStorage.getItem('contacts');
const contacts = storedContacts ? JSON.parse(storedContacts) : contactsData.contacts;

// Search for a contact
const contact = contacts.find(contact => contact.id === Number(id));

// Delete a contact
const handleDeleteContactById = async () => {
    const contactIndex = contacts.findIndex(contact => contact.id === Number(id));
    contacts.splice(contactIndex, 1);
    localStorage.setItem('contacts', JSON.stringify(contacts));

    // Redirect to the contacts list page after deletion
    router.push({ path: "/" });
};
</script>

<template>
    <div v-if="contact">
        <!-- Contact Header -->
        <h2 class="text-center pt-4 text-primary">{{ contact.firstName }} {{ contact.lastName }}</h2>
        
        <!-- Contact Details -->
        <div class="card shadow-sm border-light mt-4">
            <div class="card-body">
                <ul class="list-group list-group-flush">
                    <!-- Email Section -->
                    <li class="list-group-item">
                        <p class="m-0 fw-bold">Email</p>
                        <a :href="'mailto:' + contact.email" class="text-decoration-none text-muted">{{ contact.email }}</a>
                    </li>
                    
                    <!-- Phone Section -->
                    <li class="list-group-item">
                        <p class="m-0 fw-bold">Phone</p>
                        <p class="text-muted">
                            {{ contact ? `${String(contact.phone).slice(0, 3)}-${String(contact.phone).slice(3, 6)}-${String(contact.phone).slice(6)}` : '' }}
                        </p>
                    </li>
                </ul>
            </div>
        </div>

        <!-- Edit and Delete Buttons -->
        <div class="d-flex justify-content-center gap-4 mt-4">
            <RouterLink :to="'/edit/' + contact.id" class="btn btn-secondary btn-lg w-25">Edit</RouterLink>
            <button type="button" class="btn btn-danger btn-lg w-25" @click="handleDeleteContactById">
                Delete
            </button>
        </div>
    </div>

    <!-- Contact Not Found Message -->
    <div v-else>
        <p class="text-center pt-4 text-danger">Contact not found.</p>
    </div>
</template>

<style scoped>
/* Card Styling */
.card {
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

/* Title Styling */
h2 {
    font-size: 2rem;
    font-weight: 600;
    color: #007bff;
}

/* Card body */
.card-body {
    padding: 1.5rem;
}

/* List item Styling */
.list-group-item {
    padding: 15px;
    border: none;
}

/* Button Styling */
.btn {
    padding: 12px 18px;
    font-size: 1.1rem;
    border-radius: 50px;
}

.btn-secondary {
    background-color: #287cc6;
    border-color: #6c757d;
    transition: background-color 0.3s ease, border-color 0.3s ease;
}

.btn-secondary:hover {
    background-color: #10394f;
    border-color: #5a6368;
}

.btn-danger {
    background-color: #dc3545;
    border-color: #dc3545;
    transition: background-color 0.3s ease, border-color 0.3s ease;
}

.btn-danger:hover {
    background-color: #c82333;
    border-color: #c82333;
}

/* Link Styling */
a {
    font-size: 1rem;
    color: #495057;
    text-decoration: none;
}

a:hover {
    text-decoration: underline;
}

/* Contact Not Found Styling */
.text-danger {
    font-size: 1.25rem;
    font-weight: 500;
}
</style>
