<template>
  <user-card-list :user-list="userList"/>
  <!-- 搜索提示 -->
  <van-empty v-if="!userList || userList.length < 1" image="search" description="未匹配到用户" />
</template>

<script setup>
import {onMounted, ref} from 'vue';
import {useRoute} from "vue-router";
import myAxios from "../plugins/myAxios.ts";
import {Toast} from "vant";
import 'vant/es/toast/style';
import qs from 'qs';
import UserCardList from "../components/UserCardList.vue";

const route = useRoute();
const {tags} = route.query;
const userList = ref([]);

onMounted(async () => {
  const userListData = await myAxios.get('/user/search/tags', {
    params: {
      tagNameList: tags
    },
    paramsSerializer: params => {
      return qs.stringify(params, { indices: false })
    }
  })
  .then(function (response) {
    console.log('/user/search/tags succeed', response);
    Toast.success('请求成功');
    return response?.data;
  })
  .catch(function (error) {
    console.error('/user/search/tags error', error);
    Toast.fail('请求失败');
  });
  if (userListData) {
    userListData.forEach(user => {
      if (user.tags) {
        user.tags = JSON.parse(user.tags);
      }
    })
    userList.value = userListData;
    console.log(userListData);
  }
})

// const mockUser = {
//   id: 123,
//   username: 'yizai',
//   userAccount: '213',
//   avatarUrl: 'https://s2.loli.net/2022/08/13/nyW9ViMBht85GeJ.png',
//   gender: 0,
//   userProfile: '一名困窘小哥，目前还在踌躇，阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴阿巴',
//   phone: '18923782838',
//   email: '32178433@qq.com',
//   userRole: 0,
//   planetCode: '123',
//   tags: ['java', 'emo', '打工中', 'emo', '打工中'],
//   createTime: new Date(),
// }

</script>

<style scoped>

</style>