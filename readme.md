# Iniciando Prometheus/SNMP Exporter/Node Exporter 
<hr>

Certifique-se de criar um arquivo prometheus.yml e um snmp.yml no mesmo diretório em que o docker-compose.yml está localizado.

>O conteúdo do prometheus.yml deve conter sua configuração personalizada para o Prometheus.
>O mesmo para o snmp.yml .

**Lembrar de verificar os arquivos presentes no diretório e alterar para configuração compatível com o seu sistema**
* docker-compose.yml
* prometheus.yml
* snmp.yml
* generator.yml

#### Instalando:

```
git clone https://github.com/AleDouglas/prometheus-snmp-docker.git
```

Utilizando arquivo docker-compose.yml, podemos iniciar os serviços usando o comando:
```
docker compose up -d
```

Isso iniciará os três serviços do Prometheus, Node Exporter e SNMP Exporter em contêineres separados e eles serão acessíveis nas portas especificadas.

>**Verifique** se as portas **9090, 9100 e 9116** estão disponíveis em seu sistema antes de executar o docker-compose up -d para evitar conflitos com outros serviços em execução.
>
>Espero que isso ajude a configurar os serviços do **Prometheus, Node Exporter e SNMP Exporter usando o Docker Compose**.

<hr>
#### Ligando SNMP Exporter ao IP do Switch

É **necessário** apontar o endereço de IP do Switch para o SNMP Exporter.
Acessamos o link:
```
localhost/9116
```
Após isso, basta em target colocar o IP do Switch e apenas submeter.
**Caso localhost não funcione, experimente configurar com o IP presente na máquina.**

<hr>
## Sobre o SNMP Exporter Generator

A biblioteca "generator" do projeto SNMP Exporter do Prometheus é uma ferramenta útil para gerar configurações YAML de forma automatizada para coleta de métricas SNMP de dispositivos.

O módulo "generator" permite que você crie automaticamente as configurações YAML necessárias para coletar métricas SNMP de dispositivos específicos, com base em um arquivo de entrada que define os OIDs (Object Identifiers) desejados.

A ideia principal é que você forneça um arquivo de entrada que contenha uma lista de OIDs que você deseja coletar do dispositivo SNMP. Em seguida, a ferramenta generator irá gerar a configuração YAML correspondente para o SNMP Exporter.

#### Como utilizar o SNMP Exporter Generator
1. **Instalando dependências**
```
sudo apt-get install unzip build-essential libsnmp-dev # Debian-based distros

sudo apt install golang-go # Necessário para compilar o arquivo generator
```

2. **Instalando SNMP_EXPORTER**

```
git clone https://github.com/prometheus/snmp_exporter.git
```
  
3. **Criando arquivo Generator e pasta Mibs**

É **necessário** o arquivo *"generator"* e a pasta contendo os *"mibs"* para poder gerar o arquivo *snmp.yml*.
```
cd snmp-exporter/ # Acessando a pasta snmp-exporter
cd generator/ # Acessando a pasta generator
make generator # Gerando o arquivo generator
make mibs # Gerando pasta contendo os mibs
export MIBDIRS=mibs # Definindo variável para gerador apontar para pasta Mibs
```
  
4. **Configurar arquivo Generator**

Dentro do repositório git possui um arquivo generator.yml que servirá de exemplo para o "generator", para isso basta apenas substituir o arquivo que a pasta *generator* possui pelo que está presente nesse diretório.

~~~html
Verificar arquivo: generator.yml
~~~

5. **Gerando arquivo snmp.yml**

O módulo "generator" permite que você crie automaticamente as configurações YAML necessárias para coletar métricas SNMP de dispositivos específicos

```
./generator generate
```
  
6. **Copiar arquivo snmp.yml para pasta de configuração**

Devemos mover o arquivo snmp gerado para a pasta onde o docker-compose está localizado, substituíndo o arquivo snmp.yml antigo pelo novo gerado.

```
cp snmp.yml [path do docker-compose]
```

7. **Reiniciando o SNMP Exporter**

```
docker compose restart
```

8. **Configurando Target do SNMP Exporter**


Após todas essas etapas, precisamos entrar dizer ao SNMP Exporter qual IP estaremos utilizando.

```
localhost/9116
```
**Caso localhost não funcione, experimente configurar com o IP presente na máquina.**

Em Target colocamos o endereço do Swift e apenas submetemos , após isso todos os processos estarão prontos.