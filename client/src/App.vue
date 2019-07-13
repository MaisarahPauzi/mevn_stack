<template>
 <div id="app">
  <v-app id="inspire" dark>
    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <v-card>
          <v-toolbar color="pink" dark>
  
            <v-toolbar-title>New Entry</v-toolbar-title>
  
            <v-spacer></v-spacer>
  
            
          </v-toolbar>
          <v-list two-line >
            <template>
              <v-list-tile>
                <v-text-field label="Title" v-model="title"></v-text-field>
              </v-list-tile>
               <v-list-tile>
                <v-text-field label="Content" v-model="content"></v-text-field>
              </v-list-tile>
               <v-list-tile>
                <v-btn color="pink" dark @click="addPost()">Post it!</v-btn>
              </v-list-tile>
                 
            </template>
          </v-list>
          
          <v-toolbar color="pink" dark>
  
            <v-toolbar-title>Mini Diary List</v-toolbar-title>
  
            <v-spacer></v-spacer>
  
            
          </v-toolbar>
  
          <v-list two-line>
            <template v-for="item in items">
              <v-list-tile
                :key="item.title"
                avatar
                ripple
              >
                <v-list-tile-content>
                  <v-list-tile-title>
                    {{ item.title }}
                  </v-list-tile-title>
                  <v-list-tile-sub-title class="text--primary">
                    {{ item.content }}
                  </v-list-tile-sub-title>
                <v-list-tile-sub-title>
                  {{ item.timestamp | formatDate}}
                  </v-list-tile-sub-title>
              </v-list-tile-content>
              <v-list-tile-action>
                <v-btn flat small @click="deletePost(item._id)"><v-icon>delete</v-icon></v-btn>
              </v-list-tile-action>
              </v-list-tile>
              <v-divider
              ></v-divider>
            </template>
          </v-list>
        </v-card>
      </v-flex>
    </v-layout>
  </v-app>
</div>
</template>

<script>
/* eslint-disable */
import axios from 'axios';

export default {
  name: 'App',
  data () {
    return {
      title:'',
      content:'',
      items: [],
      date: new Date().toISOString().substr(0, 10)
    }
  },
  created(){
    axios.get(`api/posts`)
    .then(response => {
      //console.log(response.data);
      this.items = response.data;
    })
    .catch(e => {
      this.errors.push(e)
    });
  },

  methods: {

    addPost(){
      axios.post( 'api/posts',
                {
                  "title":this.title,
                  "content":this.content
                }).then(function(){
          console.log('success');
          
        })
        .catch(function(){
          console.log('failed');
        });
        this.title = '';
        this.content = '';
    },

    deletePost(id){
      axios.delete( 'api/posts/'+ id).then(function(){
          console.log('success delete record');
        })
        .catch(function(){
          console.log('failed delete record');
        });
    }
    
  }
}
</script>
