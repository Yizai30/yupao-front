<template>
  <van-form @submit="onSubmit">
    <van-field
        v-model="editUser.currentValue"
        :name="editUser.editKey"
        :label="editUser.editName"
        :placeholder="`请输入${editUser.editName}`"
    />
    <div style="margin: 16px;">
      <van-button round block type="primary" native-type="submit">
        提交
      </van-button>
    </div>
  </van-form>
</template>

<script setup lang="ts">
import {useRoute, useRouter} from "vue-router";
import {onMounted, ref} from "vue";
import myAxios from "../plugins/myAxios";
import {Toast} from "vant";
import 'vant/es/toast/style';
import {getCurrentUser} from "../services/user";
import {setCurrentUserState} from "../states/user";

const route = useRoute();
const router = useRouter();
const editUser = ref({
  editKey: route.query.editKey,
  editName: route.query.editName,
  currentValue: route.query.currentValue
})

const onSubmit = async () => {
  const currentUser = await getCurrentUser();
  if (!currentUser) {
    Toast.fail('用户未登录');
    return;
  }

  console.log(currentUser, '当前用户');

  const res = await myAxios.post("/user/update", {
    // todo 添加用户登录功能后，使用当前登录用户的 id
    'id': currentUser.id,
    [editUser.value.editKey as string]: editUser.value.currentValue
  });
  console.log(res, '更新请求');
  if (res.code === 0 && res.data > 0) {
    Toast.success('修改成功');
    const {data} = await myAxios.get('/user/current');
    setCurrentUserState(data);
    router.back();
  } else {
    Toast.fail('修改错误');
  }
};
</script>

<style scoped>

</style>