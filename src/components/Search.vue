<script setup>
    const { handleSearch } = defineProps(['handleSearch'])

    const onInputValueSearch = (event) => {
        handleSearch(event.target.value)
    }
</script>

<template>
    <div class="search-container">
        <form class="search-form">
            <input
                name="searchInput"
                class="search-input"
                type="search"
                placeholder="Search"
                aria-label="Search"
                @input="onInputValueSearch"
            />
        </form>
    </div>
</template>

<style scoped>
/* General Container Styling */
.search-container {
    display: flex;
    justify-content: center;
    margin-top: 20px;
    padding: 0 15px;
}

/* Form Styling */
.search-form {
    width: 100%;
    max-width: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
}

/* Input Styling */
.search-input {
    width: 100%;
    padding: 10px 20px;
    font-size: 1.1rem;
    border: 2px solid #ddd;
    border-radius: 25px;
    outline: none;
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.search-input:focus {
    border-color: #007bff;
    box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

.search-input::placeholder {
    color: #6c757d;
    font-style: italic;
}

/* Adjust for mobile screens */
@media (max-width: 576px) {
    .search-form {
        max-width: 300px;
    }
}
</style>
