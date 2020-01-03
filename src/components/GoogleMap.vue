<template>
    <div class="container">
        <div class="row">
            <h2>Search Location and Add Pin</h2>
            <label>
                <gmap-autocomplete
                    @place_changed = "setPlace">
                 </gmap-autocomplete>
                <button @click="addMarker" class="btn btn-primary">Add</button>
            </label>
        </div>

        <gmap-map
            :center="center"
            :zoom="12"
            style="width: 100%; height: 400px">
        <gmap-marker
            :key="index"
            v-for="(m, index) in markers"
            :position="m.position">
          </gmap-marker>
        </gmap-map>
    </div>
</template>

<script>
    export  default {
        name: "GoogleMap",
        data() {
            return {
                center: {lat: 45.508, lng: -73.587},
                markers: [],
                places: [],
                currentPlace: null
            };
        },

        mounted() {
            this.geolocate();
        },

        methods: {
        setPlace(place)
        {
            this.currentPlace = place;
        },

        addMarker()
        {
            if (this.currentPlace) {
                const marker = {
                    lat: this.currentPlace.geometry.location.lat(),
                    lng: this.currentPlace.geometry.location.lng()
                };
                this.markers.push({position: marker});
                this.places.push(this.currentPlace);
                this.center = marker;
                this.currentPlace = null;
            }
        },

        geolocate: function () {
            navigator.geolocation.getCurrentPosition(position => {
                this.center = {
                    lat: position.coords.latitude,
                    lng: position.coords.longitude
                }
            })
        }
    }
    }



</script>

<style>
    h2{
        font-family: Helvetica;;
        color: #999999;
        margin-right:  10px;
        margin-left: 20px;
    }

    input{
        font-family: Helvetica;
        margin-right: 10px;
        border-radius: 8px;
        padding: 5px;
        border: #999 1px solid;
    }
</style>