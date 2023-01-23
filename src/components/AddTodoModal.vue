<template>
  <div class="modal-mask">
    <div class="modal-wrapper">
      <div class="modal-container">
        <div class="modal-header">
          <button class="save_btn" @click="saveTodo">저장</button>
        </div>
        <div class="modal-body">
          <small class="section_title">Title</small>
          <input v-model="input_title" class="title" type="text" />
          <hr />

          <div class="input_section">
            <div class="is-vertical">
              <small class="section_title">Link</small>
              <input v-model="input_link" class="link" type="text" />
              <small class="section_title">Comment</small
              ><textarea
                v-model="input_comment"
                class="comment"
                cols="50"
                rows="10"
              ></textarea>
            </div>
            <div class="is-vertical">
              <div>
                <small class="section_title">Due</small>
                <Datepicker v-model="input_Date" />
              </div>
              <div>
                <small class="section_title">co-doers</small
                ><input
                  v-model="input_coDoer"
                  @keyup.enter="addCoDoer"
                  class="co-doer"
                  type="text"
                />
                <button @click="addCoDoer">추가</button>
                <ul id="coDoer_list"></ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, defineEmits } from "vue";
import Datepicker from "vue3-datepicker";

const input_title = ref("");
const input_link = ref("");
const input_comment = ref("");
const input_Date = ref(new Date());
const input_coDoer = ref("");
const coDoer_list = [];

const input_data = {
  title: input_title,
  link: input_link,
  comment: input_comment,
  due: input_Date,
  coDoers: coDoer_list,
};

const emit = defineEmits(["addNewOne"]);

const addCoDoer = () => {
  if (input_coDoer.value != "") {
    const li = document.createElement("li");
    li.appendChild(document.createTextNode(input_coDoer.value));
    document.querySelector("#coDoer_list").appendChild(li);

    coDoer_list.push(input_coDoer.value);
    input_coDoer.value = "";
  }
};

const saveTodo = () => {
  emit("addNewOne", input_data);
};
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
  min-height: 30vh;
  height: auto;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  color: black;
}
.save_btn {
  display: flex;
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
