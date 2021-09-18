<template>
  <div class="info-box-container">
    <p><slot name="info-box">default</slot></p>
  </div>
</template>

<style>
.info-box-container {
  padding: 5px;
  margin-bottom: 5px;
  color: white;
  background: rgb(0, 89, 255);
}
</style>