<template>
  <div>
    <h1>WELCOME TO YOUR DASHBOARD</h1>
    <button @click="logout" class="logout-btn">Logout</button>
    
    <FileUpload />
  </div>
</template>

<script>
import FileUpload from '../components/FileUpload.vue';
import { useRouter } from 'vue-router';

export default {
  components: { FileUpload },
  setup() {
    const router = useRouter();

    const logout = () => {
      localStorage.removeItem("token");  // Clear authentication token
      router.push("/");  // Redirect to login page
    };

    return { logout };
  }
};
</script>

<style>
h1 {
  color: #73f5c3;
  text-shadow: 0 0 10px rgba(50, 255, 126, 0.5);
  text-align: center;
  font-weight: 500;
  letter-spacing: 1px;
}
.logout-btn {
  display: block;
  margin: 20px auto;
  padding: 10px 15px;
  background-color: #ff4d4d;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.3s;
}
.logout-btn:hover {
  background-color: #cc0000;
}
</style>
