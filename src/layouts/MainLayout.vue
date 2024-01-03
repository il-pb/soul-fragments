<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="header bg-primary text-white q-pa-md">
      <q-toolbar>
        <q-toolbar-title class="title text-h4">
          Soul Fragments
        </q-toolbar-title>
      </q-toolbar>

      <q-btn-group push>
        <q-btn to="/about" push label="About" icon="diamond" />
        <q-btn to="/" push label="Create a Fragment" icon="draw" />
        <q-btn to="/s-f" push label="Soul Fragments" icon="receipt" />
      </q-btn-group>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<style lang="sass">
body
  font-family: $font-stack
  
.header
  opacity: 90%
  display: flex
  flex-direction: column
  align-items: center

.title
  text-align: center

</style>