<template>
    <button @click="deselectRows">Deselect Row</button>
    <ag-grid-vue
      class="ag-theme-alpine"
      style="height: 500px; width: 100%;"
      :columnDefs="columnDefs"
      :rowData="rowData"
      :defaultColDef="defaultColDef"
      rowSelection="multiple"
      animateRows="true"
      @cell-clicked="cellWasClicked"
      @grid-ready="onGridReady"
    >
    </ag-grid-vue>
  </template>
  
  <script>
  import { AgGridVue } from 'ag-grid-vue3';
  import { reactive, ref, onMounted } from 'vue';
  import "ag-grid-community/styles/ag-grid.css";
  import "ag-grid-community/styles/ag-theme-alpine.css";
  
  export default {
    name: 'App',
    components: {
      AgGridVue
    },
    setup() {
      // 创建对 gridApi 的引用，用来操作 ag-Grid
      const gridApi = ref(null);
  
      // 在 Grid Ready 时保存 API 引用
      const onGridReady = params => {
        gridApi.value = params.api;
      };
  
      // 定义行数据
      const rowData = reactive([
        { make: 'Vauxhall', model: 'Core', price: 1938 },
        { make: 'shsfr', model: 'resg', price: 1938 },
        { make: 'segszg', model: 'se', price: 1938 }
      ]);
  
      // 列定义（不需要用 reactive）
      const columnDefs = [
        { field: 'make' },
        { field: 'model' },
        { field: 'price' }
      ];
  
      // 默认列定义
      const defaultColDef = {
        sortable: true,
        filter: true
      };
  
      // 在组件加载时获取远程数据
      onMounted(() => {
        fetch("https://www.ag-grid.com/example-assets/row-data.json")
          .then((result) => result.json())
          .then((remoteRowData) => {
            // 更新行数据
            rowData.splice(0, rowData.length, ...remoteRowData);
          })
          .catch((error) => {
            console.error("Error fetching data:", error);
          });
      });
  
      // 行单元格点击事件处理
      const cellWasClicked = (event) => {
        console.log("Cell clicked: ", event);
      };
  
      // 取消选中所有行
      const deselectRows = () => {
        if (gridApi.value) {
          gridApi.value.deselectAll();
        }
      };
  
      return {
        columnDefs,
        rowData,
        defaultColDef,
        cellWasClicked,
        deselectRows,
        onGridReady
      };
    }
  };
  </script>
  
  <style scoped>
  /* 可以在这里添加一些自定义的样式 */
  </style>
  