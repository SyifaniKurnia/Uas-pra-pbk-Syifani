<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated :style="'background-color: transparent'">
      <q-toolbar>
        <q-toolbar-title>

        </q-toolbar-title>
        <div class="q-gutter-md row justify-center">
          <q-btn label="Widget" to="/" flat />
          <q-btn label="Tugas 1" to="/tugas1" flat />
          <q-btn label="Tugas 2" to="/tugas2" flat />
          <q-btn label="Tugas 3" to="/tugas3" flat />
          <q-btn label="Tugas 4" to="/tugas4" flat />
          <q-btn label="Tugas 5" to="/tugas5" flat />
          <q-btn label="Tugas 6" to="/tugas6" flat />
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <transition
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
        mode="out-in"
      >
        <router-view />
      </transition>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import anime from 'animejs'

function beforeEnter(el) {
  el.style.opacity = 0
  el.style.transform = 'translateX(100%)'
}

function enter(el, done) {
  anime({
    targets: el,
    opacity: [0, 1],
    translateX: ['100%', '0%'],
    duration: 500,
    easing: 'easeInOutQuad',
    complete: done,
  })
}

function leave(el, done) {
  anime({
    targets: el,
    opacity: [1, 0],
    translateX: ['0%', '-100%'],
    duration: 500,
    easing: 'easeInOutQuad',
    complete: done,
  })
}
</script>

