<template>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@300..700&family=Noto+Sans:ital,wght@0,100..900;1,100..900&family=Noto+Serif:ital,wght@0,100..900;1,100..900&family=Playfair+Display:ital,wght@0,400..900;1,400..900&family=Raleway:ital,wght@0,100..900;1,100..900&family=Source+Code+Pro:ital,wght@0,200..900;1,200..900&display=swap" rel="stylesheet">
    <div class="container">
      <div class="menu">
        <div class="buttons-flex">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
        <div class="title">
          <a href="https://github.com/mairaarquino" target="_blank"><h1><i class='fab fa-github'></i>github.com/mairaarquino</h1></a>
        </div>
      </div>
      <div id="app">
        <div v-for="(line, index) in lines" :key="index">
          <p v-if="line.type === 'text'">{{ line.content }}</p>
          <div v-else-if="line.type === 'code'" class="code" v-html="line.content"></div>
          <div v-else-if="line.type === 'section'" class="type2">
            <i class="fas fa-angle-right icone" :class="{ 'error': line.error, 'sucess': !line.error }"></i>
            <h2 :class="{ 'error': line.error, 'sucess': !line.error }">{{ line.content }}</h2>
          </div>
        </div>
        <div class="input-wrapper">
          <label for="terminal-input">~user in mairaarquino/github/terminal <span>$</span></label>
          <div class="type">
            <i class="fas fa-angle-right icone"></i>
            <input id="terminal-input" ref="input" @keypress="handleKeyPress" @click="handleClick" />
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        lines: [],
        delay: ms => new Promise(res => setTimeout(res, ms))
      };
    },
    mounted() {
      this.openTerminal();
    },
    methods: {
      async openTerminal() {
        await this.delay(500);
        this.createText("Bem vindo!");
        await this.delay(700);
        this.createText("Iniciando o servidor...");
        await this.delay(1500);
        this.createText("Você pode usar varios comandos, use 'help' para ver todos os comandos.");
        await this.delay(500);
        this.newLine();
      },
      async newLine() {
        await this.$nextTick();
        this.$refs.input.focus();
      },
      async getInputValue() {
        const value = this.$refs.input.value;
        if (value === "help") {
          this.trueValue(value);
          this.createCode(`<p style="color:rgb(254, 193, 52)">projects:</p>`, "Meu github com alguns dos meus projetos. Me segue lá ;)");
          this.createCode(`<p style="color:rgb(254, 193, 52)">about:</p>`, "Oque eu faço e quem sou eu?");
          this.createCode(`<p style="color:rgb(254, 193, 52)">skills:</p>`, "Minhas habilidades e competencias tecnicas.");
          this.createCode(`<p style="color:rgb(254, 193, 52)">social:</p>`, "Minha redes sociais");
          this.createCode(`<p style="color:rgb(254, 193, 52)">passions:</p>`, "Meus hobbies e paixões.");
          this.createCode(`<p style="color:rgb(254, 193, 52)">clean/clear:</p>`, "Limpar o terminal.");
        } else if (value === "projects") {
          this.trueValue(value);
          this.createCode(`<a href='https://github.com/mairaarquino' target='_blank'>https://github.com/mairaarquino</a>`);
        } else if (value === "about") {
          this.trueValue(value);
          this.createText("Oi meu nome é Maíra Arquino :)");
          this.createText('Sou Desenvolvedora Back-end Sênior & Tech Lead, Com seis anos de experiência no mundo do desenvolvimento, posso dizer que já vi de tudo um pouco e aprendi muito pelo caminho. Meu forte são Node.js e PHP, adoro desafios técnicos e resolver problemas complicados, cada projeto é uma oportunidade emocionante de aprender algo novo. Como Tech Lead, estou sempre disposta a compartilhar meu conhecimento e ajudar meus colegas a crescerem profissionalmente.');
        } else if (value === "skills") {
          this.trueValue(value);
          this.createText("Minhas hablidades:");
          this.createText('Node.js, Javascript, Typescript, PHP, NestJs, Banco de dados, Microsserviços, Serverless, Cloud, Design Patterns, Clean Architecture, Git.');
        } else if (value === "passions") {
          this.trueValue(value);
          this.createText("Meus hobbies e paixões:");
          this.createText('Livros, Gatos, Cães, Café, Música, Praia e Viagens.');
        }
         else if (value === "social") {
          this.trueValue(value);
          this.createCode(`<a href='https://github.com/mairaarquino' target='_blank'>https://github.com/mairaarquino</a>`);
          this.createCode(`<a href='https://www.linkedin.com/in/mairaarquino/' target='_blank'>https://www.linkedin.com/in/mairaarquino/</a>`);
        } else if (value === "clear" || "clean") {
          this.lines = [];
        } else {
          this.falseValue(value);
          this.createText(`Comando não encontrado: ${value}, use 'help' para ver todos os comandos.`);
        }
        this.$refs.input.value = '';
      },
      createText(text) {
        this.lines.push({ type: "text", content: text });
      },
      createCode(code, text) {
        if (text == undefined) {
            text = code
        this.lines.push({ type: "code", content: `${code}` });
        } else {
            this.lines.push({ type: "code", content: `${code} <span class='text'> ${text} </span>` });
        }
      },
      trueValue(value) {
        this.lines.push({ type: "section", content: value, error: false });
      },
      falseValue(value) {
        this.lines.push({ type: "section", content: value, error: true });
      },
      handleKeyPress(event) {
        if (event.key === "Enter") {
          event.preventDefault();
          this.getInputValue();
          this.newLine();
        }
      },
      handleClick() {
        this.$refs.input.focus();
      }
    }
  };
  </script>
  
  <style scoped>
  *{
      margin:0;
      padding:0;
      box-sizing: border-box;
      font-family: "Fira Code", monospace;
      font-size: 18px !important;
        font-optical-sizing: auto;
        font-style: normal;
  }
  ::-webkit-scrollbar-track
  {
      box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
      -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
      
      background-color: #555;
  }
  
  ::-webkit-scrollbar
  {
      width: 12px;
      background-color: #222020;
  }
  
  ::-webkit-scrollbar-thumb
  {
    border-radius: 10px;
    box-shadow: inset 0 0 6px rgba(0,0,0,.3);
      -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
      background-color: rgb(129, 129, 129);
  }
  .white{
    color:rgb(238, 238, 238);
  }
  .blue{
    color:#9CEAF3;
  }
  a{
    text-decoration: none;
    color: rgb(238, 238, 238);
  }
  body{
    background-color:#181819;
  
    height: 100vh;
    font-family: 'Fira Code', monospace;
    display:flex;
    align-items: center;
    justify-content: center;
    }
    .input-wrapper {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    }

    .input-wrapper label {
    margin-right: 5px;
    color: #76ec03;
    }
    .input-wrapper label span {
        color: #6090e6;
    }
  .container{
    display:flex;
    flex-direction: column;
    width:100vw;
    max-width:80%;
    height:600px;
    overflow: hidden;
    border-radius: 6px;
    border: 1px solid #181819;
    margin: 0 auto;
    cursor: text;
  }
  .code{
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .menu{
    display:flex;
    align-items: center;
  
    text-align: center;
    flex-direction: row;
    width: 100%;
    height: 24px;
    background-color: #424040;
    padding: 0 8px;
    cursor:default;
  }
  .menu .button{
    width:12px;
    height:12px;
    border-radius:50%;
    margin-right:8px;
    cursor:pointer;
  }
  .red{background-color:#F5544D;}
  .yellow{background-color:#FABD2F;}
  .green{background-color:#47D043;}
  div.title{
    flex:1;
    text-align:center;
  }
  .menu h1{
    font-size:13px;
    color:#40a93c;
  
  }
  .buttons-flex{
    position: absolute;
    display:flex;
    flex-direction: row;
  }
  @media (max-width:500px){
    .buttons-flex{
      position: unset;
      display:flex;
      flex-direction: row;
    }
  }
  @media (max-width:330px){
    .menu h1{
      display:none;
    }
  }
  #app{
    display:flex;
    flex-direction: column;
    background-color:#202127;
    width:100%;
    height:100%;
    padding:8px;
    overflow: auto;
    text-align: left;
  }
  p{
    font-size:16px;
    color:rgb(238, 238, 238);
    padding:8px 0;
  }
  h2{
    font-size:16px;
    color:rgb(238, 238, 238);
  }
  p.code{
    color:#66C2CD;
    margin-left:8px;
  }
  span.text{
    margin-left:16px;
    color:white;
  }
  p.path{
    color:#F7FCA0;
  }
  p.path span{
    color:#6A77D2;
  }
  p.path span + span{
    color:#9CEAF3;
  }
  
  .sucess{
    color:#5AD786;
  }
  .error{
    color:#F78F85;
  }
  p.response{
    color:rgb(238, 238, 238);
  }
  
  input{
    border:none;
    background-color: unset;
    color:rgb(69, 223, 100);
    width:100%;
    font-size:16px;
    font-family: 'Fira Code', monospace;
  }
  input:focus{
    border:none;
    outline: none;
    box-shadow: none;
    background-color: unset;
    
  }
  .type{
    display:flex;
    align-items: center;
    padding:8px 0;
  }
  .type2{
    display:flex;
    align-items: center;
    padding:8px 0;
  }
  .icone{
    color:#5AD786;
    padding-right:8px;
  }
  
  .icone.error{
    color:#F78F85;
  }
  
  </style>
  