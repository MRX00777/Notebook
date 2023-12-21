<template lang="pug">
.note
  .note-top(:class="{between: !grid}")
    h3 {{ note.title }}
    p {{ note.date }}
  p {{ note.text }}
  .note-btns
    button(@click="$emit('changeNote', note.id)")
      img(src="../assets/img/edit.svg", alt="edit")
      span {{ words.editbtn[lang] }}
    button(@click="$emit('remove', note.id)")
      img(src="../assets/img/trash.svg", alt="trash")
      span {{ words.deledit[lang] }}
</template>
<script>
export default {
  props: {
    grid: Boolean,
    note: Object,
    lang: String
  },
  inject: ['words']
};
</script>
<style lang="scss">
.note {
  background: linear-gradient(
      0deg,
      rgba(103, 80, 164, 0.05),
      rgba(103, 80, 164, 0.05)
    ),
    rgb(41 41 41);
  box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.3),
    0px 1px 3px 1px rgba(0, 0, 0, 0.15);
  border-radius: 16px;
  padding: 16px 16px 28px 16px;
 
  &-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    // flex-wrap: wrap;
    // flex-direction: column;

    @media (max-width: 700px) {
      flex-wrap: wrap;
    }
    h3 {
      font-family: RM;
      font-size: 16px;
      line-height: 24px;
      letter-spacing: 0.1px;
      color: white;
      word-break: break-all;
    }
    p {
      font-size: 14px;
      letter-spacing: 0.25px;
      color: #cac4d0;
      margin-bottom: 16px;
    }
  }
  > p {
    word-break: break-all;
    font-size: 14px;
    line-height: 20px;
    letter-spacing: 0.25px;
    color: white;
    margin-bottom: 29px;
  }
  &-btns{
    display: flex;
    justify-content: flex-end;
    gap: 20px;
    @media (max-width: 700px) {
      flex-direction: column;
    }
    button{
        background: transparent;
        border: none;
        outline: none;
        display: flex;
        align-items: center;
        gap: 10px;
        color: #6750A4;
        font-size: 14px;
        font-weight: 700;
        cursor: pointer;
        &:last-child{
            color: #CF1B1B;
        }
    }
  }
}
.between{
  display: flex;
  justify-content: space-between;
}
</style>