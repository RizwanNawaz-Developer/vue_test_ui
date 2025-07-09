<template>
	<div class="container py-5 min-vh-100">
        <!-- Title -->
        <h2 class="fw-bold mb-2 text-white text-center">Time to start your BVI company!</h2>
        <p class="mb-5 text-light text-center">
            All questions below must be answered. Take your time, and if you need to take a break,
            the form will save automatically and you can continue later. <br>Good luck!
        </p>

  		<BviCompanyForm />
	</div>
</template>

<script setup>
  import BviCompanyForm from './components/BviCompanyForm.vue'
</script>

