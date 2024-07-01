<template>
  <v-container>
    <v-text-field v-model="search" label="Search" class="mb-4"></v-text-field>
    <v-data-table
      :headers="headers"
      :items="sortedItems"
      :search="search"
      class="elevation-1"
      :sort-desc.sync="sortDesc"
      :sort-by.sync="sortBy"
    >
      <template v-slot:header>
        <thead>
          <draggable
            v-model="headers"
            tag="tr"
            :options="{ handle: '.sortable-handle' }"
            @end="updateHeaders"
          >
            <th
              v-for="(header, index) in headers"
              :key="index"
              class="sortable-handle"
              @click="sort(header.value)"
            >
              {{ header.text }}
              <v-icon small v-if="sortBy === header.value">{{
                sortIcon
              }}</v-icon>
            </th>
          </draggable>
        </thead>
      </template>

      <template
        v-for="header in headers"
        v-slot:[`item.${header.value}`]="props"
      >
        <v-edit-dialog
          :key="header.value"
          :return-value.sync="props.item[header.value]"
        >
          {{ props.item[header.value] }}
          <template v-slot:input>
            <v-text-field
              v-model="props.item[header.value]"
              label="Edit"
              single-line
            ></v-text-field>
          </template>
        </v-edit-dialog>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
  data() {
    return {
      search: "",
      sortDesc: false,
      sortBy: "name", // Default sort by 'name'
      headers: [
        { text: "Name", value: "name", sortable: true },
        { text: "Email", value: "email", sortable: true },
        { text: "Phone Number", value: "phoneNumber", sortable: true },
        { text: "Lead Status", value: "role", sortable: true },
        { text: "Create Date (EDT)", value: "createDate", sortable: true },
      ],
      items: [
        {
          id: 1,
          name: "John Doe",
          phoneNumber: "45672343219",
          role: "Lead",
          email: "john.doe@example.com",
          age: 30,
          status: "Active",
          createDate: new Date(),
        },
        {
          id: 2,
          name: "Jane Smith",
          phoneNumber: "25672343219",
          role: "Lead",
          email: "jane.smith@example.com",
          age: 25,
          status: "Inactive",
          createDate: new Date(),
        },
        {
          id: 3,
          name: "Sam Johnson",
          phoneNumber: "15672343219",
          role: "Lead",
          email: "sam.johnson@example.com",
          age: 35,
          status: "Active",
          createDate: new Date(),
        },
        {
          id: 4,
          name: "Lisa Brown",
          phoneNumber: "75672343219",
          role: "Lead",
          email: "lisa.brown@example.com",
          age: 28,
          status: "Active",
          createDate: new Date(),
        },
        {
          id: 5,
          name: "Paul Davis",
          phoneNumber: "85672343219",
          role: "Lead",
          email: "paul.davis@example.com",
          age: 22,
          status: "Inactive",
          createDate: new Date(),
        },
      ],
    };
  },
  computed: {
    sortedItems() {
      // Perform sorting based on sortBy and sortDesc
      return this.items.slice().sort((a, b) => {
        const modifier = this.sortDesc ? -1 : 1;
        if (a[this.sortBy] < b[this.sortBy]) return -1 * modifier;
        if (a[this.sortBy] > b[this.sortBy]) return 1 * modifier;
        return 0;
      });
    },
    sortIcon() {
      return this.sortDesc ? "mdi-chevron-down" : "mdi-chevron-up";
    },
  },
  methods: {
    updateHeaders(event) {
      const movedItem = this.headers.splice(event.oldIndex, 1)[0];
      this.headers.splice(event.newIndex, 0, movedItem);
    },
    sort(column) {
      if (column === this.sortBy) {
        this.sortDesc = !this.sortDesc;
      } else {
        this.sortBy = column;
        this.sortDesc = false;
      }
    },
    initDraggable() {
      var tables = document.getElementsByTagName("table");
      for (var i = 0; i < tables.length; i++) {
        resizableGrid(tables[i]);
      }

      function resizableGrid(table) {
        var row = table.getElementsByTagName("tr")[0],
          cols = row ? row.children : undefined;
        if (!cols) return;

        table.style.overflow = "hidden";

        var tableHeight = table.offsetHeight;

        for (var i = 0; i < cols.length; i++) {
          var div = createDiv(tableHeight);
          cols[i].appendChild(div);
          cols[i].style.position = "relative";
          setListeners(div);
        }

        function setListeners(div) {
          var pageX, curCol, nxtCol, curColWidth, nxtColWidth;

          div.addEventListener("mousedown", function (e) {
            curCol = e.target.parentElement;
            nxtCol = curCol.nextElementSibling;
            pageX = e.pageX;

            var padding = paddingDiff(curCol);

            curColWidth = curCol.offsetWidth - padding;
            if (nxtCol) nxtColWidth = nxtCol.offsetWidth - padding;
          });

          div.addEventListener("mouseover", function (e) {
            e.target.style.borderRight = "2px solid #0000ff";
          });

          div.addEventListener("mouseout", function (e) {
            e.target.style.borderRight = "";
          });

          document.addEventListener("mousemove", function (e) {
            if (curCol) {
              var diffX = e.pageX - pageX;

              if (nxtCol) nxtCol.style.width = nxtColWidth - diffX + "px";

              curCol.style.width = curColWidth + diffX + "px";
            }
          });

          document.addEventListener("mouseup", function () {
            curCol = undefined;
            nxtCol = undefined;
            pageX = undefined;
            nxtColWidth = undefined;
            curColWidth = undefined;
          });
        }

        function createDiv(height) {
          var div = document.createElement("div");
          div.style.top = 0;
          div.style.right = 0;
          div.style.width = "5px";
          div.style.position = "absolute";
          div.style.cursor = "col-resize";
          div.style.userSelect = "none";
          div.style.height = height + "px";
          return div;
        }

        function paddingDiff(col) {
          if (getStyleVal(col, "box-sizing") == "border-box") {
            return 0;
          }

          var padLeft = getStyleVal(col, "padding-left");
          var padRight = getStyleVal(col, "padding-right");
          return parseInt(padLeft) + parseInt(padRight);
        }

        function getStyleVal(elm, css) {
          return window.getComputedStyle(elm, null).getPropertyValue(css);
        }
      }
    },
  },
  mounted() {
    this.initDraggable()
  }
};
</script>

<style>
.v-data-table-header th {
  cursor: col-resize;
}
th,
td {
  border-right: 1px solid #e6e5e8;
}
.sortable-handle {
  cursor: grab;
}
.v-data-table-header {
  display: none;
}
</style>
