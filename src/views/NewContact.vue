<script setup>
    import { ref, computed } from 'vue'
    import { useRouter } from 'vue-router'
    import contactsData from "@/contacts.json"
    
    const router = useRouter();
    
    // Load contacts from localStorage or use default contacts
    let contacts = ref(JSON.parse(localStorage.getItem('contacts')) || contactsData.contacts);

    // Define reactive variables for input fields
    let firstName = ref('');
    let lastName = ref('');
    let email = ref('');
    let phone = ref('');

    // Handle click event to add a new contact
    const handleClick = () => {
        // Computed properties to capitalize the first letter of first name and last name
        const capitalizedFirstName = computed(() => firstName.value.charAt(0).toUpperCase() + firstName.value.slice(1))
        const capitalizedLastName = computed(() => lastName.value.charAt(0).toUpperCase() + lastName.value.slice(1))

        // Create a new contact object
        const newContact = {
            id: contacts.value.length + 1,
            firstName: capitalizedFirstName.value,
            lastName: capitalizedLastName.value,
            email: email.value,
            phone: phone.value,
        };

        // Update contacts ref with the new contact
        contacts.value.push(newContact);

        // Update localStorage with the updated contacts list
        localStorage.setItem('contacts', JSON.stringify(contacts.value));

        // Redirect to the details page of the new contact
        router.push({ path: "/details/" + newContact.id });
    }
</script>

<template>
    <h2 class="text-center pt-4 text-primary">New Contact</h2>
    <div class="container">
        <form id="form" class="form-group needs-validation" @submit.prevent="handleClick">
            <div class="row mt-3">
                <div class="col-md-6">
                    <label for="firstName" class="form-label">First Name</label>
                    <input id="firstName" class="form-control" type="text" name="firstName" v-model="firstName" placeholder="Enter your first name" required />
                </div>
                <div class="col-md-6">
                    <label for="lastName" class="form-label">Last Name</label>
                    <input id="lastName" class="form-control" type="text" name="lastName" v-model="lastName" placeholder="Enter your last name" required />
                </div>
            </div>
            <div class="mt-3">
                <label for="email" class="form-label">Email</label>
                <input id="email" class="form-control" type="email" name="email" v-model="email" placeholder="Enter your email address" required />
            </div>
            <div class="mt-3">
                <label for="phone" class="form-label">Phone</label>
                <input id="phone" class="form-control" type="text" name="phone" v-model="phone" placeholder="Enter your phone number" required />
            </div>
            <div class="text-center mt-4">
                <button id="send-form" type="submit" class="btn btn-primary btn-lg w-100">Submit</button>
            </div>
        </form>
    </div>
</template>

<style scoped>
/* Container styling */
.container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
}

/* Heading styling */
h2 {
    font-size: 2rem;
    font-weight: 600;
    color: #007bff;
}

/* Form control styling */
.form-label {
    font-size: 1.1rem;
    font-weight: 500;
    color: #333;
}

.form-control {
    border-radius: 10px;
    font-size: 1.1rem;
    padding: 12px;
    border: 1px solid #ccc;
    transition: all 0.3s ease;
}

/* Input focus effect */
.form-control:focus {
    border-color: #007bff;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

/* Submit button styling */
.btn {
    font-size: 1.2rem;
    padding: 14px;
    border-radius: 50px;
    transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn-primary {
    background-color: #007bff;
    border-color: #007bff;
}

.btn-primary:hover {
    background-color: #0056b3;
    border-color: #004085;
    transform: translateY(-2px);
}

/* Input spacing for responsiveness */
.row .col-md-6 {
    margin-bottom: 15px;
}

/* Form validation state styling */
.needs-validation .form-control:invalid {
    border-color: #dc3545;
}

/* Mobile responsiveness */
@media (max-width: 576px) {
    .container {
        padding: 10px;
    }

    .form-control {
        font-size: 1rem;
    }

    .btn {
        font-size: 1.1rem;
    }
}
</style>
