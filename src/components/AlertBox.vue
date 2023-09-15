<template>
    <div class="alert-box">
      <strong>Error!</strong>
      <br/>
      <slot />
    </div>
  </template>

  <style scoped>
  .alert-box {
    color: black;
    border: 1px solid red;
    border-radius: 4px;
    padding: 20px;
    background-color: pink;
  }

  strong {
      color: red;
  }
  </style>
