<template lang="html">
    <div>     

<div>

 <p> <small> Meet point address</small> </p>
            <!--<gmap-autocomplete
                    @place_changed="setPlace" :options="peruLimit" placeholder="Marca lugar del origen" class="form-control mb-2">
            </gmap-autocomplete>-->
            <gmap-autocomplete
                    @place_changed="setPlace">
            </gmap-autocomplete>
            <!-- <button @click="addMarker1" class="btn btn-primary">Marca A</button> -->
            <button @click.prevent="addMarker">Add</button>

</div>



        <gmap-map
                :center="center"
                :zoom="12"
                style="width:100%;  height: 400px;"
        >
            <gmap-marker
                    :key="index"
                    v-for="(m, index) in markers"
                    :position="m.position"
                    @click="center=m.position"
            ></gmap-marker>
        </gmap-map>

    </div>
</template>

<script>

    export default {
        props: {
            parentData: Object,
            stringProp: String,
            title: String
        },
        data(){
            return {
                center: { lat: 45.508, lng: -73.587 },
                markers: [],
                places: [],
                currentPlace: '',
                peruLimit: {
                    bounds: {north: -0.03, south: -18.4, east: -68.6, west: -81.3},
                    strictBounds: true
                },

            }
        },
        methods: {
            setPlace(place) {
                this.currentPlace = place;
            },
            addMarker() {
                if (this.currentPlace) {
                    const marker = {
                        lat: this.currentPlace.geometry.location.lat(),
                        lng: this.currentPlace.geometry.location.lng()
                    };
                    //this.markers.push({ position: marker });
                    //this.places.push(this.currentPlace);
                    this.markers[0] = { position: marker };
                    this.places[0] = this.currentPlace;
                    this.center = marker;
                    this.currentPlace = null;
                }
            },
            geolocate: function() {
                navigator.geolocation.getCurrentPosition(position => {
                    this.center = {
                        lat: position.coords.latitude,
                        lng: position.coords.longitude
                    };
                });
            },
        },
        computed: {

        },
        mounted() {
            this.geolocate();
        },
    }
</script>

<style lang="css" scoped>
</style>
