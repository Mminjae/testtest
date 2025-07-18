<script setup>
import { computed } from "vue";

const props = defineProps({
  news: {
    type: Object,
    required: true,
  },
  isBookmarked: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["toggle-bookmark"]);

const formattedDate = computed(() => {
  const date = new Date(props.news.publishedAt);
  return date.toLocaleDateString("en-US", {
    month: "short",
    day: "numeric",
    hour: "2-digit",
    minute: "2-digit",
  });
});

const categoryColor = computed(() => {
  const colors = {
    통화정책: "#3b82f6",
    주식시장: "#10b981",
    암호화폐: "#f59e0b",
    은행업: "#8b5cf6",
    원자재: "#ef4444",
    해외시장: "#06b6d4",
  };
  return colors[props.news.category] || "#6b7280";
});

const onBookmarkClick = (event) => {
  event.stopPropagation();
  emit("toggle-bookmark");
};
</script>

<template>
  <article class="news-card" @click="$emit('click')">
    <!-- Content Section -->
    <div class="news-content">
      <div class="news-header">
        <div class="news-meta">
          <span
            class="category-tag"
            :style="{ backgroundColor: categoryColor }"
          >
            {{ news.category }}
          </span>
          <button
            @click="onBookmarkClick"
            class="bookmark-btn"
            :class="{ bookmarked: isBookmarked }"
            :title="isBookmarked ? '북마크에서 제거' : '북마크에 추가'"
          >
            {{ isBookmarked ? "⭐" : "☆" }}
          </button>
        </div>
        <div class="source-date">
          <span class="news-source">{{ news.source }}</span>
          <span class="news-date">{{ formattedDate }}</span>
        </div>
      </div>

      <h3 class="news-title">{{ news.title }}</h3>

      <p class="news-summary">{{ news.summary }}</p>

      <div class="news-footer">
        <span class="news-author">작성자: {{ news.author }}</span>
        <span class="read-more">더보기 →</span>
      </div>
    </div>
  </article>
</template>

<style scoped>
.news-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow:
    0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
  position: relative;
}

.news-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #667eea 0%, #764ba2 50%, #667eea 100%);
  border-radius: 16px 16px 0 0;
}

:root.dark .news-card {
  background: #374151;
  border-color: #4b5563;
  box-shadow:
    0 4px 6px -1px rgba(0, 0, 0, 0.3),
    0 2px 4px -1px rgba(0, 0, 0, 0.2);
}

.news-card:hover {
  transform: translateY(-8px);
  box-shadow:
    0 20px 25px -5px rgba(0, 0, 0, 0.1),
    0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

:root.dark .news-card:hover {
  box-shadow:
    0 20px 25px -5px rgba(0, 0, 0, 0.4),
    0 10px 10px -5px rgba(0, 0, 0, 0.2);
}

/* Content Section */
.news-content {
  padding: 1.5rem;
}

.news-header {
  margin-bottom: 1.5rem;
}

.news-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.category-tag {
  background: #3b82f6;
  color: white;
  padding: 0.4rem 0.8rem;
  border-radius: 16px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.bookmark-btn {
  background: #f3f4f6;
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

:root.dark .bookmark-btn {
  background: #4b5563;
}

.bookmark-btn:hover {
  background: #e5e7eb;
  transform: scale(1.1);
}

:root.dark .bookmark-btn:hover {
  background: #6b7280;
}

.bookmark-btn.bookmarked {
  background: #fbbf24;
  color: white;
}

.source-date {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.85rem;
}

.news-source {
  color: #3b82f6;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

:root.dark .news-source {
  color: #60a5fa;
}

.news-date {
  color: #6b7280;
}

:root.dark .news-date {
  color: #9ca3af;
}

.news-title {
  font-size: 1.3rem;
  font-weight: 700;
  line-height: 1.4;
  margin-bottom: 1rem;
  color: #111827;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  transition: color 0.3s ease;
}

.news-card:hover .news-title {
  color: #667eea;
}

:root.dark .news-title {
  color: #f9fafb;
}

.news-summary {
  color: #4b5563;
  line-height: 1.6;
  margin-bottom: 1.5rem;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

:root.dark .news-summary {
  color: #d1d5db;
}

.news-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 1rem;
  border-top: 1px solid #e5e7eb;
}

:root.dark .news-footer {
  border-top-color: #4b5563;
}

.news-author {
  color: #6b7280;
  font-size: 0.9rem;
  font-style: italic;
}

:root.dark .news-author {
  color: #9ca3af;
}

.read-more {
  color: #3b82f6;
  font-weight: 600;
  font-size: 0.9rem;
  transition: color 0.3s ease;
}

:root.dark .read-more {
  color: #60a5fa;
}

.news-card:hover .read-more {
  color: #1d4ed8;
}

:root.dark .news-card:hover .read-more {
  color: #3b82f6;
}

/* Responsive Design */
@media (max-width: 768px) {
  .news-content {
    padding: 1.25rem;
  }

  .news-title {
    font-size: 1.1rem;
  }

  .news-summary {
    font-size: 0.95rem;
  }

  .category-tag {
    font-size: 0.7rem;
    padding: 0.35rem 0.7rem;
  }

  .bookmark-btn {
    width: 32px;
    height: 32px;
    font-size: 1rem;
  }

  .source-date {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.25rem;
  }
}

@media (max-width: 480px) {
  .news-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.5rem;
  }

  .news-footer {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.5rem;
  }
}
</style>
