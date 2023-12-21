<template lang="pug">
button.add-btn
  img(src="../assets/img/edit.svg", alt="alt")
</template>
<script>
export default {};
</script>
<style lang="scss">
.add-btn {
  position: fixed;
  right: 30px;
  bottom: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0px 4px 8px 3px rgba(0, 0, 0, 0.15);
  filter: drop-shadow(0px 1px 3px rgba(0, 0, 0, 0.3));
  background: linear-gradient(
      0deg,
      rgba(103, 80, 164, 0.11),
      rgba(103, 80, 164, 0.11)
    ),
    #fffbfe;
  border-radius: 16px;
  padding: 19px;
  outline: none;
  border: none;
  cursor: pointer;
}
</style>