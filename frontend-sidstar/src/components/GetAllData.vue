<template>
  <main>
    <label>Authetication Key: </label>
    <input v-model="authenticate_" type="text" class="input-element" />

    <button @click="getTopTwoWaypoints">Enter</button>
    <p></p>
    <div>
      <label class="label1">SIDs - Top 2 Waypoints</label>
      <p></p>
      <p>
        <input
          type="text"
          id="myInput"
          @keyup="searchSID"
          placeholder="Search for Airports names.."
          v-model="searchTermSids"
        />
      </p>

      <section v-if="getTopTwoWaypointsjson !== []">
        <table id="myTable">
          <tr class="header">
            <th style="width: 20%">Airports</th>
            <th style="width: 20%">1st Waypoint</th>
            <th style="width: 20%">Count</th>
            <th style="width: 20%">2nd Waypoint</th>
            <th style="width: 20%">Count</th>
          </tr>

          <tr v-for="result in this.finalSid" :key="result.id">
            <td>{{ result.airportName }}</td>
            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[0].key }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>

            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[0].value }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>
            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[1].key }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>
            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[1].value }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>
          </tr>
        </table>
      </section>
    </div>

    <p></p>
    <div>
      <label class="label1">Stars - Top 2 Waypoints</label>
      <p></p>
      <p>
        <input
          type="text"
          id="myInput"
          @keyup="searchStars"
          placeholder="Search for Airports names.."
          v-model="searchTermStars"
        />
      </p>

      <section v-if="getTopTwoWaypointsjson !== []">
        <table id="myTable">
          <tr class="header">
            <th style="width: 20%">Airports</th>
            <th style="width: 20%">1st Waypoint</th>
            <th style="width: 20%">Count</th>
            <th style="width: 20%">2nd Waypoint</th>
            <th style="width: 20%">Count</th>
          </tr>

          <tr v-for="result in this.finalStars" :key="result.id">
            <td>{{ result.airportName }}</td>
            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[0].key }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>

            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[0].value }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>
            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[1].key }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>
            <td v-if="result.topWaypointCounts !== null">
              {{ result.topWaypointCounts[1].value }}
            </td>
            <td v-if="result.topWaypointCounts === null">_</td>
          </tr>
        </table>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  name: "GetAllData",
  data() {
    return {
      authenticate_: "",
      ListOfAirports: [],
      nameOfAirportsOnly: [],
      getTopTwoWaypointsjson: [],
      searchTermSids: "",
      searchTermStars: "",
      tablerow: [],
      sid: [],
      stars: [],
      finalSid: [],
      finalStars: [],
    };
  },
  methods: {
    searchSID() {
      this.finalSid = [];

      for (let i = 0; i < this.sid.length; i++) {
        if (this.sid[i].airportName.includes(this.searchTermSids)) {
          this.finalSid.push(this.sid[i]);
        }
      }
    },

    searchStars() {
      this.finalStars = [];

      for (let i = 0; i < this.stars.length; i++) {
        if (this.stars[i].airportName.includes(this.searchTermStars)) {
          this.finalStars.push(this.stars[i]);
        }
      }
    },

    async getAllAirportName() {
      console.log(this.authenticate_);

      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");
      myHeaders.append("Access-Control-Allow-Origin", "*");
      myHeaders.append(
        "Access-Control-Allow-Methods",
        "POST,GET,OPTIONS, PUT, DELETE"
      );
      myHeaders.append(
        "Access-Control-Allow-Headers",
        "Accept, Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization"
      );

      var raw = JSON.stringify({
        authetication: this.authenticate_,
      });

      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
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
      console.log(this.authenticate_);
      var myHeaders = new Headers();

      myHeaders.append("Content-Type", "application/json");
      myHeaders.append("Content-Type", "application/json");
      myHeaders.append("Access-Control-Allow-Origin", "*");
      myHeaders.append(
        "Access-Control-Allow-Methods",
        "POST,GET,OPTIONS, PUT, DELETE"
      );
      myHeaders.append(
        "Access-Control-Allow-Headers",
        "Accept, Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization"
      );
      var raw = JSON.stringify({
        authetication: this.authenticate_,
      });

      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: raw,
        redirect: "follow",
      };

      await fetch("http://localhost:8081/api/SID/AllAirports", requestOptions)
        .then((response) => response.json())
        .then((json) => (this.getTopTwoWaypointsjson = json))
        .then()
        .catch((error) => console.log("error", error));

      this.sid = this.getTopTwoWaypointsjson.sid;
      this.stars = this.getTopTwoWaypointsjson.stars;
      this.finalSid = this.sid;
      this.finalStars = this.stars;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.input-element {
  width: 400px;
}

div {
  padding: 10px;
  border: 5px solid gray;
  margin: 0;
}
.content {
  display: flex;
  justify-content: space-between;
  max-width: 400px;
  margin: 0 auto;
  background: #a0c5e8;
  padding: 10px 10px 10px 10px;
}
span {
  width: 50px;
  height: 50px;
  background: black;
}
button {
  margin: 10px;
}
.label1 {
  font-size: 3rem;
  margin: 0 3rem;
  padding: 1rem;
  font-weight: bold;
}
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
