# Formação CyberSec ⚔️ - Guia de Aula Prática #9: Seu Primeiro Dojo no Kali Linux

Olá, guerreiro(a) cibernético(a)!

Este documento é a nossa **Aula Prática #9**. Hoje, vamos configurar e explorar a ferramenta mais essencial no arsenal de um profissional de segurança: o **Kali Linux**. Este não é apenas um arquivo de instruções; é a sua aula. Siga cada passo, leia as explicações e, o mais importante, execute os comandos para sentir o poder na ponta dos seus dedos.

Nosso objetivo é duplo:
1.  **Dominar o Terminal (CLI):** Aprender a "lâmina", a forma mais pura, rápida e poderosa de interagir com sistemas.
2.  **Explorar o Arsenal (GUI):** Entender quando e como usar a interface gráfica para ferramentas visuais e descoberta.

Vamos começar.

---

## Parte 1: O Que é o Kali Linux e Por Que o Terminal é Rei?

Antes de mergulhar na prática, entenda sua ferramenta. O **Kali Linux** é uma distribuição de Linux (baseada em Debian) criada especificamente para tarefas de segurança e pentest. Pense nele como um canivete suíço com centenas de lâminas afiadas: ferramentas para mapear redes, encontrar vulnerabilidades, explorar falhas, analisar senhas e muito mais.

Nós começamos pelo **Terminal (CLI - Command-Line Interface)** porque no mundo real:

* **É o que você encontra:** Na maioria das vezes, o acesso a um sistema invadido é uma tela preta, não um desktop bonito.
* **É rápido e eficiente:** Sem o peso de uma interface gráfica, os comandos voam.
* **É o caminho da automação:** Scripts que automatizam ataques ou defesas rodam no terminal.

Agora que a filosofia está clara, vamos sujar as mãos.

## Parte 2: Laboratório Prático 1 - O Caminho da Lâmina (CLI)

Nesta seção, vamos construir nosso dojo de linha de comando e praticar os movimentos fundamentais.

### 2.1 Preparando e Acessando seu Dojo

Os arquivos `Dockerfile` e `docker-compose.yml` na pasta `kali-cli` são a planta do nosso dojo. Eles instruem o Docker a construir um ambiente Kali já com o nosso desafio "assado" dentro dele.

1.  **Navegue até a pasta do lab:**
    Abra seu terminal e certifique-se de que você está no diretório `kali-cli`.

2.  **Construa e inicie o ambiente:**
    ```bash
    # O comando --build é essencial na primeira vez. Ele lê nossa "planta" (Dockerfile) e constrói a imagem.
    docker compose up -d --build
    ```

3.  **Entre no seu Dojo:**
    Com o ambiente rodando, vamos entrar no terminal interativo do Kali.
    ```bash
    # Este comando nos dá um shell de bash dentro do container 'kali_cli_kensei'
    docker exec -it kali_cli_kensei /bin/bash
    ```
    Seu prompt de comando mudou para `root@...:/#`. Você está dentro. A partir de agora, todos os comandos são executados dentro do Kali.

### 2.2 Dominando os Comandos Essenciais

Vamos treinar. Execute cada comando e entenda o que ele faz.

1.  **Onde estou? (`pwd`)**
    O primeiro passo em qualquer ambiente é saber sua localização.
    ```bash
    ┌──(kali㉿)-[/] pwd
    ```
    A saída `/` indica que você está na raiz do sistema de arquivos.

2.  **O que há ao meu redor? (`ls`)**
    Liste os arquivos e diretórios. A opção `-la` nos dá uma visão detalhada.
    ```bash
    ┌──(kali㉿)-[/] ls -la
    ```
    Você verá a estrutura de pastas padrão do Linux (`/bin`, `/etc`, `/home`, `/root`, etc.). É como olhar o mapa do território.

3.  **Indo para casa (`cd`)**
    O diretório "home" do usuário `root` é `/root`. É o nosso espaço pessoal.
    ```bash
    ┌──(kali㉿)-[/] cd /root
    ┌──(kali㉿)-[/root] pwd
    ```
    A saída agora será `/root`.

4.  **Criando nosso espaço de treino (`mkdir`)**
    Todo Kensei precisa de uma área para praticar.
    ```bash
    ┌──(kali㉿)-[/root] mkdir espaco_de_treino
    ┌──(kali㉿)-[/root] ls
    ```
    Veja o novo diretório `espaco_de_treino` listado.

