<template>
    
<div class="flex flex-col justify-center items-center   h-auto w-auto px-3 hover:cursor-pointer noprint" v-cloak  id="notifications" @click="showNotifications" >
   <div class=" text-black absolute top-3"> <font-awesome-icon icon="fa-regular fa-bell" size="3x"/> </div>
   <div class=" text-rose-500 absolute top-10   " v-if="notificationStore.getNotifications.length > 0"><font-awesome-layers  ><font-awesome-icon icon="fa-solid fa-square "  size="2x"/><font-awesome-layers-text :value="notificationStore.getNotifications.length"  class="text-black text-sm  pl-2"/></font-awesome-layers></div>
  
</div>





</template>




<script>
import axios from 'axios'
export default {

data(){
    return {

        notificationsCheck:false,
        polling:null
    }
},


created(){
    this.getNotifications()

},


methods:{



        getToSource(event, notification){
          
          var vm = this;
          var token = this.authStore.getToken;
          var user = this.authStore.getUser;
          var id = notification._id

      
          var data = new FormData();
          data.append("token", token);
          data.append("id", id);
          data.append("user", JSON.stringify(user))
          
          
          axios.post(vm.globalUrl + 'deleteNotification', data).then((result)=>{
   
          
            vm.getNotifications();

            if(notification.type == 'chat'){
              vm.$router.push("/ticketing/ticketDetails/" +notification.ticketId + "?chat=true")
            }else{
              
              vm.$router.push("/ticketing/ticketDetails/" +notification.ticketId)
            }

                 
            

  
      
          
          
          
          }).catch((error)=>{vm.$toast.warning(error)})

         
         
         
  

        },

       pollData(){
        var vm = this;
        this.polling = setInterval(()=>{
          vm.getNotifications()
        }, 10000)
       },


       beforeDestroy(){
        clearInterval(this.polling)
       },


       showNotifications(){

    
        this.mainStore.toggleNotification()
       }




    

    

    
}



}





</script>