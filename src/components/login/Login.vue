<script>
    export default {
        data(){
            return {
                username: "",
                password: "",
                error: "", 
            }
        },
        methods: {
            async auth(){
                try {
                    const data = {
                        "username": this.username,
                        "password": this.password,
                        
                    }
                    // || __________________  ЗАДАЕМ ЗАГОЛОВКИ ЗАПРОСА  _______________________ || 
                    const headers = new Headers();
                    headers.append('Content-Type', 'application/json');
                    headers.append('Accept', 'application/json');
                    headers.append('Access-Control-Allow-Origin', '*');
                    headers.append('Access-Control-Allow-Credentials', 'true');
                    headers.append('GET', 'POST', 'OPTIONS');
                    const url = `${import.meta.env.VITE_API_URL_AUTH}/api/auth/login/`

                    // || __________________  ОТПРАВЛЯЕМ ЗАПРОС  _______________________ || 
                    const response = await fetch(url, {
                        method: 'POST',
                        headers: headers,
                        body: JSON.stringify(data)
                    });

                    // || __________________  ПОЛУЧАЕМ ОТВЕТ  _______________________ || 
                    const result = await response.json();

                    // || __________________  ОБРАБАТЫВАЕМ ОТВЕТ  _______________________ || 
                    if (result.error) {
                        this.error = result.error;
                    } else if (result.token) {
                        document.cookie = `api_token=${result.token}`;
                        this.error = "";
                        location.href = "./"
                    } else {
                        this.error = "Internal server error!"
                    };
                } catch (error) {
                    this.error = "Internal server error!";
                }
            }
        }, 
        mounted() {
            const api_token_name = document.cookie.split("=")[0];
            if (api_token_name === "api_token") {
                location.href = "./"
            }
        },
    }
</script>

<template>
    <div class="container">
        <div class="row d-flex justify-content-center mt-5">
            <div class="col-12 col-md-8 col-lg-6 col-xl-5">
                <div class="bg-body-tertiary p-5 rounded-3">
                    <div class="alert alert-danger" role="alert" v-if=" error != ''">
                        {{ error }}
                    </div>
                    <h3 class="fs-2 mb-4">
                        Вход в синхронизатор
                    </h3>
                    <div class="input-group mb-3">
                        <span class="input-group-text">
                            <i class="fa fa-user" aria-hidden="true"></i>
                        </span>
                        <input type="text" class="form-control" v-model="username" placeholder="Имя пользователя">
                    </div>
                    <div class="input-group mb-3">
                        <span class="input-group-text">
                            <i class="fa fa-key" aria-hidden="true"></i>
                        </span>
                        <input type="password" class="form-control" v-model="password" placeholder="Пароль">
                    </div>
                    <button class="btn btn-info" @click="auth">
                        Войти
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style>

</style>