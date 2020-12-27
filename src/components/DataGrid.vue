<template>
  <div class="data-grid">
    <table class="data-grid__table">
      <thead>
        <tr>
          <th
            v-for="col in sortedColumns" 
            :key="col.key" 
            :class="col.className"
            draggable
            @click="handleColumnClick($event, col)" 
            @drop='onDrop($event, col)' 
            @dragstart='onDragStart($event, col)'
            @dragover="onDragOver($event, col)"
            @dragleave="onDragLeave($event, col)"
            @dragenter.prevent
          >
            <div class="data-grid__table__col">
              <span 
                v-if="isFilterActive(col, false)" 
                class="date-grid__table__col--filted"
              >
                  {{findFilterIndex(col, false) + 1}}
               </span>
              <span>{{col.title}}</span>
              <div class="grid__table__col__sorter">
                <i 
                  :class="{
                    'fas fa-caret-up': true,
                    'grid__table__col__sorter--active': isFilterActive(col, 1) 
                  }"
                />
                <i 
                  :class="{
                    'fas fa-caret-down': true,
                    'grid__table__col__sorter--active': isFilterActive(col, -1)  
                  }"
                />
              </div>
            </div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="row in sortedData" :key="row[rowKey]">
          <td v-for="col in sortedColumns" :key="col.key" v-html="renderCell(col, row)" />
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
const SortDirection = {
  Asc: 1,
  Desc: -1,
  None: 0
}
export default {
  name: 'DataGrid',
  props: {
    rowKey: {
      type: String,
      default: "id"
    },
    columns: {
      type: Array,
      required: true,
    },
    data: {
      type: Array,
      required: false,
    }
  },
  data: function() {
    return {
      sorters: [],
      draggingCol: null,
      cols: this.columns.map((col, i) => ({...col, order: i}))
    }
  },
  computed: {
    sortedColumns: function() {
      return [...this.cols].sort((a, b) => a.order - b.order);
    },
    sortedData: function () {
      return [...this.data].sort((curr, next) => {
        let sortResult = 0;
        for(const sort of this.sorters) {
          if(sortResult === 0) {
            if(typeof sort.sorter === 'function') {
              // Supports for customer comperer
              sortResult = sort.compare(curr, next);
            }else {
              const currValue = curr[sort.column].toString();
              const nextValue = next[sort.column].toString();
              sortResult = sort.direction * (currValue < nextValue ? -1 : (currValue > nextValue ? 1 : 0));
            } 
          }
        }
        return sortResult;
      });
    }
  },
  methods: {
    onDrop (e, droppedCol) {
      this.cols = this.cols.map((column) => {
        const updatedCol = {
          ...column,
          className: '',
        }
        
        if(this.draggingCol.key === column.key) {
          updatedCol.order = droppedCol.order;
        }

        if(column.key === droppedCol.key) {
          updatedCol.order = this.draggingCol.order;
        }

        return updatedCol;
      });

      this.draggingCol = null;
    },
    onDragOver(e, col) {
      e.preventDefault();
      this.cols = this.cols.map(column => {
        if(column.key === col.key && this.draggingCol.key !== column.key) {
          return  {
            ...column,
            className: 'over'
          }
        }
        return column;
      })
    },
    onDragLeave() {
      this.cols = this.cols.map(column => {
        return {
          ...column,
          className: '',
        };
      })
    },
    onDragStart (e, col) {
      this.draggingCol = col;
    },
    /**
     * Dir can be direction or false. If it is false direction will be ignore
     */
    findFilterIndex(column, dir) {
      return this.sorters.findIndex(x => x.column === column.key && (dir !==false ? x.direction === dir : x.direction !== 0));
    },
    isFilterActive(column, dir) {
      return this.findFilterIndex(column, dir) > -1;
    },
    /**
     * Returns new direction of the column,
     * O = None, 1 = Ascending Direction, -1 = Descanding Direction
     * columnDirection: Number current column direction
     */
    getSortDirection(columnDirection) {
      if(columnDirection++ < 1) {
        return columnDirection;
      }
      return -1;
    },
    handleColumnClick (e, column) {
      const sorterColumn = this.sorters.find(c => c.column === column.key);
      const newSortColumn = {
          column: column.dataIndex,
          sorter: column.sorter,
          direction: SortDirection.Asc
      }
      if(sorterColumn) {
        sorterColumn.direction = this.getSortDirection(sorterColumn.direction);
      }
      // Add to new filter if user pressing shift key.
      if(e.shiftKey) {
        if(! sorterColumn) {
          this.sorters.push(newSortColumn);
        }
      } else{
         this.sorters = [sorterColumn || newSortColumn];
      }
    },
    renderCell(col, row) {
      if(! col.render) {
        return row[col.dataIndex || col.key];
      }
      // Specific render for column
      return col.render(row[col.dataIndex], row)
    }
  },
}
</script>

<style scoped>
 @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;1,100&display=swap');
 @import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css');

.data-grid__table {
    width: 100%;
    text-align: left;
    border-radius: 2px 2px 0 0;
    border-collapse: separate;
    border-spacing: 0;
    font-family: 'Roboto', sans-serif;
}
.data-grid__table thead th {
  border: thin solid transparent;
  color: rgba(0, 0, 0, .85);
  font-weight: 500;
  text-align: left;
  background: #fafafa;
  border-bottom: 1px solid #f0f0f0;
  transition: background .3s ease;
  -webkit-transition: background .3s ease;

  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none; 
}
.data-grid__table  td, .data-grid__table th {
  position: relative;
  padding: 16px;
  overflow-wrap: break-word;
  
}
.data-grid__table tbody td {
  font-weight: lighter;
  border-bottom: 1px solid #f0f0f0;
  transition: background .3s;
}
.data-grid__table  th:hover {
  background: #f2f2f2;
  cursor: pointer;
}
.data-grid__table th.over {
    background: #dfdfdf;
  border: 2px dashed rgb(189, 189, 189);
}
.data-grid__table tr:hover td {
  background: #fafafa;
}
.data-grid__table__col {
  display: flex;
  justify-content: space-between;
}
.grid__table__col__sorter {
  display: flex;
  flex-direction: column;
}
.grid__table__col__sorter i.fas.fa-caret-down{
  margin-top: -8px;
}
.grid__table__col__sorter--active {
  color:rgb(40, 131, 228);
}
.date-grid__table__col--filted {
  position: absolute;
  font-size: 12px;
  right:0;
}
</style>
