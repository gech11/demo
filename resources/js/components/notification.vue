<template>
  <div>
  <button @click="notificationPopup=true" class="relative flex">
   <svg xmlns="http://www.w3.org/2000/svg" class="text-white w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
   </svg>
   <span v-if="notifications.length" class="absolute top-0 right-0 w-6 h-6 font-semibold text-white text-lg text-white">{{notifications.length}}</span>
  </button>
   <div v-if="notificationPopup" class="fixed bg-gray-100 z-20 text-xl m-2 rounded-md p-2">
    <ul>
     <li v-for="notification in notifications" :key="notification.id" class="hover:bg-blue-200">
      <a :href="notification.data.href">{{notification.data.data}}</a>
      <button @click="readNotification(notification.id)">x</button>
     </li>
    </ul>
   </div>
    <div v-if="notificationPopup" @click="notificationPopup=false" class="absolute -inset-x-0 -inset-y-full opacity-50 bg-black z-10"></div>
  </div>
</template>
<script setup>
defineProps({userId})

let notifications=$ref([])
let selectedCategories=$ref([])
let notificationPopup=$ref(false)

onMounted(() => {
    axios.get('/user/notifications')
     .then(res=>{
        notifications=res.data.notifications
     })
})

const unreadNotification=(notificationId)=>{
    axios.post('/user/notification/unread',{notificationId:notificationId,userId:userId})
    .then(res=>{
        let index=notifications.findindex(item=> item.id==notificationId)
        notifications.splice(index,1)
    })
}

</script>
