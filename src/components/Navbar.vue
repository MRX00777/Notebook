<template>
  <header class="header">
    <Transition name="header-change" mode="out-in">
      <div class="header-main" v-if="header">
        <!-- <div class="logo"><img src="../assets/img/Background@2x.png" alt="" style="width: 50px; right: 50px;"></div> -->
        <button class="header-main__lang" @click="changeLang" v-if="lang == 'uz'">RU</button>
        <button class="header-main__lang" @click="changeLang" v-else>UZ</button>
        <h1 class="header-main__title">{{ words.appbartitle[lang] }}</h1>
        <button class="header-main__search" @click="header = false">
          <img src="../assets/img/search.svg" alt="" />
        </button>
      </div>
      <div class="header-form" v-else>
        <button
          class="header-form__back"
          @click="(header = true), (search = '')"
        >
          <img src="../assets/img/back.svg" alt="" />
        </button>
        <input
          type="text"
          class="header-form__search"
          v-model="search"
          :placeholder= "words.appbarsearch[lang]" 
        />
        <button class="header-form__reset" @click="search = ''">
          <img src="../assets/img/reset.svg" alt="" />
        </button>
      </div>
    </Transition>
  </header>
</template>
<script>
export default {
  data() {
    return {
      header: true,
      search: '',
    };
  },
  methods: {
    changeLang(){
      this.$emit('changeLang', this.lang == 'ru' ? 'uz' : 'ru')
  }
  },
  watch: {
    search(val){
      this.$emit('find', val)
    }
  },
  props: {
    lang: String
  },
  inject: ['words']
  
};
</script>

<style lang="scss">
.header {
  background: rgb(41 41 41);
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25), 0px 1px 3px rgba(0, 0, 0, 0.3);
  padding: 18px 21px;
  height: 64px;
  
  &-main,
  &-form {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: white;
    &__lang,
    &__search,
    &__back,
    &__reset {
      font-size: 20px;
      color: white;
      font-weight: 700;
      outline: none;
      border: none;
      background: none;
      cursor: pointer;
    }
    &__title {
      font-weight: 400;
      font-size: 22px;
      color: white;
    }
  }
  &-form__search {
    width: 90%;
    font-size: 16px;
    color: #9d9d9d;
    font-weight: 400;
    cursor: text;
    color: white;
  }
  .header-change-enter-active,
  .header-change-leave-active {
    transition: 0.2s linear;
    
  }
  .header-change-enter-from,
  .header-change-leave-to {
    opacity: 0;
    transform: translateY(-100%);
  }
  
}
</style>