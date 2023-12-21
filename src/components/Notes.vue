<template lang="pug">
section.notes
  .container
    .notes-top
      h2(v-if="search == ''") {{ notes.length == 0 ? words.noinfobar[lang] :  words.infobar[lang]}}
      h2(v-else) {{ words.appbarsearch[lang] }}
      button(@click="grid = !grid")
        img(src="../assets/img/list.svg", alt="", v-show="grid === true")
        img(src="../assets/img/grid.svg", alt="", v-show="grid === false")
        span {{ grid ? words.list[lang] : words.grid[lang] }}
    .notes-content(:class="{ active: !grid }")
      OneNote(
        :lang="lang"
        :grid="grid",
        v-for="note in notes",
        :key="note.id",
        :note="note",
        @remove="$emit('remove', note.id)"
        @changeNote="$emit('changeNote', note.id)"
      )
</template>
<script>
import OneNote from "./OneNote.vue";

export default {
  components: {
    OneNote,
  },
  data() {
    return {
      grid: true,
    };
  },
  props: {
    notes: {
      typeof: Array,
      required: true,
    },
    lang: String,
    search: String
  },
  inject: ['words']

}

</script>
<style lang="scss">
.notes {
  margin-top: 30px;

  &-top {
    margin-bottom: 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;

    h2 {
      font-size: 22px;
      font-weight: 400;
    }

    button {
      background: linear-gradient(0deg,
          rgba(103, 80, 164, 0.11),
          rgba(103, 80, 164, 0.11)),
        #fffbfe;
      box-shadow: 0px 4px 8px 3px rgba(0, 0, 0, 0.15),
        0px 1px 3px rgba(0, 0, 0, 0.3);
      border-radius: 16px;
      padding: 21px 20px;
      color: #6750a4;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 121px;
      height: 56px;
      border: none;
      outline: none;
      gap: 15px;
      cursor: pointer;

      span {
        font-size: 14px;
        font-family: RM;
      }
    }
  }

  &-content {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 24px;

    @media (max-width: 700px) {
      grid-template-columns: repeat(2, 1fr);
    }

    &.active {
      grid-template-columns: 1fr;
    }
  }
}
</style>