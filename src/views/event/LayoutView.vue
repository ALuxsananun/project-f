<script setup lang="ts">
import { useAuthStore } from "@/stores/auth";
import { useStudentStore } from "@/stores/student";
import { storeToRefs } from "pinia";
import { ref } from "vue";

const store = useStudentStore();
const { student, professer } = storeToRefs(store);
const id = ref(student?.value?.id);

const authStore = useAuthStore();
</script>

<template>
  <div class="containerd">
    <div class="profile-section my-16">
      <!-- profile image -->
      <div v-if="student" class="flex justify-center my-auto">
        <img
          v-for="image in student?.image"
          :key="image"
          :src="image"
          alt="Student Image"
          class="border-4 border-black h-60 w-60 object-cover rounded-xl"
        />
      </div>
      <!-- student detail -->
      <div class="flex justify-center my-10">
        <div class="text-left font-mono grid grid-cols-2 text-xl">
          <p class="font-bold">Student-ID:</p>
          <p class="ml-3">{{ student?.studentID }}</p>
          <p class="font-bold">Name:</p>
          <p class="ml-3">{{ student?.name }} {{ student?.surname }}</p>
          <p class="font-bold">Department:</p>
          <p class="ml-3">{{ student?.department }}</p>
        </div>
      </div>
    </div>
    <div class="function-section my-16 border-2 rounded-md mr-16">
      <div>
        <RouterLink
          :to="{ name: 'adviser-detail', params: { id } }"
          class="font-mono font-semibold hover:text-red-800"
          active-class="active-link"
          exact-active-class="active-link"
        >
          Advisor Details
        </RouterLink>

        <RouterLink
          v-if="authStore.isStudent"
          :to="{ name: 'update-student', params: { id } }"
          class="font-mono font-semibold hover:text-red-800"
          active-class="active-link"
          exact-active-class="active-link"
        >
          | Edit Student Details
        </RouterLink>
        <RouterLink
          v-if="authStore.isStudent"
          :to="{ name: 'announcementView', params: { id } }"
          class="font-mono font-semibold hover:text-red-800"
          active-class="active-link"
          exact-active-class="active-link"
        >
          | Announcement
        </RouterLink>
        <RouterView :student="student" :professer="professer"></RouterView>
      </div>
    </div>
  </div>
</template>

<style scoped>
.containerd {
  display: flex;
}
.profile-section {
  width: 40%;
  padding: 20px;
}
.function-section {
  width: 60%;
  padding: 20px;
}
.active-link {
  color: red;
}
</style>
