<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header"> default header </slot>
          </div>
          <div class="modal-body">
            <small class="section_title">Title</small>
            <input v-model="input_title" class="title" type="text" />

            <div class="input_section">
              <div class="is-vertical">
                <small class="section_title">Link</small>
                <input v-model="input_link" class="link" type="text" />
                <small class="section_title">Comment</small
                ><textarea
                  v-model="input_comment"
                  class="comment"
                  cols="50"
                  rows="9"
                ></textarea>
              </div>
              <div class="is-vertical">
                <div id="app">
                  <!-- v-date-picker -->
                </div>
                <div>
                  <small class="section_title">co-doer</small
                  ><input class="co-doer" type="text" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script setup>
import { ref } from "vue";

const input_title = ref("");
const input_link = ref("");
const input_comment = ref("");
const coDoer_list = [];
</script>
<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 50vh;
  height: 30vh;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  color: black;
}

input {
  display: block;
  width: 300px;
  height: 50px;
  margin: 3px;
}
.input_section {
  display: flex;
}
.section_title {
  display: flex;
}
</style>
