<template>
  <button @click="getTopTwoWaypoints">Refresh</button>
  <p>{{ this.getTopTwoWaypointsjson }}</p>
</template>

<script>
export default {
  name: "GetAllData",
  data() {
    return {
      ListOfAirports: [],
      nameOfAirportsOnly: [],
      getTopTwoWaypointsjson: [],
    };
  },
  methods: {
    //get all the airport names
    async getAllAirportName() {
      var myHeaders = new Headers();
      myHeaders.append("api-key", "123");

      var requestOptions = {
        method: "GET",
        headers: myHeaders,
        redirect: "follow",
      };

      await fetch("http://localhost:8081/api/Airport", requestOptions)
        .then((response) => response.json())
        .then((json) => (this.ListOfAirports = json))
        .catch((error) => console.log("error", error));

      for (let i = 0; i < this.ListOfAirports.length; i++) {
        console.log(this.ListOfAirports[i].uid);
        this.nameOfAirportsOnly.push(this.ListOfAirports[i].uid);
      }
      console.log(this.nameOfAirportsOnly);
    },

    //get the top 2 waypoints
    async getTopTwoWaypoints() {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      await fetch("http://localhost:8081/api/SID/AllAirports", requestOptions)
        .then((response) => response.json())
        .then((json) => (this.getTopTwoWaypointsjson = json))
        .catch((error) => console.log("error", error));

      console.log(this.getTopTwoWaypointsjson);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: #fbfbfb;
  color: rgb(0, 0, 0);
  text-align: center;
  width: 90%;
  max-width: 40rem;
  font-size: 3rem;
}
#myInput {
  background-position: 10px 12px; /* Position the search icon */
  background-repeat: no-repeat; /* Do not repeat the icon image */
  width: 80%; /* Full-width */
  font-size: 16px; /* Increase font-size */
  padding: 12px 20px 12px 40px; /* Add some padding */
  border: 1px solid #ddd; /* Add a grey border */
  margin-bottom: 12px; /* Add some space below the input */
}

#myTable {
  border-collapse: collapse; /* Collapse borders */
  width: 100%; /* Full-width */
  border: 1px solid #ddd; /* Add a grey border */
  font-size: 18px; /* Increase font-size */
}

#myTable th,
#myTable td {
  text-align: left; /* Left-align text */
  padding: 12px; /* Add padding */
}

#myTable tr {
  /* Add a bottom border to all table rows */
  border-bottom: 1px solid #ddd;
}

#myTable tr.header,
#myTable tr:hover {
  /* Add a grey background color to the table header and on hover */
  background-color: #f1f1f1;
}
.greenButton {
  font: inherit;
  cursor: pointer;
  border: 1px solid #adfa93;
  background-color: #b1fd96;
  color: rgb(0, 0, 0);
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
  border-radius: 10px;
  margin: 0 1rem;
}
</style>
