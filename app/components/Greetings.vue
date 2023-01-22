<template>
    <StackLayout>
      <Button text="close" @tap="onModalClose"/>
      
    </StackLayout>
</template>
<script lang="ts">
import Vue from "nativescript-vue";
export default Vue.extend({
methods:{
    onModalClose(){
        this.$modal?.close("Hi! I'm from modal!")
    }
}
})
</script>