<script setup>
import { watch, defineProps } from 'vue';

const props = defineProps({
  freeResults: {
    type: Array,
    default: () => []
  },
  openResults: {
    type: Array,
    default: () => []
  },
  marketResults: {
    type: Array,
    default: () => []
  },
  marketPostSize: {
    type: Number,
    default: 3
  },
  searchQuery: {
    type: String,
    default: ''
  }
});

watch(() => props.freeResults, (newVal) => {
  console.log('Free Results:', newVal);
});

watch(() => props.openResults, (newVal) => {
  console.log('Open Results:', newVal);
});

watch(() => props.marketResults, (newVal) => {
  console.log('Market Results:', newVal);
  console.log('Market Results Length:', newVal.length);
  console.log('Market Post Size:', props.marketPostSize);
});
</script>

<template>
  <div class="search-result">
    <div class="search-result">
      <h2>자유 게시판</h2>
      <div>
        <div v-if="freeResults.length === 0">검색 결과가 없습니다</div>
        <div v-else class="search-result-list">
          <div v-for="result in freeResults" :key="result.id" class="search-result-item">
            <router-link :to="`/free/detail/${result.idx}`">
              <h3>{{ result.title }}</h3>
              <p>{{ result.content || '내용이 없습니다' }}</p>
            </router-link>
          </div>
          <div class="search-more-btn">
            <router-link :to="{ name: 'FreeListPage', query: { q: searchQuery } }" class="btn">더보기</router-link>
          </div>
        </div>
      </div>
    </div>
    <hr>
    <div class="search-result">
      <h2>공개 게시판</h2>
      <div>
        <div v-if="openResults.length === 0">검색 결과가 없습니다</div>
        <div v-else class="search-result-list">
          <div v-for="result in openResults" :key="result.id" class="search-result-item">
            <router-link :to="`/open/detail/${result.idx}`">
              <h3>{{ result.title }}</h3>
              <p>{{ result.content || '내용이 없습니다' }}</p>
            </router-link>
          </div>
          <div class="search-more-btn">
            <router-link :to="{ name: 'OpenListPage', query: { q: searchQuery } }" class="btn">더보기</router-link>
          </div>
        </div>
      </div>
    </div>
    <hr>
    <div class="search-result">
      <h2>장터 게시판</h2>
      <div>
        <div v-if="marketResults.length === 0">검색 결과가 없습니다</div>
        <div v-else class="search-result-list">
          <div v-for="result in marketResults.slice(0, marketPostSize)" :key="result.idx" class="search-result-item">
            <router-link :to="`/market/detail/${result.idx}`">
              <h3>{{ result.title }}</h3>
<!--              <p>{{ result.content || '내용이 없습니다' }}</p>-->
            </router-link>
          </div>
          <div class="search-more-btn">
            <router-link :to="{ name: 'MarketListPage', query: { q: searchQuery } }" class="btn">더보기</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
h2 {
  margin-top: 0;
  border-radius: 5px;
}

.search-result {
  padding: 10px;
  background-color: rgba(191, 184, 166, 0.2);
  border-radius: 10px;
}

.search-result-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.search-result-item {
  background-color: #ffffff8a;
  padding: 10px;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  gap: 5px;
}

h3 {
  margin: 0;
}

p {
  margin: 0;
}

.title {
  display: flex;
  background-color: rgb(224 97 57);
  color: #fff;
  border-radius: 0.75rem;
  height: 4rem;
  padding: 0 2rem;
  width: 100%;
  box-sizing: border-box;
  align-items: center;
  box-shadow: 2px 2px 10px rgb(0 0 0 / 10%);
}
</style>