<template>
    <div>
        <div><h1 class="py-3">Fill Form</h1></div>
        <div class="d-flex justify-content-center">
            <b-form  style="width:40%;" v-on:submit.prevent >
            <b-form-group class="text-left"  label="Email address:" >
                <b-form-input type="email" required placeholder="Enter email" v-model="formEmail">                   
                </b-form-input>  
            </b-form-group>
             <b-form-group class="text-left"  label="Name:" >
                <b-form-input type="text" required placeholder="Enter Name" v-model="formName">                   
                </b-form-input>  
            </b-form-group>
             <b-form-group class="text-left" label="DOB:" >
                <b-form-input type="date" required v-model="formDob" >                   
                </b-form-input>  
            </b-form-group>
            <b-form-group class="text-left" label="Password:" >
                <b-form-input type="password" required placeholder="Enter Password">                   
                </b-form-input>  
            </b-form-group>
            <b-form-group class="text-left" label="Confirm Password:" >
                <b-form-input type="password" required placeholder="Enter Confirm Password">                   
                </b-form-input>  
            </b-form-group>
            <b-form-checkbox name="checkbox-1" value="accepted" unchecked-value="not_accepted">
                I accept the terms and conditions
            </b-form-checkbox>
            <div class="text-center py-3" >
            <b-button @click="added">Submit</b-button>
            </div>
            <p>{{formEmail}}</p>
        <p>{{formName}}</p>
        <p>{{formDob}}</p>
        </b-form>
        
        </div>
        <div class="p-5">
            <h1 class="py-3">Filled Details</h1>
            <b-table hover :items="items" :fields="fields"></b-table>
        </div>
        
    </div>
</template>

<script>
export default {
    name:'Form',
    data:() =>{
        return{
            fields: ['email', 'name', 'dob','edit','delete'],
            items:[  ],
            formEmail:'',
            formName:'',
            formDob:'',
            value:{email:"ashwani@gmail.com",name:'Ashwani',dob:' 22/03/1997',edit:'Edit',delete:'Delete'}
            }
        },
    methods:{
        added(){
            //eslint-disable-next-line
            console.log(this.items)
            this.items.push(this.value);
        }   
    }
}
</script>