<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-data-table
          :headers="headers"
          :items="users"
          :items-per-page="5"
          class="elevation-1"
      >
       <template v-slot:item="{item}">
         <tr>
           <td>{{item.name}}</td>
           <td>{{item.lastnameFather}}</td>
           <td>{{item.lastnameMother}}</td>
           <td>{{item.email}}</td>

            <td>
              <v-tooltip bottom>
                <template v-slot:activator="{on}">
                  <v-btn color="primary" @click="dialog = true" v-on="on">
                    <v-icon>edit</v-icon>
                  </v-btn>
                </template>
                <span>Editar</span>
              </v-tooltip>
            </td>

         </tr>
       </template>


      </v-data-table>
    
    </v-row>

  <template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Open Dialog
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">User Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  label="Legal first name*"
                  required
                  v-model="userSelected.name"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Legal middle name"
                  hint="example of helper text only on focus"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  label="Legal last name*"
                  hint="example of persistent helper text"
                  persistent-hint
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="Email*"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="saveUser"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import { Component } from "vue-property-decorator";
import { userService } from "@/services/user.service";
import { UserInterface } from "@/types/User.interface";
@Component
export default class App extends Vue {
  public name: string = "HelloWorld";
  public users: any = [];
  public dialog: boolean = false;
  public userSelected: UserInterface ={
    id: 0,
    name: '',
    lastnameFather: '',
    lastnameMother: '',
    email: '',
    password: ''
  };

  public headers: any =[
    {text: "Nombre", value: "name"},
    {text: "Apellido Paterno", value: "lastnameFather"},
    {text: "Apellido Materno", value: "lastnameMother"},
    { text: "Email", value: "email" },
    { text: "Acciones", value: "" },
  ]

  async mounted(){
    this.users = await userService.getUsuarios();
    console.log(this.users);
  }

  public openModal(){
    this.dialog = true;
  }

  public async userEdit(user:UserInterface){
    console.log(user);
    this.userSelected = user;
    
    const updateUser = await userService.updateUsuarios(user);
    console.log('Update from server', updateUser);
}
  public async userDelete(id: number){
    console.log(id);
    this.userSelected = this.userSelected;

    const deleteUser = await userService.deleteUsuarios(id);
    console.log('Eliminar usuario', deleteUser);

  }

  public async userAdd(user: UserInterface){
    console.log(user);
    this.userSelected = user;

    const userAdd = await userService.agregarUsuarios(user);
    console.log('Agregar usuario', userAdd);
  }

  public saveUser(){
    console.log('User to save', this.userSelected);
  }

};
</script>