# Lab da Aula 7: Wireshark Hands-on (Análise de Tráfego)

Bem-vind@ ao lab prático de Wireshark! Nesta aula, você vai aprender a capturar e analisar o tráfego de rede usando o Wireshark, uma das ferramentas mais importantes para qualquer profissional de cibersegurança.

Vamos usar um servidor web simples rodando em Docker e o Wireshark instalado diretamente na sua máquina (host) para observar as comunicações.

---

## 🎯 Objetivos do Lab

* **Instalar** e configurar o Wireshark em seu sistema operacional.
* **Iniciar** um servidor web de teste em um contêiner Docker.
* **Capturar** o tráfego de rede gerado ao interagir com o servidor web.
* **Identificar** o **Handshake TCP (Three-Way Handshake)** e entender seu papel.
* **Analisar** o **Fluxo HTTP (GET e POST)** em pacotes.
* **Aplicar filtros** no Wireshark para encontrar informações específicas.
* **Analisar** o conteúdo dos pacotes para entender a comunicação HTTP.

---

## 🛠️ Pré-requisitos

Antes de começar, certifique-se de ter o seguinte configurado em sua máquina:

1.  **Docker Desktop:** Instalado e em execução.
    * Guia de instalação (se precisar): [https://docs.docker.com/desktop/install/](https://docs.docker.com/desktop/install/)
2.  **Git:** Instalado.
    * Para clonar o repositório do curso.

---

### **Como Obter os Arquivos do Lab:**

* **Se você é um aluno NOVO e ainda NÃO CLONOU o repositório do curso:**
    1.  Abra seu terminal.
    2.  Execute o comando para clonar o projeto:
        ```bash
        git clone https://github.com/Kensei-CyberSec-Lab/formacao-cybersec.git
        ```

* **Se você JÁ É ALUNO e CLONOU o repositório em uma aula anterior:**
    1.  Abra seu terminal.
    2.  Navegue até a pasta raiz do seu repositório `formacao-cybersec`:
        ```bash
        cd formacao-cybersec
        ```
        (Ou para o diretório onde você clonou o projeto, se for diferente).
    3.  Puxe as últimas atualizações do repositório remoto para obter os arquivos mais recentes (incluindo as correções e novas instruções para esta aula):
        ```bash
        git pull
        ```
    Certifique-se de que o `git pull` seja concluído sem erros.

---

### **Instalando o Wireshark em seu Sistema Operacional (Host):**

* **Windows:**
    1.  Acesse o site oficial do Wireshark: [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
    2.  Baixe o instalador para Windows (geralmente `Windows Installer (64-bit)`).
    3.  Execute o instalador e siga as instruções. **É CRÍTICO que você selecione e instale o `Npcap` junto**, pois ele é o driver que permite ao Wireshark capturar pacotes na rede.
    4.  Conclua a instalação.

* **macOS:**
    1.  Acesse o site oficial do Wireshark: [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
    2.  Baixe o instalador para macOS (escolha a versão `Arm 64-bit` para Macs com chip M1/M2/M3 ou `Intel 64-bit` para Macs mais antigos).
    3.  Abra o arquivo `.dmg` e arraste o ícone do Wireshark para a pasta `Applications`.
    4.  Na primeira vez que você abrir o Wireshark, ele pode solicitar a instalação de **ferramentas de auxílio de captura** (como `ChmodBPF`). **Siga as instruções para instalá-las**, pois são essenciais para que o Wireshark tenha permissão para capturar pacotes.

* **Linux (Ubuntu/Debian e WSL2 no Windows):**
    1.  Abra um terminal.
    2.  Atualize os pacotes e instale o Wireshark:
        ```bash
        sudo apt update
        sudo apt install wireshark
        ```
    3.  Durante a instalação, você será perguntado se usuários não-superusuário devem ter permissão para capturar pacotes. **Responda "Yes"**.
    4.  Para poder usar o Wireshark sem `sudo`, adicione seu usuário ao grupo `wireshark`:
        ```bash
        sudo usermod -a -G wireshark $USER
        ```
    5.  **IMPORTANTE:** Para que as alterações de grupo tenham efeito, você precisará **fazer logout e login novamente no sistema operacional (ou reiniciar)**.

---

## 🚀 Passo a Passo do Lab

Siga os passos abaixo para configurar e executar o lab. **Certifique-se de que o Wireshark esteja instalado e funcionando corretamente antes de iniciar!**

### Parte 1: Preparar o Servidor Web Docker

1.  **Navegue até a pasta da aula 7** no seu terminal:
    ```bash
    cd formacao-cybersec/modulo1-fundamentos/aula_7
    ```

2.  **Inicie o servidor web de teste em Docker:**
    ```bash
    docker compose up -d web-server-aula7
    ```
    * Este comando irá construir (se necessário) e iniciar apenas o serviço `web-server-aula7`.
    * Aguarde alguns segundos para que o contêiner suba completamente.

3.  **Verifique se o `web-server-aula7` está rodando:**
    ```bash
    docker ps
    ```
    Você deve ver `web-server-aula7` listado com `Status` como `Up ... seconds` e a porta `0.0.0.0:8080->8080/tcp` mapeada.

### Parte 2: Capturar Tráfego com Wireshark

1.  **Abra o aplicativo Wireshark** na sua máquina (Host).
    * No Windows/macOS, procure por "Wireshark" no menu Iniciar/Launchpad.
    * No Linux, digite `wireshark` no terminal.

2.  **Selecione a interface de rede para captura:**
    * Na tela inicial do Wireshark, você verá uma lista de interfaces de rede disponíveis (ex: `Wi-Fi`, `Ethernet`, `lo` ou `Loopback`, `Docker Desktop`).
    * Identifique a interface que você está usando para se conectar à internet. Ela geralmente mostrará um gráfico de atividade (ondas) ou um alto número de pacotes sendo transferidos.
    * **Clique duas vezes na interface escolhida** para iniciar a captura de pacotes.

3.  **Gere tráfego para capturar (no seu navegador):**
    * Abra seu navegador de internet (Chrome, Firefox, Edge, etc.) no **seu host (sua própria máquina)**.
    * Acesse o endereço do servidor web que está rodando no Docker e navegue pelas páginas:
        * Página Inicial (GET): `http://localhost:8080/`
        * Página Secreta (GET): `http://localhost:8080/secret`
        * Simulação de Login (GET para formulário, POST para envio): `http://localhost:8080/login_form`
            * Na página de login, clique no botão "Login" para enviar um POST.

4.  **Pare a captura no Wireshark:**
    * Volte para a janela do Wireshark.
    * Clique no botão **Stop** (o quadrado vermelho na barra de ferramentas superior) para parar a captura.

### Parte 3: Analisar Pacotes no Wireshark

Agora que você tem pacotes capturados, é hora de analisá-los!

1.  **Visão Geral da Interface do Wireshark:**
    * **Painel de Lista de Pacotes (superior):** Lista todos os pacotes capturados, com informações resumidas.
    * **Painel de Detalhes do Pacote (meio):** Exibe a estrutura hierárquica do pacote selecionado (camadas Ethernet, IP, TCP, HTTP, etc.).
    * **Painel de Bytes do Pacote (inferior):** Mostra o conteúdo bruto do pacote em formato hexadecimal e ASCII.

2.  **Aplique Filtros de Exibição:**
    Use a barra de filtro na parte superior do Wireshark (logo abaixo da barra de ferramentas) para refinar sua busca. Lembre-se de que a porta do nosso servidor é `8080`.

    * Para ver apenas tráfego TCP (incluindo o Handshake):
        ```
        tcp and tcp.port == 8080
        ```
    * Para ver apenas tráfego HTTP (GET e POST):
        ```
        http and tcp.port == 8080
        ```
    * Para ver apenas requisições HTTP POST:
        ```
        http.request.method == "POST" and tcp.port == 8080
        ```
    * Após digitar um filtro, pressione `Enter` ou clique na seta para a direita para aplicá-lo.

3.  **Exercícios Práticos de Análise:**

    * ### **Identificando o Handshake TCP (Three-Way Handshake):**
        1.  Aplique o filtro: `tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.port == 8080`
            * Isso deve mostrar o primeiro pacote do handshake (SYN).
        2.  Clique neste pacote SYN.
        3.  No Painel de Detalhes, expanda "Transmission Control Protocol" e observe que o flag `[SYN]` está marcado.
        4.  Limpe o filtro. Procure na sequência de pacotes (logo após o SYN) pelo pacote de resposta do servidor:
            * Ele terá os flags `[SYN, ACK]` marcados.
            * O próximo pacote será do seu navegador para o servidor com apenas o flag `[ACK]` marcado.
        5.  **Dica:** Você pode clicar com o botão direito no primeiro pacote SYN e selecionar `Follow` -> `TCP Stream` para ver todo o handshake e a conversa HTTP subsequente em uma única janela.

    * ### **Analisando Requisições HTTP GET:**
        1.  Aplique o filtro: `http.request.method == "GET" and tcp.port == 8080`
        2.  Encontre os pacotes para `/` e `/secret`.
        3.  Clique em um deles. No Painel de Detalhes, expanda **"Hypertext Transfer Protocol"**.
        4.  Observe os cabeçalhos da requisição, como `Host`, `User-Agent` (seu navegador) e `Accept-Language`.

    * ### **Analisando Requisições HTTP POST (Formulário de Login):**
        1.  Aplique o filtro: `http.request.method == "POST" and tcp.port == 8080`
        2.  Clique no pacote que contém a requisição POST para `/do_login`.
        3.  No Painel de Detalhes do Pacote, expanda **"HTML Form URL Encoded"**.
        4.  **Observe o `username` e o `password` enviados em texto claro!**
        5.  **Importante:** Este exemplo demonstra por que **NUNCA devemos usar HTTP para enviar informações sensíveis como credenciais em ambientes reais.** Em produção, sempre usamos **HTTPS** (HTTP Seguro), que criptografa o tráfego e impede que atacantes vejam os dados em texto claro no Wireshark. O Wireshark só conseguiria ver o tráfego HTTPS criptografado aqui.

    * ### **Visualizando as Respostas HTTP:**
        1.  Para cada requisição (GET ou POST), localize a resposta correspondente do servidor (geralmente logo abaixo na lista de pacotes, com status `200 OK`).
        2.  Clique na resposta. No Painel de Detalhes, expanda a seção **"Line-based text data"** ou **"Uncompressed entity body"** para ver o conteúdo retornado pelo servidor (HTML da página ou o JSON de sucesso/falha do login).

---

## 🧹 Limpeza do Lab

Ao finalizar os exercícios, é uma boa prática derrubar o contêiner Docker para liberar recursos do seu sistema:

1.  Retorne ao seu terminal.
2.  Certifique-se de estar na pasta `formacao-cybersec/modulo1-fundamentos/aula_7`.
3.  Execute o comando:
    ```bash
    docker compose down
    ```
    Isso irá parar e remover o contêiner `web-server-aula7`.

---

## ❓ Dúvidas?

Não se preocupe se encontrar dificuldades! A análise de tráfego pode ser complexa no início.
* Revise os passos.
* Pergunte no grupo da turma ou ao professor. Ninguém fica para trás!

**Vamos juntos desvendar os segredos da rede!**

---