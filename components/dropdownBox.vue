<template>
    <div class="fieldGroup" >
        <label :for="info.name" class="form-label" >{{info.name}}*</label>
        <ValidationProvider rules="required" v-slot="{ classes, errors }">
            <select v-model="val" @change="updateVal" class="form-field" :class="classes" >
                <option v-for="opt in info.data" :value="opt.id" >{{opt.value}}</option>
            </select>
            <span class="errMsg" >{{ errors[0] }}</span>
        </ValidationProvider>
    </div>    
</template>
<script>
    import { ValidationProvider } from 'vee-validate';
    import { extend } from 'vee-validate';
   
    extend('required', {
        validate (value) {
            return {
            required: true,
            valid: ['', null, undefined].indexOf(value) === -1,
            };
        },
        message: 'This field is required.',
        computesRequired: true
    });

    export default {
        props: [
            'info'
        ],
        components: {
          ValidationProvider
        },
        data: function () {
            return {
                val: ''
            }
        },
        methods: {
            updateVal() {
                this.$emit('setVal' , this.val);
            },
        }
    }
</script>
<style lang="scss">

</style>

