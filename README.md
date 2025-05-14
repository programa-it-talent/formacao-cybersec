
# 🛡️ Formação CyberSec – Kensei x Vai na Web

Bem-vindo ao repositório oficial do curso **Formação CyberSec**!  
Aqui você encontrará os ambientes práticos, materiais de apoio, desafios e projetos usados ao longo dos três módulos da formação.

---

## 🧭 Estrutura dos Módulos

### 📘 Módulo 1 – Fundamentos de Cibersegurança
- Conceitos básicos de segurança
- Modelo CIA (Confidencialidade, Integridade, Disponibilidade)
- Ameaças, ataques e vulnerabilidades
- Introdução à infraestrutura de rede
- Ferramentas: Kali Linux, Nmap, Wireshark

### 🛡️ Módulo 2 – Defesa e Monitoramento
- Segurança em sistemas e aplicações
- Proteção de infraestrutura
- Ferramentas de monitoramento
- Introdução à segurança em nuvem (cloud security)
- Simulações de incidentes reais

### 🕵️ Módulo 3 – Ethical Hacking e Projetos Reais
- Metodologia de Pentest
- Exploração controlada de vulnerabilidades
- Projeto final em ambiente simulado realista

---

## 🚀 Como começar

1. **Clone o repositório**
```bash
git clone https://github.com/Kensei-CyberSec-Lab/formacao-cybersec.git
cd formacao-cybersec/modulo1-fundamentos
```

2. **Suba o laboratório com Docker**
```bash
docker-compose up -d
```

3. **Acesse os containers**
```bash
docker exec -it ubuntu_lab bash
docker exec -it kali_lab bash
```

4. **Acesse o DVWA no navegador**
```
http://localhost:8080
```

---

## 🥷 Missões e desafios

- Missão Ninja #1: Subir os containers e criar um arquivo `flag.txt` com seu nome dentro do Ubuntu.
- Missão Ninja #2: Usar o Kali para identificar o IP do DVWA via nmap.
- Missão Ninja #3: Explorar uma vulnerabilidade no DVWA em modo "low".

---

## ❓ Suporte

Em caso de dúvidas, entre no grupo oficial ou abra uma *issue* aqui no GitHub.

---

## 📜 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).

---

Cibersegurança é resistência. Bora hackear o futuro com ética e inteligência!
