<template>
    <nav class="navbar">
      <ul class="navbar-menu">
        <li><a href="#">Home</a></li>
        <li><a href="community" >Community</a></li>
        <li><a href="profil">Profile</a></li>
        <li><a href="login">Login</a></li>
      </ul>
    </nav>
  </template>
  
  <script>
  export default {
    name: 'NavbarWeb',
  }
  </script>
  
  <style lang="scss">
  .navbar {
    background-color: #333;
    color: #fff;
    display: flex;
    justify-content: center; 
    align-items: center;
    padding: 10px 0; 

    .navbar-menu {
      list-style: none;
      margin: 0;
      padding: 0;

      li {
        display: inline-block;
        margin-right: 5 px; 

        a {
          text-decoration: none;
          color: #fff;
          font-weight: bold;
          padding: 5px 10px; 

          &:hover {
            text-decoration: underline;
          }
        }
      }
    }
  }
</style>
