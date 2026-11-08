# MiniEngine3D--CG - Computação Gráfica

Motor gráfico 3D modular desenvolvido no âmbito da unidade curricular de Computação Gráfica (2.º semestre do 3.º ano da Licenciatura / Mestrado em Engenharia Informática) na Universidade do Minho[cite: 4].

* **Ano Letivo:** 2024/2025[cite: 4]
* **Avaliação Final:** 17.3[cite: 4]
* **Grupo:** 36[cite: 4]

## Sobre o Projeto

O objetivo deste projeto consistiu no desenvolvimento de uma engine 3D baseada num grafo de cena hierárquico, construída em C++ e OpenGL[cite: 4]. O sistema é modular e extensível, concebido para suportar a geração procedural de primitivas geométricas, leitura de superfícies de Bézier, aplicação de transformações geométricas dinâmicas no tempo e o carregamento de configurações completas através de ficheiros XML[cite: 4]. 

O ecossistema divide-se em componentes geradoras (como o `generator` e o `cometgen.c` independente) e o motor de renderização interativo em tempo real (`engine`)[cite: 4].

## Stack Tecnológica

* **Linguagem:** C++ (Padrão C++17) e C (para geradores utilitários)[cite: 4].
* **Gráficos & Renderização:** OpenGL, GLUT / FreeGLUT, GLEW[cite: 4].
* **Gestão de Texturas:** DevIL (carregamento de imagens e mapeamento UV)[cite: 4].
* **Parsing de Dados:** TinyXML (leitura flexível da árvore de elementos XML)[cite: 4].
* **Sistema de Build:** CMake (compilação modular que linka bibliotecas estáticas locais `Utils_n_Build_lib` e `tinyXML` aos executáveis finais)[cite: 4].

## Arquitetura e Evolução Detalhada por Fases

O motor foi construído do zero de forma incremental, garantindo robustez a cada nova etapa de complexidade:

### Fase 1: Geração de Primitivas Básicas
* Desenvolvimento do módulo `generator` focado no cálculo de vértices a partir de equações matemáticas.
* Suporte para primitivas essenciais: **Planos**, **Caixas (Cubes)**, **Esferas** e **Conos**[cite: 4].
* Arquitetura de dados eficiente: as coordenadas (x, y, z) e os índices de triangulação são exportados para o formato otimizado `.3d`, evitando redundância de vértices e permitindo leituras rápidas pela engine[cite: 4].

### Fase 2: Grafo de Cena e Transformações Geométricas
* Implementação do parser via **TinyXML**, isolando a configuração visual do código fonte. O motor lê a configuração da Câmara (Fov, Near, Far, LookAt, Up) e toda a hierarquia da cena[cite: 4].
* Desenvolvimento do grafo de cena suportando nós de grupo (`<group>`) aninhados. Cada nó pode ter modelos (`<models>`) e transformações matriciais acumuláveis (`<translate>`, `<rotate>`, `<scale>`)[cite: 4].
* **Casos de Uso Desenvolvidos:** Simulação detalhada do **Sistema Solar**, recorrendo a hierarquias complexas para órbitas de luas em torno de planetas (ex: Phobos e Deimos a orbitar Marte) e desenvolvimento da primitiva procedural **Anel de Saturno** (`buildSaturnRing`)[cite: 4].

### Fase 3: Animação e Curvas de Catmull-Rom
* Refinamento das hierarquias XML para suportar translações dinâmicas baseadas no tempo[cite: 4].
* Introdução de **curvas de Catmull-Rom**: os modelos 3D deixam de ser estáticos e passam a ser animados suavemente através de arrays de pontos de controlo definidos no espaço, gerando órbitas e percursos orgânicos, perfeitamente integrados no grafo de cena[cite: 4].

### Fase 4: Superfícies de Bézier, Normais e Utilitários Procedurais
* Geração de **Superfícies de Bézier** baseadas em coordenadas de *patches* lidas de ficheiros `.patch` (por exemplo, os clássicos bules de chá ou `teapot.patch`)[cite: 4].
* Implementação autónoma de um **Gerador de Cometas (`cometgen.c`)**: Programa utilitário em C que gera a geometria de um cometa manipulando coordenadas esféricas através de funções de perturbação pseudo-aleatória (`add_random_variation`) no raio base, exportando os patches resultantes[cite: 4].
* Suporte crítico para **Iluminação e Materiais**: Cálculo transversal de vetores normais por vértice em todas as primitivas para interação realista com a luz do OpenGL[cite: 4].
* Integração final da biblioteca **DevIL** para o carregamento de texturas, com geração correta de coordenadas de mapeamento UV adaptadas a cada tipo de superfície geométrica (esferas, anéis, béziers)[cite: 4].

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
* `src/Engine/`: Core de renderização, ciclo OpenGL/GLUT, leitura do XML e travessia (traversal) da árvore de grafos[cite: 4].
* `src/Generator/`: Lógica matemática para instanciar vértices, mapear normais/UVs e calcular superfícies[cite: 4].
* `src/Utils/`: Classes auxiliares para manusear estruturas de dados e a classe `XMLDataFormat`[cite: 4].
* `tinyXML/`: Dependência embutida (`tinystr`, `tinyxmlparser`, etc.) para extração ágil da topologia da cena[cite: 4].

## Licença
Projeto desenvolvido exclusivamente para fins académicos.
