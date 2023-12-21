<template lang="pug">
Transition(name="modal")
  .modal(@click="$emit('showOrClose')")
    .modal-block(@click.stop)
      h2 {{ edit ? words.editwindownbtn[lang] : words.addbtn[lang] }} 
      form.modal-block-inputs(action="", @submit.prevent="send")
        label
          span Title
          input(type="text", placeholder="", v-model="user.title")
        label
          span Content
          textarea(
            required,
            placeholder="",
            rows="1",
            v-model="user.text"
          )
        .modal-block-btns
          button(type="button", @click="$emit('showOrClose')") {{ words.closebtn[lang] }}
          button(v-if="!edit") {{ words.addbtn[lang] }}
          button(type="button", v-else, @click="changeNote") {{ words.editbtn[lang] }}
</template>
<script>
export default {
  props: {
    edit: Boolean,
    currentId: Number,
    editNote: Object,
    lang: String
  },
  inject: ['words'],
  data() {
    return {
      user: { title: "", text: "", id: this.currentId },
    };
  },
  methods: {
    send() {
      this.$emit("addNote", { ...this.user });
      this.$emit("showOrClose");
      for (const key in this.user) this.user[key] = "";
      this.user.id = localStorage.id++
    },
    changeNote() {
      if (this.user.title != "" && this.user.text != "") {
        const editedNote = {
          id: this.editNote.id,
          title: this.user.title,
          text: this.user.text,
          date: "Изменено " + new Date().toLocaleString(),
        };
        this.$emit("editedNote", editedNote);
        this.user.title = ''
        this.user.text = ''
      }
    },
  },
};
</script>
<style lang="scss">
.modal {
  user-select: none;
  background: rgba(0, 0, 0, 0.35);
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  &-block {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    width: 312px;
    background: linear-gradient(
        0deg,
        rgba(103, 80, 164, 0.11),
        rgba(103, 80, 164, 0.11)
      ),
      rgb(40 38 40);
    border-radius: 28px;
    padding: 24px;
    h2 {
      line-height: 32px;
      color: white;
      margin-bottom: 16px;
    }
    &-inputs {
      display: flex;
      gap: 16px;
      flex-direction: column;
      label {
        display: flex;
        flex-direction: column;
        position: relative;
        span {
          position: absolute;
          font-size: 12px;
          line-height: 16px;
          letter-spacing: 0.4px;
          color: #6750a4;
          top: 8px;
          left: 16px;
          user-select: none;
        }
        input,
        textarea {
          background: #e7e0ec;
          border-radius: 4px 4px 0px 0px;
          resize: none;
          outline: none;
          border: none;
          border-bottom: 1px solid #1c1b1f;
          padding: 23px 16px 9px;
        }
      }
    }
    &-btns {
      display: flex;
      justify-content: flex-end;
      gap: 32px;
      margin-top: 34px;
      button {
        background: transparent;
        outline: none;
        border: none;
        font-family: RM;
        text-transform: uppercase;
        color: #cf1b1b;
        font-size: 14px;
        cursor: pointer;
        &:last-child {
          color: #6750a4;
        }
      }
    }
  }
}
.modal-enter-active,
.modal-leave-active {
  transition: 0.3s linear;
}
.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(1.5);
}
</style>