<template>
  <van-card
      v-for="user in props.userList"
      :desc="user.userProfile"
      :title="`${user.username}（${user.planetCode}）`"
      :thumb="user.avatarUrl"
  >
    <template #tags>
      <van-tag plain type="primary" v-for="tag in user.tags" style="margin-right: 6px; margin-top: 6px">
        {{tag}}
      </van-tag>
    </template>
    <template #footer>
      <van-button size="small">联系我</van-button>
    </template>
  </van-card>
</template>

<script setup lang="ts">
import {UserType} from "../models/user";

interface UserCardListProps {
  userList: UserType[];
}
const props = withDefaults(defineProps<UserCardListProps>(), {
  userList: [],
});
</script>

<style scoped>

</style>