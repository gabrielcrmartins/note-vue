<template>
    <div id="app">
        <h1> Tarefas </h1>

        <div class="formulario">
            <input
                v-model="novaTarefa"
                @keyup.enter="addTarefa"
                placeholder="Digite a tarefa"
                type="text">
            
            <button @click="addTarefa">
                <img src="@/assets/add.png" alt="Adicionar Tarefa" class="icone">
                Adicionar
            </button>
        </div>

    <ul class="lista">
        <li v-for="tarefa in tarefas" :key="tarefa.id">
            <input type="checkbox"
            :checked="tarefa.concluida"
            @change="alternarStatus(tarefa.id)">
            <span :class="{ concluida: tarefa.concluida }">
                {{  tarefa.texto }}
            </span>
            <button @click="deletarTarefa(tarefa.id)" 
            class="deletar">
            </button>
        </li>
    </ul>

    <p class="contador">
        {{  tarefasPendentes }} tarefa(s) pendente(s)
    </p>
</div>
    </template>

<script>
export default {
    name: 'App',
    data() {
        return {
            novaTarefa: '',
            tarefas: []
        }
    },

    computed: {
        tarefasPendentes() {
            return this.tarefas.filter(t => !concluida).length
        }
    },

    methods: {
        adicionarTarefa() {
            if (this.novaTarefa.trim() === '') return 
        
            const tarefa = {
                id: Date.now(), 
                text: this.novaTarefa,
                concluida: false
            }

            this.tarefa.push(tarefa)
            this.novaTarefa = ''
            this.salvarNoLocalStorage()
        },

        alterarStatus(id) {
            const tarefa = this.tarefa.find(t => t.id === id)
            if(tarefa) {
                tarefa.concluida = !tarefa.concluida
                this.salvarNoLocalStorage()
            }
        },

        deletarTarefa(id) {
            this.tarefas = this.tarefas(t => t.id !== id)
            this.salvarNoLocalStorage()
        },

        salvarNoLocalStorage() {
            localStorage.setItem('tarefas', JSON.stringify(this.tarefas))
        },

        carregarDoLocalStorage() {
            const tarefasSalvas = localStorage.getItem('tarefas')
            if (tarefasSalvas) {
                this.tarefas = JSON.parse(tarefasSalvas)
            }
        }
    },

    mounted() {
        this.carregarDoLocalStorage()
    }
}
</script>

<style scoped>
/* ESTILOS BONITOS E SIMPLES */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
  font-family: 'Segoe UI', Arial, sans-serif;
  background: #f8f9fa;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
  font-size: 28px;
}

.formulario {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.formulario input {
  flex: 1;
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.formulario input:focus {
  outline: none;
  border-color: #42b983;
}

.formulario button {
  padding: 12px 24px;
  background: #42b983;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s;
}

.formulario button:hover {
  background: #359268;
}

.lista {
  list-style: none;
  padding: 0;
}

.lista li {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px;
  background: white;
  margin-bottom: 8px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  transition: transform 0.2s;
}

.lista li:hover {
  transform: translateX(4px);
}

.lista li input[type="checkbox"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.lista li span {
  flex: 1;
  font-size: 16px;
  color: #2c3e50;
}

.lista li span.concluida {
  text-decoration: line-through;
  color: #999;
}

.deletar {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.deletar:hover {
  opacity: 1;
}

.contador {
  text-align: center;
  margin-top: 20px;
  color: #7f8c8d;
  font-size: 14px;
  padding-top: 16px;
  border-top: 2px solid #ecf0f1;
}
</style>