# Sistema Distribuido de Monopatines – Arquitectura General

## Repositorio principal del proyecto 
Este repositorio centraliza toda la documentación arquitectónica, los diagramas, y la justificación técnica del sistema distribuido compuesto por tres microservicios independientes:

* `viaje_service`
* `pausa_service`
* `tarifa_service`

Cada microservicio tiene su propio repositorio y código fuente.
Este cuarto repo **no contiene código**, sino que funciona como el _hub documental del proyecto_.

## Arquitectura general
El sistema está diseñado siguiendo un enfoque de **microservicios desacoplados**, donde cada uno se encarga de una parte crítica del dominio:

| Microservicio      | Responsabilidad principal                          |
| ------------------ | -------------------------------------------------- |
| **viaje_service**  | Gestiona inicio, fin y cálculo completo del viaje. |
| **pausa_service**  | Registra pausas y calcula minutos totales.         |
| **tarifa_service** | Determina tarifas y calcula costo final.           |

Cada servicio expone APIs REST y se comunica con los demás mediante llamadas HTTP.

## Justificacion del diseño en microservicios

1. Mejorar el desarrollo en equipo
2. Bajo acoplamiento y alta cohesion
3. Independencia tecnologica y de despliegue
4. Robustez del sistema

## Repositorios del proyecto
| Servicio                | Repositorio                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| **viaje_service**       | *[Microservicio Viaje](https://github.com/Melanie-SanRoman/arqui_web_tp4_grupo2_ms_viaje)*   |
| **pausa_service**       | *[Microservicio Pausa](https://github.com/Melanie-SanRoman/arqui_web_tp4_grupo2_ms_pausa)*   |
| **tarifa_service**      | *[Microservicio Tarifa](https://github.com/Melanie-SanRoman/arqui_web_tp4_grupo2_ms_tarifa)* |
| **Repo general (este)** | Documentación y diagramas                                                                    |


## Diagramas del sistema
A continuación se incluyen los principales diagramas que explican el sistema.

- Diagrama - Funcionalidades por servicio
``` mermaid
flowchart LR
%% Estilos globales
%% ----------------------------------------
classDef service fill:#e8f1ff,stroke:#3a6ea5,stroke-width:2px,color:#0b2545,rx:10,ry:10;
classDef db fill:#fff4d6,stroke:#b68b00,stroke-width:2px,color:#4e3b00,rx:10,ry:10;
classDef op fill:#e2ffe9,stroke:#41a35a,stroke-width:2px,color:#1a4e26,rx:10,ry:10;
classDef connector stroke-dasharray: 5 5;
    subgraph Viaje_Service ["viaje_service"]
        V1[Iniciar viaje]:::op
        V2[Finalizar viaje]:::op
        V3[Consultar viaje]:::op
    end

    subgraph Pausa_Service ["pausa_service"]
        P1[Registrar pausa]:::op
        P2[Devolver minutos totales]:::op
        P3[Listar pausas por viaje]:::op
    end

    subgraph Tarifa_Service ["tarifa_service"]
        T1[Calcular costo]:::op
        T2[Determinar tarifa]:::op
    end

    V2 -->|Consulta minutos| P2
    V2 -->|Lista pausas| P3
    V2 -->|Envía km y pausas| T1
    T1 -->|Retorna costo final| V2
```

- Diagrama C4 - Arquitectura distribuida

``` mermaid
%% Estilos globales
graph TD
classDef ms fill:#e3f2fd,stroke:#64b5f6,stroke-width:2px,color:#0d47a1,rx:10px,ry:10px;
classDef db fill:#fff3e0,stroke:#ffb74d,stroke-width:2px,color:#e65100,rx:10px,ry:10px;
classDef comp fill:#e8f5e9,stroke:#81c784,stroke-width:2px,color:#1b5e20,rx:10px,ry:10px;
classDef persona fill:#fce4ec,stroke:#f06292,stroke-width:2px,color:#880e4f,rx:10px,ry:10px;
classDef relation stroke-dasharray: 5 5;
subgraph Viaje_Service
    viaje([viaje_service]):::ms
    viaje_db[(viaje_db)]:::db
    viaje --> viaje_db
end

subgraph Pausa_Service
    pausa([pausa_service]):::ms
    pausa_db[(pausa_db)]:::db
    pausa --> pausa_db
end

subgraph Tarifa_Service
    tarifa([tarifa_service]):::ms
    tarifa_db[(tarifa_db)]:::db
    tarifa --> tarifa_db
end

subgraph Monopatin_Service
    monopatin([monopatin_service]):::ms
    monopatin_db[(monopatin_db)]:::db
    monopatin --> monopatin_db
end

viaje -.->|REST: minutos pausa| pausa
viaje -.->|REST: costo total| tarifa
viaje -.->|REST: ubicación| monopatin
```

- Diagrama de clases del dominio
``` mermaid
classDiagram
    class Viaje {
      Long id
      LocalDate inicio
      LocalDate fin
      Long monopatinId
      double kilometros
      double costo
    }

    class Pausa {
      Long id
      Long viajeId
      LocalDateTime inicio
      LocalDateTime fin
    }

    class Tarifa {
      Long id
      String tipo
      double montoBase
      double precioKM
      double precioMinPausa
    }
```

## Colaboradores

> * Langenheim, Geronimo V.
> * Lopez, Micaela N.
> * San Roman, Emanuel.
> * San Roman, Melanie.


