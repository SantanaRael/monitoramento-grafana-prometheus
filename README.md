# Monitoramento com Grafana, Prometheus e Node Exporter

Este projeto configura uma infraestrutura de monitoramento usando **Prometheus**, **Grafana** e **Node Exporter** em containers Docker para monitorar um servidor.

## Tecnologias Utilizadas

- **Docker**
- **Prometheus**
- **Grafana**
- **Node Exporter**
- **Docker Compose**

## Para que serve cada aplicação

- **Node Exporter**: Um **exportador de métricas** de hardware e do sistema operacional. Ele coleta informações como uso de CPU, memória, disco e rede de um servidor e expõe essas métricas em um endpoint HTTP para que o Prometheus possa coletá-las. Essencial para monitorar os recursos do sistema em tempo real.

- **Prometheus**: Um **sistema de monitoramento e alerta**. Ele coleta, armazena e consulta métricas de sistemas e aplicações a partir de diferentes alvos (como o Node Exporter). Prometheus também permite definir alertas com base em condições específicas e exibir gráficos dessas métricas.

- **Grafana**: Uma **plataforma de visualização de dados** que se conecta a diversas fontes de dados, incluindo o Prometheus. O Grafana permite criar dashboards personalizados para monitorar métricas e gerar insights visuais de forma interativa.

## Arquivos Importantes

- **`docker-compose.yml`**: Define os serviços do **Prometheus**, **Grafana** e o servidor monitorado.
- **`prometheus.yml`**: Arquivo de configuração do Prometheus para definir os alvos e o scraping das métricas.

## Pré-requisitos

Certifique-se de ter o Docker e Docker Compose instalados no seu ambiente. Para isso, siga os passos abaixo:

- **Docker**: [Instalar Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Instalar Docker Compose](https://docs.docker.com/compose/install/)

## Como Executar

1. **Clonar o Repositório**:

   ```bash
   git clone https://github.com/SantanaRael/monitoramento-grafana-prometheus
   cd monitoramento-grafana-prometheus
   ```

2. **Configurar o Ambiente**:
   
   Certifique-se de que o arquivo `prometheus.yml` está configurado corretamente para monitorar o Node Exporter. Um exemplo básico está incluído no repositório.

3. **Subir os Containers**:

   Use o seguinte comando para iniciar os serviços:

   ```bash
   docker-compose up -d
   ```

4. **Acessar o Grafana**:

   O Grafana estará disponível na porta **3000**. Abra o navegador e acesse:

   ```
   http://localhost:3000
   ```

   - O usuário e senha padrão são `admin/admin` (você será solicitado a alterar a senha ao fazer o login pela primeira vez).

5. **Adicionar o Prometheus como Data Source no Grafana**:

   - No Grafana, vá para **Configuration > Data Sources**.
   - Adicione o **Prometheus** como data source com o seguinte URL:
     ```
     http://prometheus:9090
     ```

6. **Criar Dashboards no Grafana**:

   Agora você pode criar dashboards customizados no Grafana utilizando as métricas coletadas pelo **Node Exporter**.

## Métricas Principais

Algumas métricas que você pode utilizar nos dashboards do Grafana:

- **`up`**: Indica se o serviço monitorado está disponível (1 = UP, 0 = DOWN).
- **`node_cpu_seconds_total`**: Utilização da CPU dividida por estados (usuário, sistema, idle, etc.).
- **`node_memory_MemAvailable_bytes`**: Memória disponível no sistema.
- **`node_filesystem_avail_bytes`**: Espaço disponível em disco.
- **`node_network_receive_bytes_total`** e **`node_network_transmit_bytes_total`**: Dados recebidos e enviados pela interface de rede.

## Configuração do Node Exporter

No serviço `monitored-server`, o **Node Exporter** é executado para expor métricas de hardware do sistema. Aqui está a configuração do volume para permitir que o **Node Exporter** acesse o sistema host:

```yaml
volumes:
  - /proc:/host/proc:ro
```

Certifique-se de que o Prometheus está configurado para acessar o `monitored-server` na porta **9100** no arquivo `prometheus.yml`.

