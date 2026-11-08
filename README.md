# MiniEngine3D--CG - Computação Gráfica

Motor gráfico 3D modular desenvolvido no âmbito da unidade curricular de Computação Gráfica (2.º semestre do 3.º ano da Licenciatura / Mestrado em Engenharia Informática) na Universidade do Minho.

* **Ano Letivo:** 2024/2025
* **Avaliação Final:** 17.3
* **Grupo:** 36

## Sobre o Projeto

O objetivo do projeto consistiu no desenvolvimento de uma mini engine 3D baseada em grafo de cena, modular e extensível, com suporte a primitivas geométricas procedurais, transformações geométricas, curvas, superfícies de Bézier e ficheiros de configuração XML. O sistema divide-se em duas ferramentas principais:
1. **Generator:** Aplicação responsável pela criação procedural de primitivas geométricas e superfícies, exportando para ficheiros de vértices e índices (`.3d` ou `.patch`).
2. **Engine:** Motor de renderização em tempo real em OpenGL que lê a estrutura hierárquica da cena a partir de ficheiros XML.

## Stack Tecnológica

* **Linguagem:** C++ (Padrão C++17)
* **Gráficos & Renderização:** OpenGL, GLUT, GLEW
* **Gestão de Texturas:** DevIL
* **Parsing de XML:** TinyXML
* **Sistema de Build:** CMake

## Descrição Detalhada por Fases

O projeto foi construído de forma incremental ao longo de quatro fases principais:

### Fase 1: Geração de Primitivas Básicas
* Foco na criação das bases geométricas elementares no módulo `generator`.
* Implementação de primitivas como **Planos**, **Caixas (Cubes)**, **Esferas** e **Conos**, calculando vértices e índices de triangulação.
* Exportação dos dados para formato `.3d` para leitura posterior.

### Fase 2: Transformações Geométricas e Ficheiros XML
* Introdução do motor de renderização (`engine`) com capacidade de parsing de ficheiros XML (TinyXML).
* Suporte para transformações geométricas hierárquicas aplicadas aos modelos: **Translação**, **Rotação** e **Escala**.
* Implementação do sistema de câmara interativa baseada em coordenadas esféricas (rato e teclado).
* Cenários de teste complexos, incluindo a simulação do **Sistema Solar** e novas primitivas como o **Anel de Saturno** (`buildSaturnRing`).

### Fase 3: Grafos de Cena Hierárquicos e Curvas
* Evolução da estrutura XML para suportar um **grafo de cena completo** com grupos aninhados e acumulação de transformações geométricas.
* Implementação de **curvas de Catmull-Rom**, permitindo animar objetos ao longo de trajetórias tridimensionais complexas na cena.

### Fase 4: Superfícies de Bézier, Normais e Texturas
* Implementação de geradores de **superfícies de Bézier** a partir de ficheiros de patches (`.patch`).
* Cálculo automático de normais por vértice e mapeamento de **coordenadas de textura** (UV mapping).
* Integração da biblioteca DevIL para carregamento e aplicação de texturas e materiais aos modelos renderizados em OpenGL.

## Como Executar

### 1. Compilação com CMake
Na diretoria do projeto, executa os seguintes comandos no terminal:

    mkdir build
    cd build
    cmake ..
    cmake --build .

### 2. Gerar Primitivas (.3d)
Exemplo para gerar uma caixa ou esfera através do generator:

    ./generator box 2 3 box.3d
    ./generator sphere 2 90 90 sphere.3d

### 3. Executar o Motor (Engine)
Carregar uma cena a partir de um ficheiro de configuração XML:

    ./engine ../test_files_phase_2/test_2_1.xml

## Licença

Projeto desenvolvido para fins académicos.
