<template>

    <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
    <el-menu-item class="flow-left">
        <a href="#"><img src="../assets/logo.png" width="70px" height="70px" alt="HRM Logo" class="img-fluid logo"></a>
    </el-menu-item>
        <el-menu-item class="flow-right" index="logout"><a href="/api/account/sign_out">Logout</a></el-menu-item>
        <el-menu-item class="flow-left" v-for="item in availablePages" :index="item" :key="item.id">{{ item | capitalize }}</el-menu-item>
    </el-menu>
</template>

<script>
    const allPages = {
        patient: ['appointments', 'specialists'],
        specialist: ['appointments', 'specialists', 'patients'],
        receptionist: ['appointments', 'specialists', 'patients'],
    };
    import { mapGetters } from 'vuex';
    export default {
        data() {
            return {
                activeIndex: 'specialists'
            }
        },
        computed: {
            ...mapGetters(['userType']),
            availablePages() {
                return allPages[this.userType] || [];
            }
        },
        filters: {
          capitalize: function (value) {
            if (!value) return '';
            value = value.toString();
            return value.charAt(0).toUpperCase() + value.slice(1);
          }
        },
        methods: {
            handleSelect(key) {
                if (!['logout', this.$route.name].includes(key)) {
                    this.$router.push(`/${key}`);
                }
            }
        },
        mounted() {
            if (['appointments', 'specialists', 'patients'].includes(this.$route.name)) {
                this.activeIndex = this.$route.name;
            }
        }
    }
</script>

<style>
    .flow-right {
        float: right !important;
    }
    .flow-left {
        float: left !important;
    }
</style>