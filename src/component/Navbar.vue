<template>
<div class="navbar">
    <h1>Moral Combat</h1>
    <span> Choisissez une compétence pour vaincre Julien. Après chaque action un point de skill est régénéré.  </span>
</div>
</template>

<script>
export default {
    name: 'Navbar'
}
</script>
<style scoped>
.navbar {
    background-color: #f5f5f5;
    padding: 10px;
    border-bottom: 1px solid #e0e0e0;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}
</style>