<template>
    <div>
        <!-- Main Content -->
        <main class="main-box">
            <router-view></router-view>
        </main>

        <!-- Footer -->
        <footer class="footer-box">
            &copy; 2024 IMU-Talk-Pure. All rights reserved.
        </footer>
    </div>
</template>

<script>
import { ref, computed, watch } from 'vue';
import { useRoute, useRouter } from 'vue-router';

export default {
    name: 'App',
    components: {
        
    },
    setup() {
        
    }
}
</script>

<style>
@import 'assets/BaseStyle.css';

.main-box {
    display: flex;
    box-sizing: border-box;
    align-items: center;
    justify-content: center;
    padding: 10px;
    width: 100%;
    min-height: 97vh;
}

.footer-box {
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>