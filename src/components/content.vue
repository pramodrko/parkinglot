<template>
    <div id="content" style="  text-align: left;">
        <v-row v-if="userVehicalType == 'car'">
            <v-col cols="12" v-for="slot in carSlots" :key="slot.id" md="6">
                <v-card class="elevation-6 mt-10" style="padding-left: 18px;">
                    <v-row>
                        <v-col cols="12" sm="7">
                            <!-- <v-checkbox :label="'Car Slot:'+slot.id" class="mt-n1" color="blue" aria-checked="green"> </v-checkbox> -->
                            <span class="caption blue--text mt-n1 font-weight-bold">Car Slot: {{slot.id}}</span>
                        </v-col>
                        <v-col cols="12" sm="7">
                            <span class="caption blue--text mt-n1 font-weight-bold">Parked User:
                                {{slot.parkedUser}}</span>
                        </v-col>
                        <v-col cols="12" sm="7">
                            <span class="caption blue--text mt-n1 font-weight-bold">Parked Time:
                                {{slot.parkedTime}}</span>
                        </v-col>
                        <v-col cols="7" sm="7" v-if="!slot.parkedUser">
                            <v-btn color="blue" dark block tile @click.native="parkCar(slot.id)"
                                :disabled="parkDisabled">Park</v-btn>
                        </v-col>
                        <v-col cols="7" sm="7" v-if="slot.parkedUser">
                            <v-btn color="red" dark block tile @click.native="unParkCar(slot.id)"
                                :disabled="unParkDisabled">UnPark</v-btn>
                        </v-col>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
        <v-row v-if="userVehicalType == 'bike'">
            <v-col cols="12" v-for="slot in bikeSlots" :key="slot.id" md="6">
                <v-card class="elevation-6 mt-10" style="padding-left: 18px;">
                    <v-row>
                        <v-col cols="12" sm="7">
                            <!-- <v-checkbox :label="'Car Slot:'+slot.id" class="mt-n1" color="blue" aria-checked="green"> </v-checkbox> -->
                            <span class="caption blue--text mt-n1 font-weight-bold">Bike Slot: {{slot.id}}</span>
                        </v-col>
                        <v-col cols="12" sm="7">
                            <span class="caption blue--text mt-n1 font-weight-bold">Bike User:
                                {{slot.parkedUser}}</span>
                        </v-col>
                        <v-col cols="12" sm="7">
                            <span class="caption blue--text mt-n1 font-weight-bold">Parked Time:
                                {{slot.parkedTime}}</span>
                        </v-col>
                        <v-col cols="7" sm="7" v-if="!slot.parkedUser">
                            <v-btn color="blue" dark block tile @click.native="parkBike(slot.id)"
                                :disabled="parkDisabled">Park</v-btn>
                        </v-col>
                        <v-col cols="7" sm="7" v-if="slot.parkedUser">
                            <v-btn color="red" dark block tile @click.native="unParkBike(slot.id)"
                                :disabled="unParkDisabled">UnPark</v-btn>
                        </v-col>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        data: () => ({
            snackbar: false,
            colorValue: 'error',
            snackbartimeout: 12000,
            snackbartext: '',
            userEmail: "",
            userNumberPlate: "",
            userVehicalType: "",
            parkDisabled: false,
            unParkDisabled: false,
            carSlots: [],
            bikeSlots: [],
            carSlotUrl: "http://localhost:3000/carSlots",
            bikeSlotUrl: "http://localhost:3000/bikeSlots",

        }),
        mounted() {
            this.userEmail = this.$route.params.userData.email
            this.userNumberPlate = this.$route.params.userData.numberPlate
            this.userVehicalType = this.$route.params.userData.vehicalType
            this.getAllCarSlots();
            this.getAllBikeSlots();
        },
        methods: {
            getAllCarSlots() {
                axios.get(this.carSlotUrl)
                    .then(response => {
                        var serverResponse = response.data.filter(slot => !slot.parkedUser || slot.parkedUser ==
                            this.userEmail);
                        this.carSlots = serverResponse
                        console.log(serverResponse)
                        this.changeBtnStatus()
                    })
                    .catch(e => {
                        console.log(e)
                    })
            },
            getAllBikeSlots() {
                axios.get(this.bikeSlotUrl)
                    .then(response => {
                        var serverResponse = response.data.filter(slot => !slot.parkedUser || slot.parkedUser ==
                            this.userEmail);
                        this.bikeSlots = serverResponse
                        console.log(serverResponse)
                        this.changeBtnStatus()
                    })
                    .catch(e => {
                        console.log(e)
                    })
            },
            parkCar(slotId) {
                axios.put(this.carSlotUrl + "/" + slotId, {
                        parkedUser: this.userEmail,
                        parkedTime: new Date(),
                        numberPlate: this.userNumberPlate,
                    })
                    .then(response => {
                        var serverResponse = response.data
                        console.log(serverResponse)
                        this.getAllCarSlots()
                        this.changeBtnStatus()
                    })
                    .catch(e => {
                        console.log(e)
                    })
            },
            unParkCar(slotId) {
                axios.put(this.carSlotUrl + "/" + slotId, {
                        parkedUser: "",
                        parkedTime: "",
                        numberPlate: "",
                    })
                    .then(response => {
                        var serverResponse = response.data
                        console.log(serverResponse)
                        this.getAllCarSlots();
                    })
                    .catch(e => {
                        console.log(e)
                    })
            },
            parkBike(slotId) {
                axios.put(this.bikeSlotUrl + "/" + slotId, {
                        parkedUser: this.userEmail,
                        parkedTime: new Date(),
                        numberPlate: this.userNumberPlate,
                    })
                    .then(response => {
                        var serverResponse = response.data
                        console.log(serverResponse)
                        this.getAllBikeSlots();
                    })
                    .catch(e => {
                        console.log(e)
                    })
            },
            unParkBike(slotId) {
                axios.put(this.bikeSlotUrl + "/" + slotId, {
                        parkedUser: "",
                        parkedTime: "",
                        numberPlate: "",
                    })
                    .then(response => {
                        var serverResponse = response.data
                        console.log(serverResponse)
                        this.getAllBikeSlots();

                    })
                    .catch(e => {
                        console.log(e)
                    })
            },
            changeBtnStatus() {
                let slots = this.carSlots.filter(slot => slot.parkedUser == this.userEmail) || this.bikeSlots.filter(
                    slot => slot.parkedUser == this.userEmail)
                if (slots.length != 0) {
                    this.parkDisabled = true
                    // this.ubParkDisabled = false
                } else {
                    this.parkDisabled = false
                    this.ubParkDisabled = false
                }
            }
        }
    };
</script>

<style lang="scss" scoped>
    .teaser {
        position: relative;
        width: 100%;
        max-width: 768px;
        height: 400px;
        margin-bottom: 40px;
        background-size: cover;
        background-position: top;
        border-radius: 20px;
        overflow: hidden;

        .teaser-content {
            position: absolute;
            left: 20px;
            bottom: 20px;
            width: 210px;
            height: 50px;
            padding: 0 0 0 20px;
            background-color: #111;
            color: #efefef;
            border-radius: 20px;
            font-size: 2rem;
            cursor: pointer;
        }

        i {
            font-size: 1.6rem;
        }
    }

    .infobox {
        float: left;
        width: 80px;
        height: 80px;
        padding: 10px;
        margin: 0 20px 20px 0;
        border-radius: 20px;
        border: 1px solid #ccc;

        .title {
            font-weight: bold;
        }

        .value {
            padding: 10px 0 0 0;
            font-size: 2rem;
        }
    }

    .v-input--selection-controls__input {
        background: green !important;
    }
</style>