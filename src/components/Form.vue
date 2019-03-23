<template>
  <v-container>
    <v-layout align-center>
      <v-flex xs12 md8 sm8 column offset-md2 offset-sm2>
        <v-card>
          <v-card-text>
            <v-select label="Язык форума" v-model="lang" item-text="name" item-value="val" :items="langs"></v-select>
            <v-select label="Форумы" v-model="forums_selected" multiple item-text="name" item-value="uri"
                      :items="forums"
                      v-if="forums.length"></v-select>
            <v-btn color="primary" v-if="forums_selected.length" @click.stop="genLink">Сгенерировать</v-btn>
            <v-container v-if="links.length">
              <ul>
                <li v-for="link in links" :key="link"><a :href="link" target="_blank">{{ link }}</a></li>
              </ul>
            </v-container>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
    export default {
        name: "Form",
        data() {
            return {
                langs: [
                    {
                        val: "en",
                        name: "English"
                    },
                    {
                        val: "ru",
                        name: "Русский"
                    }
                ],
                lang: null,
                forums: [],
                forums_selected: [],
                links: []
            }
        },
        watch: {
            lang() {
                fetch("http://192.168.43.244:5000/get?lang=" + this.lang)
                    .then(resp => resp.json())
                    .then(json => {
                        if (json.status === "ok")
                            this.forums = json.forums;
                        else
                            alert("Error!");
                    })
            }
        },
        methods: {
            genLink() {
                fetch("http://192.168.43.244:5000/gen2", {
                    method: "POST",
                    headers: {
                        'Accept': 'application/json, text/plain, */*',
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        lang: this.lang,
                        forums: this.forums_selected
                    })
                })
                    .then(resp => resp.json())
                    .then(json => {
                        this.links = json.links;
                    })
            }
        }
    }
</script>

<style scoped>

</style>