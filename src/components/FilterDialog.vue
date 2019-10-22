<template>
  <v-dialog v-model="dialog" max-width="480">
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-card>
        <v-card-title primary-title>
          <span class="title" v-text="title" />
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="12" class="py-0">
              <v-select
                v-model="form.subject"
                :items="subjects"
                label="Subject"
              />
            </v-col>
            <v-col cols="12" class="py-0">
              <v-text-field
                v-model="form.keyword"
                :rules="keywordRules"
                label="Keyword"
                placeholder="Words or Pattern"
                required
              />
            </v-col>
            <v-col cols="12" class="py-0">
              <v-checkbox v-model="form.regExp" label="Regular Expression" />
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click.native="onCloseClick">Cancel</v-btn>
          <v-btn color="primary" text @click.native="onSaveClick">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-form>
  </v-dialog>
</template>

<script>
export default {
  props: {
    value: {
      type: Boolean,
      required: true
    },
    inputs: {
      type: Object,
      default: () => {}
    },
    title: {
      type: String,
      default: 'Add Rule'
    }
  },
  data() {
    return {
      subjects: [
        { text: 'Author', value: 'author' },
        { text: 'Message', value: 'message' }
      ],
      keywordRules: [(v) => !!v || 'Keyword is required'],
      valid: false,
      dialog: false,
      form: {}
    }
  },
  watch: {
    value(value) {
      this.dialog = value
      if (value) {
        this.form = {
          subject: 'message',
          ...this.inputs
        }
      }
    },
    dialog(value) {
      if (!value) {
        this.$emit('update:inputs', null)
      }
      this.$emit('input', value)
    }
  },
  methods: {
    onCloseClick() {
      this.$emit('update:inputs', null)
      this.$emit('input', false)
    },
    onSaveClick() {
      if (!this.$refs.form.validate()) {
        return
      }
      this.$emit('update:inputs', { ...this.form })
      this.$emit('input', false)
    }
  }
}
</script>