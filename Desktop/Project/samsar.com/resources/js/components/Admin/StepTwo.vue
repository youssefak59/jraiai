<template>
  <div>
    <div class="container">
      <div class="field">
        <label class="label">Username</label>
        <div class="control">
          <input
            :class="['input', ($v.form.username.$error) ? 'is-danger' : '']"
            type="text"
            placeholder="Text input"
            v-model="form.username"
          >
        </div>
        <p v-if="$v.form.username.$error" class="help is-danger">This username is invalid</p>
      </div>
      <div class="field">
        <label class="label">Email</label>
        <div class="control">
          <input
            :class="['input', ($v.form.demoEmail.$error) ? 'is-danger' : '']"
            type="text"
            placeholder="Email input"
            v-model="form.demoEmail"
          >
        </div>
        <p v-if="$v.form.demoEmail.$error" class="help is-danger">This email is invalid</p>
      </div>
      <div class="field">
        <label class="label">Message</label>
        <div class="control">
          <textarea
            :class="['textarea', ($v.form.message.$error) ? 'is-danger' : '']"
            placeholder="Textarea"
            v-model="form.message"
          ></textarea>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { validationMixin } from "vuelidate";
import { required, email } from "vuelidate/lib/validators";
export default {
  props: ["clickedNext", "currentStep", "username"],
  mixins: [validationMixin],
  data() {
    return {
      form: {
        username: "",
        demoEmail: "",
        message: ""
      }
    };
  },
  validations: {
    form: {
      username: {
        required
      },
      demoEmail: {
        required,
        email
      },
      message: {
        required
      }
    }
  },
  watch: {
    $v: {
      handler: function(val) {
        if (!val.$invalid) {
          this.$emit("can-continue", { value: true });
        } else {
          this.$emit("can-continue", { value: false });
        }
      },
      deep: true
    },
    clickedNext(val) {
      if (val === true) {
        this.$v.form.$touch();
      }
    }
  },
  mounted() {
    if (!this.$v.$invalid) {
      this.$emit("can-continue", { value: true });
    } else {
      this.$emit("can-continue", { value: false });
    }
  }
};
</script>