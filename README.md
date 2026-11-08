# MiniEngine3D--CG - Computação Gráfica

Motor gráfico 3D modular desenvolvido no âmbito da unidade curricular de Computação Gráfica (2.º semestre do 3.º ano da Licenciatura / Mestrado em Engenharia Informática) na Universidade do Minho[cite: 2].

* **Ano Letivo:** 2024/2025[cite: 2]
* **Avaliação Final:** 17.3[cite: 2]
* **Grupo:** 36[cite: 2]

## Sobre o Projeto

O objetivo do projeto consistiu no desenvolvimento de uma mini engine 3D baseada em grafo de cena, modular e extensível, com suporte a primitivas geométricas procedurais, transformações geométricas, curvas, superfícies de Bézier e ficheiros de configuração XML[cite: 2]. O sistema divide-se em duas ferramentas principais:
1. **Generator:** Aplicação responsável pela criação procedural de primitivas geométricas e superfícies, exportando para ficheiros de vértices e índices (`.3d` ou `.patch`)[cite: 2].
2. **Engine:** Motor de renderização em tempo real em OpenGL que lê a estrutura hierárquica da cena a partir de ficheiros XML[cite: 2].

## Stack Tecnológica

* **Linguagem:** C++ (Padrão C++17)[cite: 2]
* **Gráficos & Renderização:** OpenGL, GLUT, GLEW[cite: 2]
* **Gestão de Texturas:** DevIL[cite: 2]
* **Parsing de XML:** TinyXML[cite: 2]
* **Sistema de Build:** CMake[cite: 2]

## Descrição Detalhada por Fases

O projeto foi construído de forma incremental ao longo de quatro fases principais:

### Fase 1: Geração de Primitivas Básicas
* Foco na criação das bases geométricas elementares no módulo `generator`.
* Implementação de primitivas como **Planos**, **Caixas (Cubes)**, **Esferas** e **Conos**, calculando vértices e índices de triangulação.
* Exportação dos dados para formato `.3d` para leitura posterior[cite: 2].

### Fase 2: Transformações Geométricas e Ficheiros XML
* Introdução do motor de renderização (`engine`) com capacidade de parsing de ficheiros XML (TinyXML)[cite: 2].
* Suporte para transformações geométricas hierárquicas aplicadas aos modelos: **Translação**, **Rotação** e **Escala**[cite: 2].
* Implementação do sistema de câmara interativa baseada em coordenadas esféricas (rato e teclado)[cite: 2].
* Cenários de teste complexos, incluindo a simulação do **Sistema Solar** e novas primitivas como o **Anel de Saturno** (`buildSaturnRing`)[cite: 2].

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
