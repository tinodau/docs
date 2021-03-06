<template>
  <v-card flat>
    <v-snackbar
      v-model="snackbar"
      absolute
      top
      right
      color="success"
    >
      <span>Registration successful!</span>
      <v-icon dark>check_circle</v-icon>
    </v-snackbar>
    <v-form @submit.prevent="submit" ref="form">
      <v-container grid-list-xl fluid>
        <v-layout wrap>
          <v-flex xs12 sm6>
            <v-text-field
              color="purple darken-2"
              label="First name"
              required
              v-model="form.first"
              :rules="rules.name"
            ></v-text-field>
          </v-flex>
          <v-flex xs12 sm6>
            <v-text-field
              color="blue darken-2"
              label="Last name"
              v-model="form.last"
              required
              :rules="rules.name"
            ></v-text-field>
          </v-flex>
          <v-flex xs12>
            <v-text-field
              color="teal"
              multi-line
              v-model="form.bio"
            >
              <div slot="label">
                Bio <small>(optional)</small>
              </div>
            </v-text-field>
          </v-flex>
          <v-flex xs12 sm6>
            <v-select
              color="pink"
              label="Favorite animal"
              v-model="form.favoriteAnimal"
              required
              :items="animals"
              :rules="rules.animal"
            ></v-select>
          </v-flex>
          <v-flex xs12 sm6>
            <v-slider
              color="orange"
              label="Age"
              hint="Be honest"
              min="1"
              max="100"
              thumb-label
              v-model="form.age"
              :rules="rules.age"
            ></v-slider>
          </v-flex>
          <v-flex xs12>
            <v-checkbox
              color="green"
              v-model="form.terms"
            >
              <div slot="label" @click.stop="">
                Do you accept the
                <a href="javascript:;" @click.stop="terms = true">terms</a>
                and
                <a href="javascript:;" @click.stop="conditions = true">conditions?</a>
              </div>
            </v-checkbox>
          </v-flex>
        </v-layout>
      </v-container>
      <v-card-actions>
        <v-btn flat @click="resetForm">Cancel</v-btn>
        <v-spacer></v-spacer>
        <v-btn
          flat
          color="primary"
          type="submit"
          :disabled="!formIsValid"
        >Register</v-btn>
      </v-card-actions>
    </v-form>
    <v-dialog v-model="terms" width="70%">
      <v-card>
        <v-card-title class="title">Terms</v-card-title>
        <v-card-text v-for="n in 5" :key="n">
          {{ content }}
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            flat
            color="purple"
            @click="terms = false"
          >Ok</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="conditions" width="70%">
      <v-card>
        <v-card-title class="title">Conditions</v-card-title>
        <v-card-text v-for="n in 5" :key="n">
          {{ content }}
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            flat
            color="purple"
            @click="conditions = false"
          >Ok</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>
<script>
  export default {
    data () {
      const defaultForm = Object.freeze({
        first: '',
        last: '',
        bio: '',
        favoriteAnimal: '',
        age: null,
        terms: false
      })

      return {
        form: Object.assign({}, defaultForm),
        rules: {
          age: [
            val => val < 10 || `I don't believe you!`
          ],
          animal: [val => (val || '').length > 0 || 'This field is required'],
          name: [val => (val || '').length > 0 || 'This field is required']
        },
        animals: ['Dog', 'Cat', 'Rabbit', 'Turtle', 'Snake'],
        conditions: false,
        content: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nec odio. Praesent libero. Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet. Duis sagittis ipsum. Praesent mauris. Fusce nec tellus sed augue semper porta. Mauris massa. Vestibulum lacinia arcu eget nulla. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Curabitur sodales ligula in libero. Sed dignissim lacinia nunc.`,
        snackbar: false,
        terms: false,
        defaultForm
      }
    },

    computed: {
      formIsValid () {
        return (
          this.form.first &&
          this.form.last &&
          this.form.favoriteAnimal &&
          this.form.terms
        )
      }
    },

    methods: {
      resetForm () {
        this.form = Object.assign({}, this.defaultForm)
        this.$refs.form.reset()
      },
      submit () {
        this.snackbar = true
        this.resetForm()
      }
    }
  }
</script>
