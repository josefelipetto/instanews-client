<template>
    <b-navbar toggleable="md" type="dark" variant="info">

        <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>

        <b-navbar-brand href="/">InstaNews</b-navbar-brand>

        <b-collapse is-nav id="nav_collapse">
             
            <!-- Right aligned nav items -->
            <b-navbar-nav class="ml-auto">

            <b-nav-form>
                <b-form-input size="sm" class="mr-sm-2" type="text" placeholder="Buscar" v-model="text1"/>
                <b-button size="sm" class="my-2 my-sm-0" @click="search">Buscar</b-button>
                
            </b-nav-form>

            {{text}}
            <b-nav-item-dropdown right>
                <!-- Using button-content slot -->
                <template slot="button-content">
                <em>Usuário</em>
                </template>
                <b-dropdown-item href="/profile">Perfil</b-dropdown-item>
                <b-dropdown-item href="/login" >Entrar</b-dropdown-item>
                <b-dropdown-item href="/register" >Registrar</b-dropdown-item>
                <b-dropdown-item href="#">Sair</b-dropdown-item>
            </b-nav-item-dropdown>
            </b-navbar-nav>

        </b-collapse>

       
    </b-navbar>


</template>

<script>
export default{
    data(){
        return {
            text1 : ''
        }
    },
    methods:{
        search(){
            this.$router.push('/search/' + this.text1)
        }
    }
}
</script>



