<template>
  <button class="add_btn" @click="clickAddTodo">+ NEW</button>
  <ag-grid-vue
    class="ag-theme-alpine"
    :columnDefs="gridOptions.columnDefs"
    :rowData="gridOptions.rowData"
    :defaultColDef="gridOptions.defaultColDef"
    :gridOptions="gridOptions"
    @grid-ready="onGridReady"
    @cell-clicked="clickRowDelete"
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
    due: "2023-01-30",
    understanding: "Not yet",
    del_btn: "❌",
  },
  {
    title: "TOEIC",
    link: "https://www.toeic.co.kr/",
    coDoers: "jihong, ji, hong",
    due: "2023-01-31",
    understanding: "Not yet",
    del_btn: "❌",
  },
];
const myColumnDefs = [
  { headerName: "Title", field: "title", suppressMenu: true },
  { headerName: "Link", field: "link", suppressMenu: true },
  { headerName: "Co-doer", field: "coDoers", suppressMenu: true },
  { headerName: "Due", field: "due", suppressMenu: true, sortable: true },
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
  { headerName: "", field: "del_btn", suppressMenu: true },
];

const gridApi = null;
const columnApi = null;
const gridOptions = {
  rowData: myRowData,
  columnDefs: myColumnDefs,
  defaultColDef: {
    flex: 1,
    resizable: true,
  },
};

const onGridReady = (params) => {
  gridApi = params.api;
  params.api.sizeColumnsToFit();
};

const clickAddTodo = () => {
  showModal.value = !showModal.value;
};

const convertMonthData = (month) => {
  switch (month) {
    case "Jan":
      month = "01";
      break;
    case "Feb":
      month = "02";
      break;
    case "Mar":
      month = "03";
      break;
    case "Apr":
      month = "04";
      break;
    case "May":
      month = "05";
      break;
    case "Jun":
      month = "06";
      break;
    case "Jul":
      month = "07";
      break;
    case "Aug":
      month = "08";
      break;
    case "Sep":
      month = "09";
      break;
    case "Oct":
      month = "10";
      break;
    case "Nov":
      month = "11";
      break;
    case "Dec":
      month = "12";
      break;
    default:
      break;
  }
  return month;
};

const addNewTodo = (input_data) => {
  showModal.value = false;

  if (input_data.title.value != "") {
    const date_list = input_data.due.value.toString().split(" ");
    date_list[1] = convertMonthData(date_list[1]);
    date_list[2] = date_list[2].padStart(2, "0");

    const date_info = [date_list[3], date_list[1], date_list[2]];
    const tmp = {
      title: input_data.title.value,
      link: input_data.link.value,
      comment: input_data.link.value,
      due: date_info.join("-"),
      coDoers: input_data.coDoers.join(),
      understanding: "Not yet",
      del_btn: "❌",
    };
    myRowData.push(tmp);
  }
  gridOptions.api.setRowData(myRowData);
};

const clickRowDelete = (params) => {
  const col_name = params.column.colId;
  if (col_name === "del_btn") {
    params.api.applyTransaction({
      remove: [params.node.data],
    });
  }
  // 기존 todo modal 띄우기
  else {
  }
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
