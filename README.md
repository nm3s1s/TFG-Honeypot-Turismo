# [cite_start]Sistema de Inteligencia de Ciberamenazas mediante Honeypot para el Sector Turístico de Lanzarote [cite: 2]

![Status](https://img.shields.io/badge/Status-Operativo-success)
![Institution](https://img.shields.io/badge/ASIR-CIFP_ZONZAMAS-blue)
![TFG](https://img.shields.io/badge/TFG-2025_2026-orange)

## 📌 Descripción del Proyecto

[cite_start]Este repositorio contiene la documentación, la memoria final y el código de automatización desarrollados para mi Proyecto de Fin de Grado (TFG) del ciclo ASIR en el CIFP ZONZAMAS[cite: 1, 3]. 

[cite_start]El objetivo principal de este proyecto es transformar la ciberseguridad del sector turístico —donde el 78% de las PYMES carecen de medidas de seguridad activas [cite: 8, 9][cite_start]— pasando de un modelo de defensa reactivo a uno de inteligencia proactiva[cite: 15, 16]. [cite_start]Para ello, se ha desplegado una red de señuelos (honeypots) simulando la infraestructura de una PYME real ("Apartamentos Playa Dorada") para capturar ataques, analizar técnicas y generar Indicadores de Compromiso (IOCs) reales y accionables[cite: 16, 23].

[cite_start]Durante el primer mes de operación continua, el sistema logró capturar más de 281.000 eventos de ataque[cite: 79, 81, 82].

## 🏗️ Arquitectura e Infraestructura

El despliegue técnico se fundamenta en herramientas Open Source y Cloud Computing:

* [cite_start]**Cloud Hosting:** Oracle Cloud Infrastructure (Instancia VM.Standard2.4 con 4 OCPUs y 60 GB RAM) para el procesamiento de logs en tiempo real y la ejecución de contenedores sin degradación[cite: 25, 27, 28, 29, 30].
* [cite_start]**Sistema Base:** Ubuntu 24.04 LTS (Hardened) expuesto directamente a internet mediante IP Pública para maximizar la captura de ataques[cite: 31, 32, 33, 34].
* [cite_start]**Honeypot Framework:** T-Pot 24.04 ejecutándose sobre Docker Engine para garantizar el aislamiento (sandboxing)[cite: 36, 37, 40]. Mantiene activos más de 20 sensores, incluyendo:
    * [cite_start]**Cowrie:** Simulación SSH/Telnet para captura de fuerza bruta[cite: 50, 51].
    * [cite_start]**Tanner:** Honeypot web de alta interacción simulando un PMS hotelero[cite: 52, 53].
    * [cite_start]**Dionaea:** Captura de malware para análisis forense[cite: 54, 55].
    * [cite_start]**Conpot:** Simulación de dispositivos IoT/SCADA comunes en infraestructuras hoteleras inteligentes[cite: 56, 57].
* [cite_start]**SIEM / Data Pipeline:** El stack ELK (Logstash, Elasticsearch, Kibana) normaliza los eventos al esquema ECS y permite la visualización geográfica y el análisis forense en tiempo real[cite: 44, 45, 65, 74, 76, 77].

## 📂 Contenido del Repositorio

* `/Memoria/`: Memoria oficial del TFG detallando la justificación, arquitectura, presupuesto y conclusiones.
* [cite_start]`/Presentacion/`: Diapositivas utilizadas para la defensa oral del proyecto (`.pptx`)[cite: 1].
* `/Scripts/`: Conjunto de herramientas en Python diseñadas para interactuar con la API de Elasticsearch del SOC e inferir inteligencia automatizada.

## 🛠️ Herramientas de Inteligencia (Scripts)

[cite_start]Los scripts adjuntos demuestran la viabilidad de extraer inteligencia accionable a partir de los datos crudos[cite: 90]. Para utilizarlos, se requiere conexión al entorno ELK del proyecto.

1.  `analizar_bots_vs_turistas.py`: Consulta los User-Agents capturados por Nginx/Tanner para demostrar empíricamente que el tráfico web no proviene de clientes legítimos, sino de herramientas automatizadas (bots y escáneres) buscando vulnerabilidades.
2.  `analizar_fuerza_bruta_turismo.py`: Analiza los logs del sensor Cowrie para extraer los diccionarios de usuarios y contraseñas más utilizados por los atacantes en sus intentos de acceso SSH.
3.  `extractor_ioc_turismo.py`: Identifica las IPs más agresivas y las rutas web sospechosas, catalogándolas por nivel de riesgo para su uso en la protección del perímetro.
4.  [cite_start]`extractor_phishing_tanner.py`: Audita las rutas web específicas atacadas dentro del simulador PMS hotelero, extrayendo inteligencia útil para la formación del personal contra el phishing y explotación web[cite: 95, 96].
5.  [cite_start]`generador_blacklist.py`: Automatiza la defensa traduciendo el Top 15 de IPs atacantes directamente a reglas de bloqueo de `iptables` listas para inyectarse en el firewall perimetral del hotel[cite: 93, 94].
6.  `investigacion_forense_ip.py`: Realiza un perfilado profundo (Threat Hunting) sobre la IP más hostil, trazando qué servicios específicos ha atacado y qué payloads ha intentado inyectar.

## 🚀 Uso e Instalación (Scripts)

1. Clona el repositorio:
   ```bash
   git clone [https://github.com/tu-usuario/SOC-Honeypot-Turismo.git](https://github.com/tu-usuario/SOC-Honeypot-Turismo.git)
   cd SOC-Honeypot-Turismo/Scripts

    Instala las dependencias necesarias:
    Bash

    pip install elasticsearch urllib3

    Ejecuta cualquier herramienta de análisis. Ejemplo:
    Bash

    python3 generador_blacklist.py
