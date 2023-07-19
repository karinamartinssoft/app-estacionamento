<template>
    <div id="carro-table">
        <Mensagem :msg="msg" v-show="msg" />
        <div>
            <div id="carro-table-heading">
                <div class="order-id">#:</div>
                <div>Nome da pessoa: </div>
                <div>Nome do carro: </div>
                <div>Placa do carro: </div>
                <div>Horário de entrada: </div>
                <div>Ações: </div>
            </div>
        </div>
        <div id="carro-table-rows">
            <div class="carro-table-row" v-for="carro in carros" :key="carros_id">
                <div class="order-number">{{ carros_id }}</div>
                <div>{{ carro.nome }}</div>
                <div>{{ carro.nomeCarro }}</div>
                <div>{{ carro.placa }}</div>
                <div>{{ carro.hora }}</div>
                <div>
                    <select name="status" class="status" @change="updateCarro($event, carro.id)">
                        <option value="">Status carro</option>
                        <option :value="status.tipo" v-for="statu in status" :key="statu.id"
                            :selected="carro.status == statu.tipo">{{ statu.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deletarCarro(carro.id)">Delete</button>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import Mensagem from './Mensagem.vue';
export default {

    name: "Dashboard",
    data() {
        return {
            carros: '',
            carros_id: '',
            status: [],
            msg: ''


        }
    },
    components: {
        Mensagem
    },
    methods: {
        async getCadastros() {
            const req = await fetch("http://localhost:3000/carros")
            const data = await req.json()
            this.carros = data
            this.getStatus()
        },
        async getStatus() {
            const req = await fetch("http://localhost:3000/status")
            const data = await req.json()
            this.status = data
        },
        async deletarCarro(id) {
            const req = await fetch(`http://localhost:3000/carros/${id}`, {
                method: "DELETE"
            })

            const data = await req.json()
            this.msg = `Carro removido com sucesso. Obrigado`
            this.getCadastros()
        },
        async updateCarro(event, id) {
            const opcoes = event.target.value
            const dataJson = JSON.stringify({ status: opcoes })
            const req = await fetch(`http://localhost:3000/carros/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            })

            const res = await req.json()

            this.msg = `O carro de ${res.nome} foi atualizado para ${res.status}. Obrigado`

            setTimeout(() => this.msg = '', 3000)


        }
    },
    mounted() {
        this.getCadastros()

    }
}

</script>

<style>
#carro-table {
    max-width: 1500px;
    margin: 0;
}

#carro-table-heading,
#carro-table-rows .carro-table-row {
    display: flex;
    flex-wrap: wrap;
}

#carro-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid rgba(53, 30, 180);
}

#carro-table-heading div,
.carro-table-row div {
    width: 19%;
}

.carro-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid rgba(53, 30, 180);
}

#carro-table-heading .order-id,
.carro-table-row .order-number {
    width: 5%;
}

select {
    padding: 10px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #bf3f3f;
    color: white;
    font-weight: bold;
    padding: 10px;
    font-size: 16px;
    border: 2px;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    background-color: tomato;
    font-size: 18px;
}
</style>