5.  **Entrando no espaço de treino (`cd`)**
    ```bash
    ┌──(kali㉿)-[/root] cd espaco_de_treino
    ```

6.  **Escrevendo nosso primeiro pergaminho (`echo`)**
    O comando `echo` escreve texto. O `>` redireciona esse texto para um arquivo.
    ```bash
    ┌──(kali㉿)-[/root/espaco_de_treino] echo "A disciplina é a ponte entre metas e realizações." > pergaminho.txt
    ```

7.  **Lendo o pergaminho (`cat`)**
    Use `cat` para exibir o conteúdo de um arquivo no terminal.
    ```bash
    ┌──(kali㉿)-[/root/espaco_de_treino] cat pergaminho.txt
    ```
    A sabedoria do Kensei aparecerá na sua tela.

### 2.3 Desafio: Encontrando o Pergaminho Secreto

Você treinou os movimentos básicos. Agora, um teste. Eu escondi uma "flag" (uma prova, um segredo) em algum lugar no seu ambiente.

* **Sua Missão:** Encontrar e ler o conteúdo do arquivo `.segredo_dojo`.
* **Pistas:**
    * Ele está em um diretório oculto (o nome do diretório começa com `.`).
    * Este diretório está na pasta `/root`.

Use `ls -la`, `cd` e `cat` para encontrar a flag. Poste-a em nosso grupo com a hashtag **#DesafioAula9**!

### 2.4 Limpando o Dojo

Ao final do treino, um bom guerreiro sempre limpa seu espaço.

1.  **Saindo do Kali:**
    ```bash
    ┌──(kali㉿)-[...] exit
    ```
2.  **Desligando o Ambiente:**
    No terminal da sua máquina (na pasta `kali-cli`), desligue o container.
    ```bash
    docker compose down
    ```

---

## Parte 3: Laboratório Prático 2 - O Arsenal Completo (GUI)

Você provou que pode lutar com a lâmina nua. Agora, vamos explorar o arsenal tecnológico que uma interface gráfica oferece.

### 3.1 Preparando e Acessando o Ambiente Gráfico

1.  **Navegue e Inicie:**
    Vá para a pasta `kali-gui` no seu terminal e execute:
    ```bash
    # Lembre-se, este build pode levar vários minutos!
    docker compose up -d --build
    ```

2.  **Conecte-se via VNC:**
    * Abra seu programa **VNC Viewer**.
    * Conecte-se ao endereço: `localhost:5901`
    * Senha: `kensei`

Bem-vindo ao desktop do Kali!

### 3.2 Explorando o Arsenal Visual

1.  **O Terminal Ainda Vive:** A primeira coisa a fazer é abrir o terminal dentro da GUI. Encontre o ícone e clique nele. Execute `ls /root` e `ls -la /root`. Viu? O desafio `.diretorio_secreto` também está aqui. Por baixo dos panos, é o mesmo sistema poderoso.

2.  **O Mapa do Tesouro (Menu de Aplicações):** O grande valor da GUI é a descoberta. Clique no menu de aplicações (o ícone do dragão do Kali). Navegue pelas categorias. Não precisa clicar nas ferramentas ainda, apenas veja o que existe:
    * Em `01 - Information Gathering`, você encontrará ferramentas como `nmap` e `maltego`.
    * Em `03 - Web Application Analysis`, você verá o `burp suite`.
    * Em `05 - Password Attacks`, você achará o `hashcat` e o `john the ripper`.

    Sua tarefa aqui é simplesmente **explorar**. Crie um mapa mental de onde as principais ferramentas estão localizadas. Isso economizará um tempo precioso no futuro.

### 3.3 Encerrando o Laboratório Gráfico

1.  Feche a janela do VNC Viewer.
2.  No seu terminal, na pasta `kali-gui`, desligue o ambiente:
    ```bash
    docker compose down
    ```

---

## Conclusão da Aula

Parabéns! Hoje você montou e explorou seu primeiro dojo de segurança. Você aprendeu a importância do terminal, praticou os comandos essenciais e entendeu o lugar da interface gráfica no seu cinto de ferramentas.

A prática constante leva à maestria. Repita esses laboratórios até que os comandos se tornem memória muscular.

Até a próxima aula, onde usaremos nosso dojo CLI para a primeira missão real: Reconhecimento com Nmap.

**Kaze, desligando.** 🥷