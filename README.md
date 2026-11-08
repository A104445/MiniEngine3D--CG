# MiniEngine3D--CG - Computação Gráfica

Motor gráfico 3D modular desenvolvido no âmbito da unidade curricular de Computação Gráfica (2.º semestre do 3.º ano da Licenciatura / Mestrado em Engenharia Informática) na Universidade do Minho.

* **Ano Letivo:** 2024/2025
* **Avaliação Final:** 17.3
* **Grupo:** 36

## Sobre o Projeto

O objetivo deste projeto consistiu no desenvolvimento de uma engine 3D baseada num grafo de cena hierárquico, construída em C++ e OpenGL. O sistema é modular e extensível, concebido para suportar a geração procedural de primitivas geométricas, leitura de superfícies de Bézier, aplicação de transformações geométricas dinâmicas no tempo e o carregamento de configurações completas através de ficheiros XML. 

O ecossistema divide-se em componentes geradoras (como o `generator` e o `cometgen.c` independente) e o motor de renderização interativo em tempo real (`engine`).

## Stack Tecnológica

* **Linguagem:** C++ (Padrão C++17) e C (para geradores utilitários).
* **Gráficos & Renderização:** OpenGL, GLUT / FreeGLUT, GLEW.
* **Gestão de Texturas:** DevIL (carregamento de imagens e mapeamento UV).
* **Parsing de Dados:** TinyXML (leitura flexível da árvore de elementos XML).
* **Sistema de Build:** CMake (compilação modular que linka bibliotecas estáticas locais `Utils_n_Build_lib` e `tinyXML` aos executáveis finais).

## Arquitetura e Evolução Detalhada por Fases

O motor foi construído do zero de forma incremental, garantindo robustez a cada nova etapa de complexidade:

### Fase 1: Geração de Primitivas Básicas
* Desenvolvimento do módulo `generator` focado no cálculo de vértices a partir de equações matemáticas.
* Suporte para primitivas essenciais: **Planos**, **Caixas (Cubes)**, **Esferas** e **Conos**.
* Arquitetura de dados eficiente: as coordenadas (x, y, z) e os índices de triangulação são exportados para o formato otimizado `.3d`, evitando redundância de vértices e permitindo leituras rápidas pela engine.

### Fase 2: Grafo de Cena e Transformações Geométricas
* Implementação do parser via **TinyXML**, isolando a configuração visual do código fonte. O motor lê a configuração da Câmara (Fov, Near, Far, LookAt, Up) e toda a hierarquia da cena.
* Desenvolvimento do grafo de cena suportando nós de grupo (`<group>`) aninhados. Cada nó pode ter modelos (`<models>`) e transformações matriciais acumuláveis (`<translate>`, `<rotate>`, `<scale>`).
* **Casos de Uso Desenvolvidos:** Simulação detalhada do **Sistema Solar**, recorrendo a hierarquias complexas para órbitas de luas em torno de planetas (ex: Phobos e Deimos a orbitar Marte) e desenvolvimento da primitiva procedural **Anel de Saturno** (`buildSaturnRing`).

### Fase 3: Animação e Curvas de Catmull-Rom
* Refinamento das hierarquias XML para suportar translações dinâmicas baseadas no tempo.
* Introdução de **curvas de Catmull-Rom**: os modelos 3D deixam de ser estáticos e passam a ser animados suavemente através de arrays de pontos de controlo definidos no espaço, gerando órbitas e percursos orgânicos, perfeitamente integrados no grafo de cena.

### Fase 4: Superfícies de Bézier, Normais e Utilitários Procedurais
* Geração de **Superfícies de Bézier** baseadas em coordenadas de *patches* lidas de ficheiros `.patch` (por exemplo, os clássicos bules de chá ou `teapot.patch`).
* Implementação autónoma de um **Gerador de Cometas (`cometgen.c`)**: Programa utilitário em C que gera a geometria de um cometa manipulando coordenadas esféricas através de funções de perturbação pseudo-aleatória (`add_random_variation`) no raio base, exportando os patches resultantes.
* Suporte crítico para **Iluminação e Materiais**: Cálculo transversal de vetores normais por vértice em todas as primitivas para interação realista com a luz do OpenGL.
* Integração final da biblioteca **DevIL** para o carregamento de texturas, com geração correta de coordenadas de mapeamento UV adaptadas a cada tipo de superfície geométrica (esferas, anéis, béziers).

## Como Executar Localmente

### 1. Compilar o Projeto
Na diretoria principal do projeto, cria a pasta de build e utiliza o CMake:

    mkdir build
    cd build
    cmake ..
    cmake --build .

### 2. Geração de Modelos e Patches
Utiliza os executáveis gerados para criar os ficheiros 3D que alimentarão o motor de renderização.

Exemplos de geração de primitivas (Fase 1 e 2):

    ./generator box 2 3 box.3d
    ./generator sphere 2 90 90 sphere.3d
    ./generator ring 2.2 3 4 30 30 ring.3d

Gerar a geometria do cometa a partir do código C (Fase 4):

    gcc -o cometgen cometgen.c -lm
    ./cometgen

### 3. Executar o Motor Gráfico
Inicia a engine passando como argumento o ficheiro XML que modela o mundo:

    ./engine ../test_files_phase_4/test_4_1.xml
    ./engine ../test_files_phase_2/solar_system_final.xml
    ./engine ../test_files_phase_2/solar_system_real_scale.xml

## Estrutura de Diretórios
* `src/Engine/`: Core de renderização, ciclo OpenGL/GLUT, leitura do XML e travessia (traversal) da árvore de grafos.
* `src/Generator/`: Lógica matemática para instanciar vértices, mapear normais/UVs e calcular superfícies.
* `src/Utils/`: Classes auxiliares para manusear estruturas de dados e a classe `XMLDataFormat`.
* `tinyXML/`: Dependência embutida (`tinystr`, `tinyxmlparser`, etc.) para extração ágil da topologia da cena.

## Licença
Projeto desenvolvido exclusivamente para fins académicos.
