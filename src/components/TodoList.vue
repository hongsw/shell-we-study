<template>
  <button class="add_btn" @click="clickAddTodo">+ NEW</button>
  <ag-grid-vue
    class="ag-theme-alpine"
    :columnDefs="columnDefs"
    :rowData="rowData"
    :defaultColDef="defaultColDef"
    :gridOptions="gridOptions"
    @grid-ready="onGridReady"
    @first-data-rendered="onFirstDataRendered"
    @grid-size-changed="onGridSizeChanged"
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
import { GridOptionsService } from "ag-grid-enterprise";
// import { LicenseManager } from "ag-grid-enterprise";
// LicenseManager.setLicenseKey("info@ag-grid.com");

const showModal = ref(false);
const myRowData = [
  // [DATA-EXAMPLE]
  {
    title: "TOEIC",
    link: "https://www.toeic.co.kr/",
    coDoers: "jihong, ji, hong",
    due: "Jan/31/2023",
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
  { headerName: "Title", field: "title" },
  { headerName: "Link", field: "link" },
  { headerName: "Co-doer", field: "coDoers" },
  { headerName: "Due", field: "due" },
  {
    headerName: "Understanding",
    field: "understanding",
    editable: true,
    cellEditor: "agRichSelectCellEditor",
    cellEditorPopup: true,
    cellEditorParams: {
      values: ["Not yet", "Doing", "Understanding"],
    },
  },
];

const gridApi = null;
const defaultColDef = {
  resizable: true,
};
const rowData = myRowData;

const gridOptions = {
  rowData: myRowData,
  columnDefs: myColumnDefs,
};

const onFirstDataRendered = (params) => {
  params.api.sizeColumnsToFit();
};

const onGridSizeChanged = (params) => {
  // get the current grids width
  var gridWidth = document.getElementById("grid-wrapper").offsetWidth;
  // keep track of which columns to hide/show
  var columnsToShow = [];
  var columnsToHide = [];
  // iterate over all columns (visible or not) and work out
  // now many columns can fit (based on their minWidth)
  var totalColsWidth = 0;
  var allColumns = params.columnApi.getColumns();
  if (allColumns && allColumns.length > 0) {
    for (var i = 0; i < allColumns.length; i++) {
      var column = allColumns[i];
      totalColsWidth += column.getMinWidth() || 0;
      if (totalColsWidth > gridWidth) {
        columnsToHide.push(column.getColId());
      } else {
        columnsToShow.push(column.getColId());
      }
    }
  }
  // show/hide columns based on current grid width
  params.columnApi.setColumnsVisible(columnsToShow, true);
  params.columnApi.setColumnsVisible(columnsToHide, false);
  // fill out any available space to ensure there are no gaps
  params.api.sizeColumnsToFit();
};

const onGridReady = (params) => {
  gridApi = params.api;
  console.log("onGridReady");
  console.log(params.api);
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

  console.log(myRowData);
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
</style>
