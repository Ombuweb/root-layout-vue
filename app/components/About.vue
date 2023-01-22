<template>
    <StackLayout ref="stacky" backgroundColor="blue" height="300" width="50%" horizontalAlignment="left">
      <Label text="Home" textWrap="true" @tap="onTap"/>
      <Label text="Products" textWrap="true" />
      
      <Label text="Log out" textWrap="true" />
      
    </StackLayout>
</template>

<script lang="ts">
import { Dialogs, EventData } from '@nativescript/core';
import Vue from 'nativescript-vue';
import GreetingView from "./Greetings.vue"
export default  Vue.extend({
  methods:{
    onTap(args:EventData){
      this.$showModal(GreetingView).then((value:string)=>{
        Dialogs.alert(value)
      })
    }
  }
})

</script>