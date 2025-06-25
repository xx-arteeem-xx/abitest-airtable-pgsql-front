<script>
    export default {
        methods: {
            logout(){
                document.cookie = 'api_token=; Max-Age=-1;';
                location.href = "./login";
            }, 
            async authCheck() {
                const headers = new Headers();
                headers.append('Access-Control-Allow-Origin', '*');
                headers.append('Access-Control-Allow-Credentials', 'true');
                headers.append('GET', 'POST', 'OPTIONS');
                headers.append('Authorization', `Bearer ${document.cookie.split("=")[1]}`);
                const url = `${import.meta.env.VITE_API_URL_AUTH}/api/auth/`;

                // || __________________  ОТПРАВЛЯЕМ ЗАПРОС  _______________________ || 
                const response = await fetch(url, {
                    method: 'GET',
                    headers: headers
                });

                // || __________________  ПОЛУЧАЕМ ОТВЕТ  _______________________ || 
                const result = await response.json();
                return result;
            }
        },
        async mounted() {
            try {
                const api_token_name = document.cookie.split("=")[0];
                if (api_token_name !== "api_token") {
                    location.href = "./login"
                } else {
                    const result = await this.authCheck();
                    if (result.error) {
                        this.logout();
                    };
                }
            } catch (error) {
                this.logout()
            }
        },
    }
</script>

<template>
    <div class="container my-5 text-center bg-body-tertiary rounded-3 p-5 d-flex flex-column align-items-center">
        <div class="img mb-3">
            <img src="../assets/logo.png" id="logo">
        </div>
        <h1 class="display-5 fw-bold">
            Синхронизировать Airtable и PostgresQL
        </h1>
        <p class="mx-auto fs-4 text-muted">
            Данный сервис предназначен для синхронизации админки в airtable и базы данных PostgresQL.
        </p>
        <div class="d-inline-flex gap-2 mb-5">
            <button class="btn btn-info btn-lg px-4" type="button" @click="storeToDB()">
                Синхронизировать!
            </button>
            <button class="btn btn-warning btn-lg px-4" type="button" @click="logout()">
                Выйти
            </button>
        </div>
    </div>
</template>

<style>
    #logo{
        width: 100%;
        height: auto;
    }
    .img{
        width: 300px;
    }
</style>