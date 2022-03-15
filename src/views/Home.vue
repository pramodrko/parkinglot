<template>
  <v-container>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="10">
        <v-card class="elevation-6 mt-10">
          <v-window v-model="step">
            <v-window-item :value="1">
              <v-row>
                <v-col cols="12" md="6">
                  <v-card class="mt-12 card" elevation="1">
                    <h4 class="text-center">Login in to Your Account</h4>
                    <h6 class="text-center  grey--text ">Log in to your account so you can use <br>
                      parkinglot system</h6>
                    <v-form ref="loginForm" v-model="loginValid">
                      <v-row align="center" justify="center">
                        <v-col cols="12" sm="8">

                          <v-text-field label="Email" v-model="loginEmail" :rules="emailRules" outlined dense
                            color="blue" autocomplete="false" class="mt-16" />
                          <v-text-field label="Password" v-model="loginPassword" :rules="passwordRules" outlined dense
                            color="blue" autocomplete="false" type="password" />
                          <v-btn color="blue" dark block tile @click.native="loginUser">Log in</v-btn>
                        </v-col>
                      </v-row>
                    </v-form>
                  </v-card>
                </v-col>
                <v-col cols="12" md="6" class="blue rounded-bl-xl">
                  <div style="  text-align: center; padding: 180px 0;">
                    <v-card-text class="white--text">
                      <h3 class="text-center ">Don't Have an Account Yet?</h3>
                      <h6 class="text-center">Let's get you all set up so you can start using<br>
                        oparkinglot system</h6>
                    </v-card-text>
                    <div class="text-center">
                      <v-btn tile outlined dark @click="step++">SIGN UP</v-btn>
                    </div>
                  </div>
                </v-col>
              </v-row>
            </v-window-item>
            <v-window-item :value="2">
              <v-row>
                <v-col cols="12" md="6" class="blue rounded-br-xl">
                  <div style="  text-align: center; padding: 180px 0;">
                    <v-card-text class="white--text">
                      <h3 class="text-center ">Alredy Signed up?</h3>
                      <h6 class="text-center">Log in to your account so you can use <br>
                        parkinglot system</h6>
                    </v-card-text>
                    <div class="text-center">
                      <v-btn tile outlined dark @click="step--">Log in</v-btn>
                    </div>
                  </div>
                </v-col>

                <v-col cols="12" md="6">
                  <v-card-text class="mt-12">
                    <h4 class="text-center">Sign Up for an Account</h4>
                    <h6 class="text-center  grey--text ">Let's get you all set up so you can start using<br>
                      oparkinglot system</h6>
                    <v-row align="center" justify="center">
                      <v-col cols="12" sm="8">
                        <v-form ref="registerForm" v-model="registerValid">
                          <v-row>
                            <v-col cols="12" sm="6">
                              <v-text-field label="First Name" v-model="firstName" :rules="firstNameRules" outlined
                                dense color="blue" autocomplete="false" class="mt-4" />
                            </v-col>
                            <v-col cols="12" sm="6">
                              <v-text-field label="Last Name" v-model="lastName" :rules="lastNameRules" outlined dense
                                color="blue" autocomplete="false" class="mt-4" />
                            </v-col>
                          </v-row>
                          <v-text-field label="Email" v-model="registerEmail" :rules="emailRules" outlined dense
                            color="blue" autocomplete="false" />
                          <v-text-field label="Password" v-model="registerPassword" :rules="passwordRules" outlined
                            dense color="blue" autocomplete="false" type="password" />
                          <v-row>
                            <v-col cols="12" sm="7">
                              <v-radio-group v-model="vehicalType" label="Vehical Type" mandatory outlined dense
                                color="blue" class="radio mt-n1">
                                <v-radio label="Bike" value="bike"></v-radio>
                                <v-radio label="Car" value="car"></v-radio>

                              </v-radio-group>
                            </v-col>


                          </v-row>
                          <v-text-field label="Number Plate" v-model="numberPlate" :rules="numberPlateRules" outlined
                            dense color="blue" autocomplete="false" />
                          <v-btn color="blue" dark block tile @click.native="registerUser">Sign up</v-btn>
                        </v-form>
                      </v-col>
                    </v-row>
                  </v-card-text>
                </v-col>
              </v-row>
            </v-window-item>
          </v-window>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" :color="colorValue" :timeout="snackbartimeout" id="login" top right>{{ snackbartext }}</v-snackbar>
  </v-container>
</template>

<script>
  import axios from 'axios'

  export default {
    data: () => ({
      snackbar:false,
      colorValue:'error',
      snackbartimeout: 12000,
      snackbartext:'',
      step: 1,
      loginEmail: "",
      loginPassword: "",
      firstName: "",
      lastName: "",
      registerEmail: "",
      registerPassword: "",
      numberPlate: "",
      vehicalType: "bike",
      userUrl:"http://localhost:3000/users",
      firstNameRules: [v => !!v || 'First Name is required.'],
      lastNameRules: [v => !!v || 'Last Name is required.'],
      passwordRules: [v => !!v || 'Password is required.'],
      emailRules: [v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ],
      numberPlateRules: [v => !!v || 'Number Plate is required.',
        v => /^[A-Z]{2}[ -][0-9]{1,2}(?: [A-Z])?(?: [A-Z]*)? [0-9]{4}$/.test(v) || 'Number Plate must be valid'
      ],
    }),
    props: {
      source: String
    },
    mounted() {
      this.getAllUsers();
    },
    methods: {
      getAllUsers() {
        axios.get(this.userUrl)
          .then(response => {
            var serverResponse = response.data
            console.log(serverResponse)
          })
          .catch(e => {
            console.log(e)
          })
      },
      registerUser() {
        axios.post(this.userUrl, {firstName:this.firstName,lastName:this.lastName,
                                  email:this.registerEmail,
                                  password:this.password,vehicalType:this.vehicalType,numberPlate:this.numberPlate})
          .then(response => {
            var serverResponse = response.data
            this.step--
            console.log(serverResponse)
          })
          .catch(e => {
            console.log(e)
          })
      },
      loginUser() {
        axios.get(this.userUrl)
          .then(response => {
            var serverResponse = response.data
            var user = serverResponse.find(u => u.email === this.loginEmail && u.password === this.loginPassword);
            if(user){
              console.log(user)
              this.$router.push({ name: 'Dashboard', params: {userData:user}});
            }
            else{
               this.snackbar = true
              this.colorValue = 'error'
              this.snackbartext = "User name or password is wrong please check again";
            }
            
          })
          .catch(e => {
            console.log(e)
          })
      },

    },


  }
</script>
<style scoped>
  .v-application .rounded-bl-xl {
    border-bottom-left-radius: 300px !important;
  }

  .v-application .rounded-br-xl {
    border-bottom-right-radius: 300px !important;
  }

  /* .radio >>> label {
    font-weight: bold !important; 
  } */
</style>