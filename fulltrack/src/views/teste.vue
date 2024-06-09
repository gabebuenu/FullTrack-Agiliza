<template>
    <div class="text">
      <h1>Motoristas</h1>
    </div>
    <div id="app" class="container">
      <div class="card">
        <div class="img-perfil">
          <img :src="imgPerfil" class="img-perfil" alt="Imagem de perfil">
        </div>
        <h3>Adicione um Motorista</h3>
        <div class="butaozinho">
          <router-link to="/cadastro" class="butaozinho">Clique Aqui</router-link>
        </div>
      </div>
      <div class="card-list-container">
        <div class="card-list">
          <CaminhoneiroCard
            v-for="caminhoneiro in caminhoneiros"
            :key="caminhoneiro.nomeCaminhoneiro"
            :caminhoneiro="caminhoneiro"
            :imgPerfil="imgPerfil"
            @delete-caminhoneiro="handleDeleteCaminhoneiro"
          />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import CaminhoneiroCard from '@/components/CaminhoneiroCard.vue';
  
  export default {
    components: {
      CaminhoneiroCard
    },
    data() {
      return {
        caminhoneiros: [],
        imgPerfil: 'https://raw.githubusercontent.com/gabebuenu/guardarimagens/6375c7327f6ba3b2ace0ebe97edab425b6e856a7/user3.svg'  // Substitua pelo caminho correto
      };
    },
    methods: {
      async fetchCaminhoneiros() {
        const response = await fetch('/api/join/caminhoneiros-com-caminhoes');
        this.caminhoneiros = await response.json();
      },
      async handleDeleteCaminhoneiro(caminhoneiro) {
        const response = await fetch(`/api/join/delete-caminhoneiro/${caminhoneiro.id}`, {
          method: 'DELETE'
        });
        if (response.ok) {
          this.caminhoneiros = this.caminhoneiros.filter(c => c !== caminhoneiro);
        } else {
          console.error('Falha ao deletar caminhoneiro');
        }
      }
    },
    created() {
      this.fetchCaminhoneiros();
    }
  };
  </script>
  
  <style>
  .text {
    text-align: center;
  }
  .text h1{
    color: #2c3e50;
  }
  
  .card {
    flex: 0 0 auto;
    border: 1px solid #ccc;
    width: 20vw;
    padding: 5vw;
    margin: 8px;
    border-radius: 8px;
    box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    
  }
  
  .card h3 {
    font-size: 100%;
  }
  
  .butaozinho {
    display: flex;
    justify-content: center;
    margin-top: 10px;
  }
  
  .butaozinho a {
    background-color: red;
    border-radius: 16px;
    color: #fff;
    text-decoration: none;
    padding: 10px 20px;
  }
  
  .card-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .card .img-perfil{
    width: 100%;
  }
  
  .carde {
    background-color: #fff;
    flex: 0 0 auto;
    border: 1px solid #ccc;
    width: 20vw;
    padding: 5vw;
    margin: 8px;
    border-radius: 8px;
    box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
  }
  @media (min-width: 768px) {
    .container {
      flex-direction: row;
      justify-content: space-around;
      align-items: flex-start;
    }
  }
  </style>