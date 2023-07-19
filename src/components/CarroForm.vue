<template>
    <div>
        <form id="carro-form" @submit="createCarro">
            <div class="input-container">
                <label for="nome">Nome da pessoa:</label>
                <input type="text" id="nome" v-model="nome" placeholder="Informe o seu nome: ">
            </div>
            <div class="input-container">
                <label for="nome-carro">Nome do carro:</label>
                <input type="text" id="nome-carro" v-model="nomeCarro" placeholder="Informe o nome do carro: ">
            </div>
            <div class="input-container">
                <label for="placa">Placa do veículo:</label>
                <input type="text" id="placa" v-model="placa" placeholder="Informe a placa do veículo: ">
            </div>
            <div class="input-container">
                <label for="hora-entrada">Horário de entrada: </label>
                <input type="text" id="hora-entrada" v-model="hora" placeholder="Informe o horário de entrada: ">
            </div>
            <div class="input-container">
                <input type="submit" class="submit-btn" value="Cadastrar o carro">
            </div>
        </form>

        <Mensagem :msg="msg" v-show="msg" />

    </div>
</template>

<script>
import Mensagem from './Mensagem.vue';

export default {

    name: "CarroForm",

    data() {
        return {
            nome: '',
            nomeCarro: '',
            placa: '',
            hora: '',
            msg: ''
        }
    },
    methods: {
        async createCarro(e) {
            e.preventDefault();

            const data = {
                nome: this.nome,
                nomeCarro: this.nomeCarro,
                placa: this.placa,
                hora: this.hora,
                status: "Solicitado",

            }

            const dataJson = JSON.stringify(data)

            const req = await fetch("http://localhost:3000/carros", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json()
            console.log(res)

            this.msg = `Carro cadastrado com sucesso, obrigado!`
            setTimeout(() => this.msg = "", 3000)



            //Limpar campos

            this.nome = this.nomeCarro = this.placa = this.hora = ""
        }
    },
    components: {
        Mensagem
    }

}

</script>

<style scoped>
#carro-form {
    max-width: 300px;
    margin: 0 auto;
}

.input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #bf3f3f;
}

input {
    padding: 5px 10px;
    width: 300px;
}

.submit-btn {
    background-color: #bf3f3f;
    color: white;
    font-weight: bold;
    padding: 10px;
    font-size: 16px;
    border: 2px;
    cursor: pointer;
    transition: .5s;
}

.submit-btn:hover {
    background-color: tomato;
    font-size: 18px;
}
</style>