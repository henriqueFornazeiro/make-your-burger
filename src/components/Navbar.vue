<template>
    <nav>
        <router-link to="/" id="logo-url">
            <img :src="logo" :alt="alt" id="logo">
        </router-link>
        <router-link to="/">Home</router-link>
        <router-link to="/pedidos">Pedidos</router-link>
    </nav>
</template>

<script>
export default {
    name: 'Navbar',
    props:["logo", "alt"]
}
</script>

<style scoped>
    nav{
        background-color: #222;
        border-bottom: 4px solid #111;
        padding: 15px 50px;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }

    nav a{
        color: #fcba03;
        text-decoration: none;
        margin: 12px;
        transition: .2s;
    }

    nav a:hover{
        color: #fcba03da;
    }

    #logo-url{
        margin: auto;
        margin-left: 0;
    }

    #logo{
        width: 40px;
        height: 40px
    }

    
</style>