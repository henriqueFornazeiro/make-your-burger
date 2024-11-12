<template>
    <div>
        <p>Componente de mensagem</p>
        <div>
            <form id="form" @submit="createBurger">
                <div class="input-container">
                    <label for="nome">Nome do cliente</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha o pão</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="" selected>Selecione seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="cane">Escolha a carne</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione seu carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
                    </select>
                </div>
                <div class="input-container" id="opcionais-container">
                    <label for="opcionais" id="opcionais-title">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.tipo">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar meu Burger!">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Formulario',
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisData: null,
            nome: null,
            carne: null,
            opcionais: []
        }
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes")
            const data = await req.json()

            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisData = data.opcionais;

            console.log("Data", data)
        },

        async createBurger(e) {
            e.preventDefault()
            
            if((!this.nome || this.nome == "") || (!this.carne || this.carne == "") || (!this.pao || this.pao == "")){
                alert("Preencha todos os campos, por favor!")

                return
            }

            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            };

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            })

            const res = await req.json()

            this.nome = ""
            this.carne = ""
            this.pao = ""
            this.opcionais =[]
        }
    },
    mounted() {
        this.getIngredientes()
    }
}
</script>

<style scoped>
#form {
    max-width: 400px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
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
    border-left: 4px solid #fcba03;
}

input,
select {
    padding: 5px 10px;
    width: 300px;
}

#opcionais-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
}

#opcionais-title {
    width: 100%;
}

.checkbox-container {
    display: flex;
    align-items: center;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
    width: auto;
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold
}

.submit-btn {
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    width: 100%;
}

.submit-btn:hover {
    background-color: transparent;
    color: #222
}
</style>