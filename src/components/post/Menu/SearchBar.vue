<script setup>
import { defineProps, defineEmits } from 'vue';

defineProps({
  searchInput: {
    type: String,
    default: ''
  }
});

const emit = defineEmits(['update:searchInput', 'performSearch']);

const handleInput = (event) => {
  emit('update:searchInput', event.target.value);
};

const handleSearch = () => {
  emit('performSearch');
};
</script>

<template>
  <div class="search-wrap">
    <div class="search-box">
      <input
          :value="searchInput"
          @input="handleInput"
          @keyup.enter="handleSearch"
          autocomplete="off"
          type="text"
          placeholder="검색어를 입력하세요"
      />
      <button aria-label="search" type="button" @click="handleSearch">
        <img src="../../../assets/icon/searchIcon.svg" alt=""/>
      </button>
    </div>
  </div>
</template>

<style scoped>
.search-wrap {
  display: flex;
  gap: 10px;
}

.search-box {
  width: 100%;
  height: 40px;
  padding: 0px 20px;
  background-color: rgb(255, 255, 255);
  position: relative;
  border-radius: 100px;
  border: 1px solid #e06139;
  transition: 0.5s;
  margin-bottom: 10px;


  input {
    width: 100%;
    box-sizing: border-box;
    padding: 0px 40px 0px 44px;
    height: 40px;
    border: none;
    color: rgb(0, 0, 0);
    position: absolute;
    right: 0px;
    background-color: transparent;
    font-size: 16px;
    line-height: 24px;
  }

  button {
    position: absolute;
    left: 10px;
    top: 8px;
  }
}

.post-write {
  background-color: #e06139;
  border-radius: 5px;
  width: 130px;
  height: 100%;
  padding: 10px;
  text-align: center;

  a {
    color: #fff;
  }

  &:hover {
    background-color: #e0613976;
  }
}
</style>