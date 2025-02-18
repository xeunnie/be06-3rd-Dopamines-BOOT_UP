<script setup>
import {defineProps, defineEmits, ref, computed, nextTick, onMounted, watch} from 'vue';

const props = defineProps({
  author: {
    type: String,
    required: true
  },
  content: {
    type: String,
    required: true
  },
  createdAt: {
    type: String,
    required: true
  },
  likeCount: {
    type: Number,
    required: true
  },
  likeStatus: {
    type: Boolean,
    required: false
  },
  idx: {
    type: Number,
    required: true
  },
});

const emit = defineEmits(['delete', 'edit', 'reply', 'update:likeCount']);
const isEditing = ref(false);
const editableContent = ref(props.content);
const inputRef = ref(null);

const userNickName = computed(() => {
  const user = JSON.parse(localStorage.getItem('user'));
  return user?.userNickName || '';
});

// 로컬 상태로 postLikeCount 관리
const localLikeCount = ref(props.likeCount);
const isLiked = ref(false);

// 대댓글 삭제
function handleDelete() {
  console.log("handleDeleteReComment: ", props.idx);
  emit('delete', props.idx);
}

// 작성자와 현재 접속자 정보 확인
const canEditReComment = computed(() => {
  return userNickName.value === props.author;
});

// 권한 확인 성공하면 input 활성화 후 focus처리
function handleEdit() {
  isEditing.value = !isEditing.value;

  nextTick(() => {
    if (isEditing.value && inputRef.value) {
      inputRef.value.focus();
    }
  })
}

// 대댓글 수정
function reCommentUpdate() {
  console.log("this is reCommentUpdate section")
  console.log(props.idx)
  console.log(editableContent.value)
  emit('update', {
    idx: props.idx, // 대댓글 idx
    content: editableContent.value // 대댓글 수정 내용
  });
}

// 대댓글에서 대댓글창 활성화
function handleReply() {
  emit('reply');
}

// 날짜 년-월-일로 표현하는 함수
function formatDate(dateString) {
  const date = new Date(dateString);
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, '0'); // 월은 0부터 시작하므로 +1
  const day = String(date.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
}

// 게시글 조회시, 해당 유저가 좋아요를 눌렀던 적이 있는지 확인 후 표시
onMounted(async() => {
  const likeIcon = document.querySelector('.btn-like img');
  isLiked.value = await props.likeStatus;
  if (isLiked.value) {
    likeIcon.src = require('@/assets/icon/fill_marked.svg');
  } else {
    likeIcon.src = require('@/assets/icon/empty_marked.svg');
  }
});

// props 변경을 감지하고 로컬 상태 업데이트
watch(() => props.likeCount, (newCount) => {
  localLikeCount.value = newCount;
});

watch(() => props.likeStatus, (newStatus) => {
  isLiked.value = newStatus;
  const likeIcon = document.querySelector('.btn-like img');

  if (newStatus) {
    likeIcon.src = require('@/assets/icon/fill_marked.svg');
  } else {
    likeIcon.src = require('@/assets/icon/empty_marked.svg');
  }
});

function postLike() {
  const likeIcon = document.querySelector('.btn-like img');

  if (isLiked.value) {
    likeIcon.src = require('@/assets/icon/empty_marked.svg');
    localLikeCount.value--;
  } else {
    likeIcon.src = require('@/assets/icon/fill_marked.svg');
    localLikeCount.value++;
  }

  isLiked.value = !isLiked.value;
  emit('update:likeCount', props.idx);
}
</script>

<template>
  <div class="comment-view">
    <div class="comment-wrap">
      <div>
        <p><strong>{{ author }}</strong> - {{ formatDate(createdAt) }}</p>
      </div>
      <div v-if="isEditing" class="comment-edit-container">
        <input ref="inputRef" v-model="editableContent"/>
        <button class="comment-create-btn" @click="reCommentUpdate">수정</button>
      </div>
      <div v-else>
        <p>{{ content }}</p>
      </div>
    </div>
    <div class="comment-method-container">
      <div>
        <button @click="handleDelete" v-if="canEditReComment">삭제</button>
        <button @click="handleEdit" v-if="canEditReComment">수정</button>
        <button @click="handleReply">대댓글</button>
      </div>
      <div class="like-wrap">
        <button class="btn-like" @click="postLike">
          <img :src="isLiked ? require('@/assets/icon/fill_marked.svg') : require('@/assets/icon/empty_marked.svg')"
               alt="like icon">
        </button>
        <p class="post-count-text">{{ localLikeCount }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.comment-view {
  padding: 1rem;
  border-bottom: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
}

.comment-method-container {
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
  flex-direction: column;
}

.like-wrap {
  display: flex;
  align-items: center;

  p {
    margin: 0;
  }
}

.comment-wrap {
  width: 85%;
}

.comment-edit-container {
  display: flex;
}

.comment-create-btn {
  font-size: 1rem;
  padding: 6px 10px;
  background-color: rgba(191, 184, 166, 0.7);
  color: #212529;
  border-radius: 5px;
}

.comment-wrap input {
  width: 90%;
  font-size: 1rem;
  border: none;
  outline: none;
}

.comment-wrap input:focus {
  border-bottom: 2px solid #e06139a3;
}
</style>