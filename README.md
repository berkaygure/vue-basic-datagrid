# Vue Basic Datagrid
A basic data-grid.

## Demo

[Live Demo](https://vue-data-grid-case-study.vercel.app)

## Features
- Drag-drop column order (with HTML5)
- Multiple sorter (with shift-key)
## Install
```
yarn add vue-basic-datagrid
```
## Usage
```html
<template>
    <data-grid  :columns="columns" :data="data"/>
</template>
```

```javascript
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
            return `<a href="/#flight-details">${flightNumber}</a>`;
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
```



