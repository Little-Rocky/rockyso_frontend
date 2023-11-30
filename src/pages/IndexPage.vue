<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
    <MyDivider />
    <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
      <a-tab-pane key="post" tab="文章">
        <PostList />
      </a-tab-pane>
      <a-tab-pane key="picture" tab="图片">
        <PictureList />
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户">
        <UserList />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import PictureList from "@/components/PictureList.vue";
import UserList from "@/components/UserList.vue";
import MyDivider from "@/components/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";

// 默认文章页
const router = useRouter();
// 获取当前页面的路由信息
const route = useRoute();
const activeKey = route.params.category;

// 初始值
const initSearchParams = {
  text: "",
  // 增加页码
  pageSize: 10,
  pageNum: 1,
};

// 抽象成一个类，来记搜索条件
const searchParams = ref(initSearchParams);

// 监听路由信息
watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text,
  } as any;
});

// 方法实现
const onSearch = (value: string) => {
  alert(value);
  router.push({
    query: searchParams.value,
  });
};

// 根据标签改变路由参数方法实现
const onTabChange = (key: string) => {
  router.push({
    // 路由参数
    path: `/${key}`,
    query: searchParams.value,
  });
};
</script>
