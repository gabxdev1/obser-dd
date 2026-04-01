flowchart LR
subgraph APP["Aplicação Java"]
direction TB
CODE["Código da aplicação<br/>(Spring Boot / Java)"]
JVM["JVM"]
DDJ["dd-java-agent.jar<br/>(auto-instrumentação)"]

    CODE --> JVM
    DDJ -. "intercepta execução" .-> JVM
end

subgraph LOCAL["Coleta local"]
direction TB
DDA["Datadog Agent"]
end

subgraph PROXY["Saída corporativa"]
direction TB
PXY["Proxy centralizada"]
end

subgraph DD["Destino final"]
direction TB
DDOG["Datadog Platform"]
end

JVM -- "requisições, chamadas HTTP,\nqueries SQL, filas, erros" --> DDJ
DDJ -- "traces / spans" --> DDA
DDJ -- "APM / contexto de execução" --> DDA
DDA -- "telemetria" --> PXY
PXY -- "telemetria" --> DDOG

NOTE["O dd-java-agent.jar não envia direto para a Datadog Platform.<br/>Ele instrumenta a JVM e entrega os dados ao Datadog Agent local."]
NOTE -.-> DDJ
NOTE -.-> DDA

classDef app fill:#E8F4FF,stroke:#1D4ED8,color:#0B1F44,stroke-width:1.5px;
classDef agent fill:#ECFDF3,stroke:#047857,color:#052E16,stroke-width:1.5px;
classDef proxy fill:#FFF7ED,stroke:#C2410C,color:#431407,stroke-width:1.8px;
classDef dest fill:#F3F4F6,stroke:#374151,color:#111827,stroke-width:1.5px;
classDef note fill:#FEF3C7,stroke:#B45309,color:#78350F,stroke-width:1.5px;

class CODE,JVM,DDJ app;
class DDA agent;
class PXY proxy;
class DDOG dest;
class NOTE note;