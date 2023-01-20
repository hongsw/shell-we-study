<template>
  <ag-grid-vue
    class="ag-theme-alpine"
    :columnDefs="columnDefs"
    :rowData="rowData"
    :defaultColDef="defaultColDef"
    @grid-ready="onGridReady"
    @first-data-rendered="onFirstDataRendered"
    @grid-size-changed="onGridSizeChanged"
  >
  </ag-grid-vue>
</template>
<script setup>
import "ag-grid-community/styles//ag-grid.css";
import "ag-grid-community/styles//ag-theme-alpine.css";
import { AgGridVue } from "ag-grid-vue3";
import "ag-grid-enterprise";
// import { LicenseManager } from "ag-grid-enterprise";
// LicenseManager.setLicenseKey("info@ag-grid.com");

const gridApi = null;
const defaultColDef = {
  resizable: true,
};
const columnDefs = [
  { headerName: "Title", field: "title" },
  { headerName: "Link", field: "link" },
  { headerName: "Co-doer", field: "coDoer" },
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
const rowData = [
  // fetch()
  // [DATA-EXAMPLE]
  {
    title: "TOEIC",
    link: "https://www.toeic.co.kr/",
    coDoer: "jihong, ji, hong",
    due: "2023/01/31",
    understanding: "Not yet",
  },
  {
    title: "TOEIC",
    link: "https://www.toeic.co.kr/",
    coDoer: "jihong, ji, hong",
    due: "2023/01/31",
    understanding: "Not yet",
  },
];

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
  // gridApi = params.api;
  // params.api.sizeColumnsToFit();
};
</script>
<style scoped>
.ag-theme-alpine {
  width: 70vh;
  height: 50vh;
}
</style>
