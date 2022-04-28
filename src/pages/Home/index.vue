<template>
  <TheHeader />
  <textarea v-model="authCode"></textarea>
  <!-- <button btn @click="toStore">兑换商城</button>-->
  <p text-center py-3></p>
    <section  flex-c gap-2>
   <button btn @click="getAuthCode">获取授权code</button>
   </section>
   <TheTabbar />
  <section flex-c gap-2>
    <UPButton
      :timeout="2000"
      class="customBtn"
      scope="scope.mobile"
      text="授权mobile"
      btn
      @click="btnClick"
    />
    <UPButton
      :timeout="2000"
      class="customBtn"
      scope="scope.auth"
      text="授权auth"
      btn
      @click="btnClick"
    />
    <button btn @click="wakeShare">share</button>
  </section>
  <br/>

</template>

<script setup lang="ts">
  import { Toast } from 'vant';

  const router = useRouter();

  function toStore() {
    router.push({
      name: 'Store',
    });
  }

 const authCode = ref<string>('');

  function getAuthCode() {
    upsdk.pluginReady(function () {
      upsdk.appletAuth({
        success: function (data: any) {
          authCode.value = data.code;
        },
        fail: function (error: any) {
          authCode.value = error.errmsg;
        },
      });
    });
  }

  function btnClick(isSuccess: boolean, result: any) {
    console.log(result);
    if (!isSuccess) {
      Toast.fail(result.errmsg);
    } else {
      Toast.success('授权成功');
    }
  }

  function wakeShare() {
    console.log(111);
    upsdk.pluginReady(() => {
      upsdk.showFlashInfo({
        msg: '测试支付成功！',
      });
      upsdk.pay({
        tn: 'that.message',
        success: () => {
          // 支付成功, 开发者执行后续操作。
          upsdk.showFlashInfo({
            msg: '测试支付成功！',
          });
        },
        fail: (err) => {
          // 支付失败, err.msg 是失败原因描述, 比如TN号不合法, 或者用户取消了交易 等等。
          upsdk.showFlashInfo({
            msg: err.msg,
          });
        },
      });
    });
  }
</script>

<style scoped lang="scss"></style>

<style lang="scss" scoped></style>
