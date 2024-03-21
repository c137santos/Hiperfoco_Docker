# Sua imagem é baseada em que? Baseada em debian
FROM debian

# Metadados
LABEL app="Santos"

# Variável de ambiente
ENV CLARA="TDAH"

# Executar comandos em momentos de build
RUN apt-get update && apt-get install -y stress && apt-get clean

# Executar uma processo
CMD stress --cpu 1 --vm 1 --vm-bytes 64M