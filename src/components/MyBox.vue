<template>
    <div class="box">
        {{text}} - {{age}}
        <span :class="['status', status]"> - {{ status }}</span>
    </div>
</template>
<script>
export default {
    name: 'MyBox',
    props: {
      text: {
        type: String,
        default: "N/A"
      },
      age: {
        type: Number,
        required: true
      },
      status: {
        type: String,
        validator: (value) => ["active", "inactive"].includes(value),
        default: "active"
      }
    }
    
}
</script>
<style>
.active {
  background-color: green;
}
.inactive {
  background-color: red;
}
.box {
  min-width: 30px;
  min-height: 30px;
  background-color: blueviolet;
  align-items: center;
  justify-content: center;
  display: flex;

}
</style>