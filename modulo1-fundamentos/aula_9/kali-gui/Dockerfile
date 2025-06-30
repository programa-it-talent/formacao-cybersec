# Dockerfile atualizado para o lab GUI
FROM kalilinux/kali-rolling

ARG VNC_PASSWORD=kensei
ENV VNC_PASSWORD=${VNC_PASSWORD}
ENV USER=root

RUN apt-get update && \
    apt-get install -y kali-desktop-xfce tightvncserver && \
    apt-get clean

# <<< ADICIONANDO O DESAFIO DIRETAMENTE NA IMAGEM >>>
RUN mkdir -p /root/.diretorio_secreto && \
    echo "KenseiCyberSec{D3s4f10_CLI_C0nclu1d0!}" > /root/.diretorio_secreto/.segredo_dojo

COPY xstartup /root/.vnc/xstartup
RUN chmod +x /root/.vnc/xstartup

COPY startup.sh /startup.sh
RUN chmod +x /startup.sh

EXPOSE 5901
CMD ["/startup.sh"]