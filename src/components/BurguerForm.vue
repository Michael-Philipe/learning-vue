<template>
  <div>
    <Message :msg="data.msg" v-show="data.msg" />
    <div>
      <form id="burger-form" @submit.prevent="createBurger">
        <div class="input-container">
          <label for="name">Nome do cliente</label>

          <input
            type="text"
            id="nome"
            name="name"
            v-model="data.nome"
            placeholder="Digite seu nome"
          />
        </div>
        <div class="input-container">
          <label for="pao">escolha seu pão</label>
          <select for="pao" id="pao" v-model="data.pao">
            <option value="">selecione seu pao</option>
            <option v-for="pao in data.paes" :key="pao.id" :value="pao.tipo">
              {{ pao.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="carne">escolha a carne</label>
          <select for="carne" id="carne" v-model="data.carne">
            <option value="">selecione sua carne</option>
            <option
              v-for="carne in data.carnes"
              :key="carne.id"
              :value="carne.tipo"
            >
              {{ carne.tipo }}
            </option>
          </select>
        </div>
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="opcionais"
            >Selecione os opcionais</label
          >
          <div
            v-for="opcional in data.opcionaisData"
            :key="opcional.id"
            class="checkbox-container"
          >
            <input
              type="checkbox"
              name="opcionais"
              v-model="data.opcionais"
              :value="opcional.tipo"
            />
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar meu Burguer" />
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import Message from '@/components/Message.vue';
import { onMounted, ref } from 'vue';

const data = ref({
  paes: '',
  carnes: '',
  opcionaisData: null,
  nome: '',
  pao: '',
  carne: '',
  opcionais: [],
  msg: '',
});

const getIngredientes = async () => {
  const req = await fetch('http://localhost:3000/ingredientes');
  const res = await req.json();

  data.value.paes = res.paes;
  data.value.carnes = res.carnes;
  data.value.opcionaisData = res.opcionais;
};
const createBurger = async () => {
  const inputData = {
    nome: data.value.nome,
    carne: data.value.carne,
    pao: data.value.pao,
    opcionais: Array.from(data.value.opcionais),
    status: 'Soliciado',
  };

  const dataJson = JSON.stringify(inputData);

  const req = await fetch('http://localhost:3000/burgers', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: dataJson,
  });

  const res = await req.json();
  //colocar msg de sistema

  data.value.msg = `Pedido N° ${res.id} realizado com sucesso`;

  //limpar msg

  setTimeout(() => {
    return (data.value.msg = '');
  }, 3000);

  //limpar campos
  data.value.nome = '';
  data.value.carne = '';
  data.value.pao = '';
  data.value.opcionais = [];
};

onMounted(() => {
  getIngredientes();
});
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.input-container {
  display: flex;
  flex-direction: column;

  width: 75%;
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
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
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
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
