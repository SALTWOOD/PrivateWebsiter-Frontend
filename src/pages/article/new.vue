<template>
    <HeaderImage v-if="article.background" :title="article.title" :description="article.description" :background="article.background" />
    <br />
    <div :class="{ 'dark-mode': isDarkMode }">
        <h1>创建新文章</h1>
        <form @submit.prevent="submitArticle" class="article-form">
            <div class="form-group">
                <label for="title">文章标题</label>
                <v-text-field v-model="article.title" id="title" label="请输入文章标题" required />
            </div>

            <div class="form-group">
                <label for="title">文章描述</label>
                <v-text-field v-model="article.description" id="description" label="请输入文章描述" required />
            </div>

            <label for="background">文章内容</label>
            <ManagedEditor :article="article" :title="article.title" />
            <br />

            <div class="form-group">
                <label for="background">背景图片 URL</label>
                <v-text-field v-model="article.background" id="background" label="请输入背景图片的 URL" required />
            </div>

            <div class="form-group">
                <label for="published">是否发布</label>
                <v-checkbox v-model="article.published" id="published" :label="article.published ? '已发布' : '未发布'" />
            </div>

            <v-btn type="submit" color="primary" class="submit-button">提交</v-btn>

            <div v-if="error" class="error-message">{{ error }}</div>
            <div v-if="success" class="success-message">文章创建成功！</div>
        </form>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
import { useTheme } from 'vuetify';
import { Shared } from '@/types/Shared';
import { Article } from '@/types/Article';
import HeaderImage from '@/components/HeaderImage.vue';

const error = ref('');
const success = ref(false);
const article = ref<Article>(new Article());

// 获取 Vuetify 的主题信息
const theme = useTheme();
const isDarkMode = computed(() => theme.current.value.dark); // 是否深色模式

const router = useRouter();

async function submitArticle() {
    try {
        const newArticle = {
            title: article.value.title,
            content: article.value.content,
            description: article.value.description,
            background: article.value.background,
            published: article.value.published,
        };

        const response = await axios.post('/api/articles', newArticle);

        success.value = true;
        error.value = ''; // Reset error message

        // Redirect to the article page or to a list of articles after success
        router.push(`/article/${response.data.id}`);
    } catch (err) {
        error.value = '创建文章失败，请稍后再试。';
        success.value = false; // Reset success message
    }
}

onMounted(() => {
    document.title = `创建新文章 - ${Shared.info.value.title}`;
});
</script>

<style scoped>
.new-article-page {
    max-width: 1000px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f9f9f9;
    /* 浅色模式背景 */
    color: #333;
    /* 浅色模式文字颜色 */
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s, color 0.3s;
}

.new-article-page.dark-mode {
    background-color: #121212;
    /* 深色模式背景 */
    color: #f0f0f0;
    /* 深色模式文字颜色 */
}

h1 {
    text-align: center;
    margin-bottom: 20px;
}

.article-form {
    display: flex;
    flex-direction: column;
}

.form-group {
    margin-bottom: 15px;
}

.submit-button {
    background-color: #4CAF50;
    color: white;
    border: none;
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s;
}

.submit-button:hover {
    background-color: #45a049;
}

.submit-button.dark-mode {
    background-color: #66bb6a;
    /* 深色模式按钮颜色 */
}

.error-message {
    color: red;
    text-align: center;
    margin-top: 15px;
}

.success-message {
    color: green;
    text-align: center;
    margin-top: 15px;
}
</style>

<route lang="yaml">
meta:
  layout: appbar
</route>