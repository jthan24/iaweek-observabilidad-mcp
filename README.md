# iaweek-observabilidad-mcp

## instalacion de la demo de opentelemetry

https://opentelemetry.io/docs/demo/docker-deployment/


### Diagrama de arquitectura
https://opentelemetry.io/docs/demo/architecture/


```bash
git clone https://github.com/open-telemetry/opentelemetry-demo.git
cd opentelemetry-demo/
docker compose up --force-recreate --remove-orphans --detach
```

### URLs para verificar el estado de la instalacion

Web store: http://localhost:8080/
Grafana: http://localhost:8080/grafana/
Load Generator UI: http://localhost:8080/loadgen/
Jaeger UI: http://localhost:8080/jaeger/ui/
Tracetest UI: http://localhost:11633/, only when using make run-tracetesting
Flagd configurator UI: http://localhost:8080/feature


## Instalacion del Grafana MCP

```bash
docker pull mcp/grafana
docker run --rm --network opentelemetry-demo --name grafana-mcp -i -e GRAFANA_URL=http://grafana:3000/ -e GRAFANA_SERVICE_ACCOUNT_TOKEN=EXAMPLE_TOKEN mcp/grafana
``` 



## Instalacion de GeminiCLI
```bash
docker pull node:22-alpine
docker run -it --name gemini --rm --network opentelemetry-demo --entrypoint sh node:22-alpine
apk install vim
npm install -g @google/gemini-cli
cd /tmp
mkdir .gemini
export GEMINI_API_KEY="EXAMPLE_TOKEN"
```

Contenido del archivo .gemini/settings.json 
```json
{
  "security": {
    "auth": {
      "selectedType": "gemini-api-key"
    }
  },
  "mcpServers": {
    "grafana": {
      "type": "sse",
      "url": "http://grafana-mcp:8000/sse"
    }
  }
}
```