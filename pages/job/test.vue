<template>
   
                <v-card
                  flat
                  class="d-flex flex-column fill-height"
                >
                  <v-card-title>
                    Revision
                  </v-card-title>
                  <v-card-text class="flex-grow-1 overflow-y-auto">
                    <template>
                      <div v-for="(msg, i) in messages" :key="i"
                        :class="{ 'd-flex flex-row-reverse': msg.me }"
                      >
                        <v-menu offset-y>
                          <template v-slot:activator="{ on }">
                             <v-chip
                                :color="msg.me ? 'primary' : 'secondary'"
                                dark
                                style="height:auto;white-space: normal;"
                                class="pa-4 mb-2"
                                v-on="on"
                              >
                                {{ msg.content }}
                                <sub
                                  class="ml-2"
                                  style="font-size: 0.5rem;"
                                >{{ msg.created_at }}</sub>
                               
                              </v-chip>
                          </template>
                         
                        </v-menu>
                      </div>
                    </template>
                  </v-card-text>
                  <!-- <v-card-text class="flex-shrink-1">
                      <v-text-field
                      v-model="messageForm.content"
                      label="type_a_message"
                      type="text"
                      no-details
                      outlined
                      append-outer-icon="send"
                      @keyup.enter="messages.push(messageForm)"
                      @click:append-outer="messages.push(messageForm)"
                      hide-details
                    />
                  </v-card-text> -->
                </v-card>
     
</template>


<script>
  export default {
    data: () => ({
  
    messages: [
      {
        content: "lorem ipsum",
        me: true,
        created_at: "11:11am"
      },
      {
        content: "dolor",
        me: false,
        created_at: "11:11am"
      },
      {
        content: "dolor",
        me: false,
        created_at: "11:11am"
      },
      {
        content: "dolor",
        me: false,
        created_at: "11:11am"
      },
      {
        content: "dolor",
        me: true,
        created_at: "11:11am"
      },
      {
        content: "dolor",
        me: false,
        created_at: "11:12am"
      },
      {
        content: "dolor",
        me: false,
        created_at: "11:14am"
      }
    ],
    messageForm: {
      content: "",
      me: true,
      created_at: "11:11am"
    }
  })
   
  }
</script>