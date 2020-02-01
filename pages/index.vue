<template>
  <div class="page-container" >
    <div class="form-container" >
      <h1 class="title">
        Application Form
      </h1>
      <ValidationObserver v-slot="{ handleSubmit }">
        <form @submit.prevent="handleSubmit(save)" >
          <div v-for="(cmp, idx) in fields.components" >
            <component :is="dynamicComponent(cmp.component_type)" :info="cmp" @setVal="setAns($event, idx)" ></component>
          </div>
          <input class="form-button" type="submit" value="Submit" />
        </form>
      </ValidationObserver>
      <pre>To show values: {{answers}}</pre>
    </div>
  </div>
</template>

<script>
import textBox from '../components/textBox.vue';
import dropdownBox from '../components/dropdownBox.vue';
import { ValidationObserver, ValidationProvider, extend, localize } from 'vee-validate';
import axios from 'axios';

export default {
  components: {
    textBox,
    dropdownBox,
    ValidationObserver
  },
  data(){
    return {
      answers:[],
    }
  },
  async asyncData ({ params }) {
    const { data } = await axios.get(`https://gist.githubusercontent.com/vincentpaca/b66af9a67d2e7195847b281fa7f08f66/raw/46cb0fee1d8ff541ea0304c6702cc2e5e5a8c581/partner_form.json`)
    return { fields: data }
  },
  created(){
    this.answers = new Array(this.fields.components.length);
  },
  methods: {
      save() {
          console.log("save data..." + this.answers);
      },
      setAns(ans, idx){
        console.log("Setting the answer: ", ans);
        this.answers.splice(idx, 1, ans);
      },
      dynamicComponent(type){
        return type + "Box";
      }
  }
}
</script>


