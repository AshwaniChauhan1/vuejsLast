<template>
    <div>
        <div><h1 class="py-3">Fill Form</h1></div>
        <div class="d-flex justify-content-center">
            <b-form  style="width:40%;" v-on:submit.prevent >
                <p style="color:red">
                  {{error}}  
                </p>
            <b-form-group class="text-left"  label="Email address:" >
                <b-form-input type="email" required placeholder="Enter email" v-model="form.email">                   
                </b-form-input>  
            </b-form-group>
             <b-form-group class="text-left"  label="Name:" >
                <b-form-input type="text" required placeholder="Enter Name" v-model="form.name">                   
                </b-form-input>  
            </b-form-group>
             <b-form-group class="text-left" label="DOB:" >
                <b-form-input type="date" required v-model="form.dob" >                   
                </b-form-input>  
            </b-form-group>
            <b-form-group class="text-left" label="Password:" >
                <b-form-input type="password" placeholder="Enter Password" v-model="password">                   
                </b-form-input>  
            </b-form-group>
            <b-form-group class="text-left" label="Confirm Password:" >
                <b-form-input type="password" placeholder="Enter Confirm Password" v-model="cpassword">                   
                </b-form-input>  
            </b-form-group>
            <b-form-checkbox name="checkbox-1" value="accepted" unchecked-value="not_accepted" v-model="status">
                I accept the terms and conditions
            </b-form-checkbox>
            <div class="text-center py-3" >
            <b-button @click="added">Submit</b-button>
            </div>
        </b-form>
        
        </div>
        <div class="p-5">
            <h1 class="py-3">Filled Details</h1>
            <b-table hover :items="items" :fields="fields">
                <template v-slot:cell(edit)="row">
                     <a @click="edit(row)">Edit</a>
                </template>
                <template v-slot:cell(delete)="row">
                    <a @click="del(row)">Delete</a>
                </template>
            </b-table>
        </div>
        
    </div>
</template>

<script>
export default {
    name:'Form',
    data:() =>{
        return{
            error:'',
            fields: ['email', 'name',{key:'dob',sortable:true},'edit','delete'],
            items:[  ],
            form:{email:"",name:"",dob:""},
            val:{},
            password:'',
            cpassword:'',
            status:"not_accepted",
            editIndex:false,
            indexval:''
            }
        },
    methods:{
        added(){
            if (this.form.email =='') {
            this.error='Email required.';
            }
            else if(this.form.name ==''){
                this.error='Name required.';
            }
            else if(this.form.dob ==''){
                this.error='DOB required.';
            }
            else if(this.password =='' || this.cpassword ==''){
                this.error='Password required.';
            }
            else if(this.password != this.cpassword){
                this.error='Password not match';
            }
            else if(this.status != "accepted"){
                this.error='please read terms and conditions';
            }
            else{
                if (this.editIndex ==true) {
                //eslint-disable-next-line
                console.log(this.items[this.indexval]);
                this.val={email:this.form.email,name:this.form.name,dob:this.form.dob,edit:"Edit",delete:"Delete"};
                this.items.splice(this.indexval,1,this.val);
                this.editIndex = false;
                this.form.email='';
                this.form.name='';
                this.form.dob='';
              }
                else{
                this.error='';
                this.val={email:this.form.email,name:this.form.name,dob:this.form.dob,edit:"Edit",delete:"Delete"};
                this.items.push(this.val);
                this.form.email='';
                this.form.name='';
                this.form.dob='';
                this.password='';
                this.cpassword='';
                this.status="not_accepted";   
                }
            }
             
        },
        del(item){
            this.$delete(this.items,item.index );
        },
        edit(item){
            this.form.email=item.item.email;
            this.form.name=item.item.name;
            this.form.dob=item.item.dob;
            this.editIndex=true;
            this.indexval =item.index;
            //eslint-disable-next-line
            // console.log(this.indexval);
         
        }
        
      
    }
}
</script>

<style scoped>
</style>