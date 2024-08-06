<script>

</script>

<template>

  <nav class="container">
      <div>
           <img src="../assets/planet-5-logo.png" class="logo" alt="planet Logo">
      </div>
  </nav>
</template>

<style scoped>
  .container {
    background-color: #2b3843;
    height: 72px;
    border-radius: 10px;
    padding: 5px;
    display: flex;
    align-items: center; /* Center items vertically */
    justify-content: space-between; /* Space between logo and text */
  }

  .logo {
    height: 60px;
    width: 150px;
    margin-left: 20px;
    border-radius: 5px;
  }

  .logo-text {
    color: rgb(56, 97, 131);
    font-size: 30px;
    font-weight: 600;
    margin-left: 20px;
  }

  /* Media Queries for Responsiveness */

  @media (max-width: 1200px) {
    .logo {
      height: 50px;
      width: 120px;
    }

    .logo-text {
      font-size: 26px;
    }
  }

  @media (max-width: 992px) {
    .logo {
      height: 40px;
      width: 100px;
    }

    .logo-text {
      font-size: 22px;
    }
  }

  @media (max-width: 768px) {
    .container {
      flex-direction: column; /* Stack items vertically on smaller screens */
      align-items: center; /* Center items horizontally */
      height: auto; /* Adjust height for better fit */
      padding: 10px; /* Add padding for spacing */
    }

    .logo {
      height: 40px;
      width: 100px;
      margin: 10px 0; /* Add margin for spacing */
    }

    .logo-text {
      font-size: 18px;
      margin: 0; /* Remove margin for better fit */
    }
  }
</style>
