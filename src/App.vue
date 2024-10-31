<script setup>
import { reactive } from 'vue';

const nome = "Ana"
const meuObj ={
  nome: "Ana",
  filmeFavorito: "Titanic"
}

function dizOla(nome) {
  return `Olá ${nome}!`
}

const imagemTitanic = "https://play-lh.googleusercontent.com/560-H8NVZRHk00g3RltRun4IGB-Ndl0I0iKy33D7EQ0cRRwH78-c46s90lZ1ho_F1so=w240-h480-rw"
const imagemEAssim = "https://br.web.img3.acsta.net/c_310_420/img/61/b3/61b35aa40057cba4f7df23c689d6979e.PNG"

const imagemPaoQueijo = "https://static.itdg.com.br/images/auto-auto/c191b11438b416fedce2c6edf6e74e3b/receitas-com-3-ingredientes.jpg"
const imagemBolo = "https://cdn.awsli.com.br/800x800/1345/1345272/produto/59442505/19c2592eb9.jpg"
const botaoDesabilitado = false;

const gostaDePaoQueijo = false
const gostaDeBolo = false 

const estaAutorizado = true

const estado = reactive({ //reactive é uma função que cria um objeto reativo. Ou seja, qualquer alteração feita no objeto estado refletirá automaticamente no DOM. Isso facilita o gerenciamento de estados em componentes Vue.
  contador:0,
  email: '',
  saldo: 5000,
  transferindo: 0,
  nomesArray : ['Kaio', 'Natália', 'Márcia', 'Rita'],
  nomeAInserir: '',
})

function incrementar(){
  estado.contador++;
}
function decrementar(){
  estado.contador--;
}

function alteraEmail(evento){
  estado.email = evento.target.value; //Ao ser chamada pelo evento @keyup no <input>, evento recebe o objeto do evento, o qual contém o valor atual do campo input acessível por evento.target.value.
}

function mostraSaldoFuturo(){
  const{ saldo, transferindo} = estado;
  return saldo - transferindo;
}
function alteraSaldo(evento){
  estado.transferindo = evento.target.value;
}

function validaTransferencia(){
  return estado.transferindo > estado.saldo
}
function cadastraNome(){  //Para inserir o nome no array nomesArray faz o push na função utilizando a variavel nomeAInderir estabelecida no objeto estado
  if(estado.nomeAInserir.length >= 3){
    estado.nomesArray.push(estado.nomeAInserir)
  }else{
    alert("Digite mais caracteres")
  }
  
}


</script>

<template>

  <h1> {{dizOla('Mari')}}</h1>
  <img v-bind:src="imagemTitanic" alt="">  <!--o v-bind: ou simplesmente : associa um paraametro do sript ao src-->
  <img :src="imagemEAssim" alt="">
  <button :disabled="botaoDesabilitado">Enviar!</button> 
  <img v-if="gostaDeBolo" :src="imagemBolo" alt="">
  <img v-else-if="gostaDePaoQueijo" :src="imagemPaoQueijo" alt="">
  <h2 v-else>Não gosta de nenhuma opção</h2>
<!-- Prestar atenção na ordem-->
  <h2 v-if="estaAutorizado"> Seja bem-vindo</h2> <!-- v-if, v-else-if e v-else são usadas para renderizar elementos condicionalmente  -->
  <h2 v-else>Não possui acesso</h2>

  <br>
  <hr>
  {{ estado.contador }}
  <button @click="incrementar" type="button">+</button>
  <button @click="decrementar" type="button">-</button>
  <br>
  <hr>

  {{ estado.email }}
  <input type="email" @keyup="alteraEmail">
  <br>
  <hr>

  Saldo: {{ estado.saldo }} <br>
  Transferindo: {{ estado.transferindo }} <br>
  Saldo depois da Transferência: {{ mostraSaldoFuturo() }} <br>
  <input :class="{ invalido: validaTransferencia() }" @keyup="alteraSaldo" type="number" placeholder="Quantia para transferir"> <br>  <!-- A classe só é aplicada quando a função retorna true -->
  <span v-if="validaTransferencia()">Valor maior que o saldo disponível</span> <!-- Se for true v-if aparece o span, se não aparece o botao -->
  <button v-else>Transferir</button>

  <br>
  <hr>Array Impresso em formato de lista <!-- Para imprimir listaArray não basta os mustachess, é necessário incluir o v-for na sua estrutura básica  -->
  <ul>
    <li v-for="nome in estado.nomesArray">
      {{ nome }}
    </li>
  </ul>
  <input  @keyup="evento => estado.nomeAInserir = evento.target.value" type="text" placeholder="Digite um novo nome">
  <button @click="cadastraNome" type="button">Cadastrar nome</button>

  <br>
  <hr>Array Impresso em formato de título
  <h3 v-for ="nome in estado.nomesArray">{{ nome }}</h3>
</template>

<style scoped>
img{
  max-width: 300px;
}

.invalido{
  outline-color: red;
  border-color: red;
}
</style>
