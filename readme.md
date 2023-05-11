

#### Iniciando Prometheus/SNMP Exporter/Node Exporter ####

Certifique-se de criar um arquivo prometheus.yml e um snmp.yml no mesmo diretório em que o docker-compose.yml está localizado.
O conteúdo do prometheus.yml deve conter sua configuração personalizada para o Prometheus.
O mesmo para o snmp.yml .

Após criar o arquivo docker-compose.yml, você pode iniciar os serviços

[ TODOS OS ARQUIVOS ESTÃO PRESENTES NESSE DIRETÓRIO ]

usando o comando:

docker-compose up -d

Isso iniciará os três serviços do Prometheus, Node Exporter e SNMP Exporter em contêineres separados 
e eles serão acessíveis nas portas especificadas.

Verifique se as portas 9090, 9100 e 9116 estão disponíveis em seu sistema antes de executar 
o docker-compose up -d para evitar conflitos com outros serviços em execução.

Espero que isso ajude a configurar os serviços do Prometheus, 
Node Exporter e SNMP Exporter usando o Docker Compose.

###########################################################

#### Sobre o SNMP Exporter Generator ####

A biblioteca "generator" do projeto SNMP Exporter do Prometheus é uma ferramenta útil para gerar configurações YAML 
de forma automatizada para coleta de métricas SNMP de dispositivos.

O módulo "generator" permite que você crie automaticamente as configurações YAML necessárias para coletar 
métricas SNMP de dispositivos específicos, com base em um arquivo de entrada que define os OIDs (Object Identifiers) desejados.

A ideia principal é que você forneça um arquivo de entrada que contenha uma lista de OIDs 
que você deseja coletar do dispositivo SNMP. Em seguida, a ferramenta generator irá 
gerar a configuração YAML correspondente para o SNMP Exporter.


** Como utilizar o SNMP Exporter Generator **


# 1. Instalando dependências #
sudo apt-get install unzip build-essential libsnmp-dev # Debian-based distros
sudo apt install golang-go # Necessário para compilar o arquivo generator

# 2. Instalando SNMP_EXPORTER #
git clone https://github.com/prometheus/snmp_exporter.git

# 3. Generator #
cd generator/
make generator
sudo make mibs # Opcional

# 4. Configurar arquivo Generator #

Arquivo de exemplo -> generate-exemplo.yml

# 5. Criando arquivo snmp.yml #
./generator generate

# 6. Copiar arquivo snmp.yml para pasta de configuração #
sudo cp snmp.yml [path do docker]

# 7. Reiniciando serviços #
sudo docker compose restart





