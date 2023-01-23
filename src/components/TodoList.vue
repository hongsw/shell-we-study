<template>
  <button class="add_btn" @click="clickAddTodo">+ NEW</button>
  <button class="remove_btn" @click="clickRemoveTodo">REMOVE</button>
  <ag-grid-vue
    class="ag-theme-alpine"
    :columnDefs="gridOptions.columnDefs"
    :rowData="gridOptions.rowData"
    :defaultColDef="gridOptions.defaultColDef"
    :gridOptions="gridOptions"
    @grid-ready="onGridReady"
  >
  </ag-grid-vue>

  <AddTodoModal
    v-if="showModal"
    @close="showModal = false"
    @addNewOne="addNewTodo"
  >
  </AddTodoModal>
</template>

<script setup>
import AddTodoModal from "./AddTodoModal.vue";
import { ref } from "vue";

import "ag-grid-community/styles//ag-grid.css";
import "ag-grid-community/styles//ag-theme-alpine.css";
import { AgGridVue } from "ag-grid-vue3";
import "ag-grid-enterprise";
// import { LicenseManager } from "ag-grid-enterprise";
// LicenseManager.setLicenseKey("info@ag-grid.com");

const showModal = ref(false);

const myRowData = [
  // [DATA-EXAMPLE]
  {
    title: "TOEIC",
    link: "https://www.toeic.co.kr/",
    coDoers: "jihong, ji, hong",
    due: "Jan/30/2023",
    understanding: "Not yet",
  },
  {
    title: "TOEIC",
    link: "https://www.toeic.co.kr/",
    coDoers: "jihong, ji, hong",
    due: "Jan/31/2023",
    understanding: "Not yet",
  },
];
const myColumnDefs = [
  { headerName: "Title", field: "title", suppressMenu: true },
  { headerName: "Link", field: "link", suppressMenu: true },
  { headerName: "Co-doer", field: "coDoers", suppressMenu: true },
  { headerName: "Due", field: "due", suppressMenu: true },
  {
    headerName: "Understanding",
    field: "understanding",
    editable: true,
    cellEditor: "agRichSelectCellEditor",
    cellEditorPopup: true,
    cellEditorParams: {
      values: ["Not yet", "Doing", "Understanding"],
    },
    suppressMenu: true,
  },
];

const gridApi = null;
const columnApi = null;
const gridOptions = {
  rowData: myRowData,
  columnDefs: myColumnDefs,
  defaultColDef: {
    resizable: true,
  },
};

const onGridReady = (params) => {
  gridApi = params.api;
  params.api.sizeColumnsToFit();
};

const addNewTodo = (input_data) => {
  showModal.value = false;

  if (input_data.title.value != "") {
    const date_list = input_data.due.value.toString().split(" ").slice(1, 4);
    const tmp = {
      title: input_data.title.value,
      link: input_data.link.value,
      comment: input_data.link.value,
      due: date_list.join("/"),
      coDoers: input_data.coDoers.join(),
      understanding: "Not yet",
    };
    myRowData.push(tmp);
  }
  gridOptions.api.setRowData(myRowData);
};

const clickRemoveTodo = () => {
  const selectedRow = gridOptions.api.getFocusedCell();
  myRowData.splice(selectedRow.rowIndex, 1);
  gridOptions.api.setRowData(myRowData);
};

const clickAddTodo = () => {
  showModal.value = !showModal.value;
};
</script>
<style scoped>
.ag-theme-alpine {
  width: 70vh;
  height: 50vh;
}
.add_btn {
  display: flex;
}
.remove_btn {
  display: flex;
}
</style>
