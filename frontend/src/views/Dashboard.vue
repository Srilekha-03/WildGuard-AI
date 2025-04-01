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
  color: #83e6ba;
  text-shadow: 5px 5px 10px rgba(50, 255, 125, 0.222);
  text-align: center;
  font-weight: 1000;
  letter-spacing: 1px;
}
.logout-btn {
  display: block;
  margin: 20px auto;
  padding: 10px 15px;
  background-color: #b52525;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.3s;
}
.logout-btn:hover {
  background-color: #842525;
}
</style>
