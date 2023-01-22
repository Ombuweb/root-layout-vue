<template>
  <Page>
    <ActionBar>
      <Label text="Home" class="font-bold text-lg" />
    </ActionBar>
    <ScrollView>
      <GridLayout rows="auto,*" @loaded="onGridLoaded">
        <RootLayout row="1"> </RootLayout>
        <FlexboxLayout flexDirection="row" justifyContent="space-between">
          <Button flexGrow="1" text="open" @tap="open" popupIndex="1" />
          <Button flexGrow="1" text="front" @tap="bringToFront" popupIndex="1" />
          <Button flexGrow="1" text="close" @tap="close" popupIndex="1" />

        </FlexboxLayout>
      </GridLayout>
    </ScrollView>
  </Page>
</template>

<script lang="ts">
import Vue, { NativeScriptVue, NativeScriptVueConstructor } from 'nativescript-vue';
import { Button, CoreTypes, Dialogs, EventData, Frame, getRootLayout, GridLayout, ListView, RootLayoutOptions, Screen, StackLayout, Utils, View } from '@nativescript/core';
import AboutView from "./About.vue"
import GreetingView from "./Greetings.vue"

import { } from "vue"
interface IData {
  popupViews: { view: View; options: RootLayoutOptions; extra?: any }[];
  currentView: View;
}
interface IMethods {
  open: (args: EventData) => void;
  bringToFront: (args: EventData) => void;
  close: (args: EventData) => void;
  extractViewFromVue: () => { view: View, id: number };
  openModal: () => void;
  getPopup: (color: string, size: number, offset: number) => View;
  onGridLoaded: (args: EventData) => void;
}
export default Vue.extend<IData, IMethods, {}, {}>({
  data() {
    return {
      popupViews: [],
      currentView: new Button() as View
    };
  },
  mounted() {
    this.popupViews = [{
      view: this.getPopup('#EA5936', 110, -30),
      options: {
        shadeCover: {
          color: 'linear-gradient(to bottom, red, blue)',
          opacity: 0.7,
          tapToClose: true,
        },
        animation: {
          enterFrom: {
            opacity: 0,
            translateX: -100,
            duration: 500,
          },
          exitTo: {
            opacity: 0,
            duration: 300,
            translateX: -100
          },
        },
      },
      extra: {
        customExitAnimation: {
          opacity: 0,
          translate: { x: 0, y: -500 },
        },
      },
    },
    {
      view: this.getPopup('#232652', 110, 0),
      options: {
        shadeCover: {
          color: 'pink',
          opacity: 0.7,
          tapToClose: false,
          animation: {
            exitTo: {
              scaleX: 0,
            },
          },
        },
      },
    },
    {
      view: this.getPopup('#E1E4E8', 110, 30),
      options: {
        shadeCover: {
          color: '#ffffdd',
          opacity: 0.5,
          tapToClose: true,
          ignoreShadeRestore: true,
          animation: {
            enterFrom: {
              translateX: -1000,
              duration: 500,
            },
            exitTo: {
              rotate: -180,
              duration: 500,
            },
          },
        },
        animation: {
          enterFrom: {
            rotate: 180,
            duration: 300,
          },
          exitTo: {
            rotate: 180,
            opacity: 0,
            duration: 300,
            curve: CoreTypes.AnimationCurve.spring,
          },
        },
      },
    },
    ]
    this.extractViewFromVue()
  },
  // components: {
  //   AboutView,
  // },
  methods: {
    onGridLoaded(args: EventData) {

      // console.dir((<GridLayout>args.object).ios)
      // for (const key in (<GridLayout>args.object).ios) {
      //   //console.log(key)
      // }
      // console.log("GridLoaded: ", Object.keys((<GridLayout>args.object).ios))
    },
    open(args: EventData): void {
      const shadeCover = getRootLayout().getShadeCover()
      getRootLayout()
        .open(
          this.currentView,
          this.popupViews[0].options
          //this.popupViews[(<any>args.object).popupIndex].options
        )
        .catch((ex) => console.error(ex));
    },
    bringToFront(args: EventData): void {
      getRootLayout()
        .bringToFront(this.popupViews[(<any>args.object).popupIndex].view, true)
        .catch((ex) => console.error(ex));
    },
    close(args: EventData): void {

      if (
        this.popupViews[(<any>args.object).popupIndex]?.extra
          ?.customExitAnimation
      ) {
        getRootLayout()
          .close(

            this.currentView
            //this.popupViews[(<any>args.object).popupIndex].view,
            //this.popupViews[(<any>args.object).popupIndex].extra
            //.customExitAnimation
          )
          .catch((ex) => console.error(ex));
      } else {
        getRootLayout()
          .close(this.currentView)
          .catch((ex) => console.error(ex));
      }
    },
    getPopup(color: string, size: number, offset: number): View {

      const layout = new StackLayout();
      layout.height = size;
      layout.width = size;
      layout.marginTop = offset;
      layout.marginLeft = offset;
      layout.backgroundColor = color;
      layout.borderRadius = 10;

      return layout;
    },
    extractViewFromVue() {
      // const frame = new CGRect()
      // const dPicker = UIDatePicker.alloc().initWithFrame(frame)
      // dPicker.frame.size

      var navEntryInstance = new Vue({
        name: 'ModalEntry',
        render: function (h) {
          return h(AboutView)
        }
      });

      var modalPage = (navEntryInstance.$mount().$el as any).nativeView as View;
      this.currentView = modalPage
      // const test =  navEntryInstance.$children[0].$mount().$el;
      //console.log(Utils.getClass(test),test)
      return { view: modalPage, id: modalPage._domId }
    },
    openModal() {
      this.$showModal(GreetingView).then((value: string) => {
        Dialogs.alert(value)
      })
    }
  },
});
</script>

<style>
/* .info {
    font-size: 20;
  } */
</style>
