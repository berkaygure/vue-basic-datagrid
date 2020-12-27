<template>
  <div class="container">
    <h2 class="demo-title">Vue-Basic-DataTable Demo</h2>
    <ul>
      <li>You can change columns order with dragging</li>
      <li>For multiple sorting press shift-key and click to columns.</li> 
    </ul>
    <div class="table-wrapper">
      <data-grid  :columns="columns" :data="data"/>
    </div>
  </div>
</template>

<script>
import DataGrid from './components/DataGrid.vue'
const getDate = (dateStr) => { 
  return new Date(Date.parse(dateStr));
}
export default {
  name: 'app',
  data: () => {
    return {
      columns: [
        { 
          title: 'Flight Number', 
          key: 'flight_number', 
          dataIndex: 'flight_number',
          render: function (flightNumber) {
            return `<a href="/#flight-details"><i style="font-size:11px" class="fas fa-external-link-alt"></i> ${flightNumber}</a>`;
          }
        },
        { title: 'Airline', key: 'airline', dataIndex: 'airline'},
        { title: 'Destination', key: 'destination', dataIndex: 'destination'},
        { 
          title: 'Scheduled', 
          key: 'scheduled', 
          dataIndex: 'scheduled',
          render: function (scheduled) { 
            return scheduled.toDateString();
          }
        },
      ],
      data: [
        {
          id: 1,
          flight_number: 1000,
          airline: 'Freebird',
          destination: 'Amsterdam',
          scheduled: new Date(),
        },
        {
          id: 2,
          flight_number: 1001,
          airline: 'Freebird',
          destination: 'Berlin',
          scheduled: getDate("05/09/1993"),
        },
        {
          id: 3,
          flight_number: 1002,
          airline: 'Turkish Airlines',
          destination: 'Baku',
          scheduled: getDate("10/29/1923"),
        },
        {
          id: 4,
          flight_number: 1456,
          airline: 'Korean Air',
          destination: 'Seoul',
          scheduled: getDate("10/15/2021"),
        },
        {
          id: 5,
          flight_number: 1985,
          airline: 'Frontier Airlines',
          destination: 'Kosova',
          scheduled: getDate("05/15/2020"),
        },
        {
          id: 6,
          flight_number: 1756,
          airline: 'Freebird',
          destination: 'Berlin',
          scheduled: getDate("2/28/2020"),
        },
        {
          id: 7,
          flight_number: 3256,
          airline: 'Fly Emirates',
          destination: 'Monaco',
          scheduled: getDate("11/14/2020"),
        },
        {
          id: 8,
          flight_number: 8556,
          airline: 'Fly Emirates',
          destination: 'Tahran',
          scheduled: getDate("12/12/2020"),
        },
        {
          id: 9,
          flight_number: 1458,
          airline: 'Tailwind',
          destination: 'Tokyo',
          scheduled: getDate("6/26/2020"),
        },
        {
          id: 10,
          flight_number: 1123,
          airline: 'Pegasus',
          destination: 'Berlin',
          scheduled: getDate("7/10/2020"),
        },
      ]
    }
  },
  components: {
    DataGrid
  }
}
</script>
<style>
  .container {
    width: 90%;
    margin: 0 auto;
    font-family: 'Roboto', sans-serif;
  }
  .demo-title {
    text-align: center;
    border-bottom: thin solid #eee;
    padding-bottom: 20px;
  }
  .table-wrapper {
    margin: 0 auto;
    box-shadow: 1px 1px 5px rgba(0,0,0,.3);
  }
</style>