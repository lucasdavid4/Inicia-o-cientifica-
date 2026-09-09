# Desenvolvimento de Modelos Paramétricos 2D/3D de Dispositivos em Fibra Óptica com Aplicações em Sensoriamento

Repositório associado ao plano de trabalho **PIN2166-2024**, desenvolvido no âmbito do **PIBITI/UFPE/CNPq**, Edital PROPESQI nº 006/2025.

O projeto tem como foco o desenvolvimento de modelos paramétricos bidimensionais e tridimensionais de dispositivos em fibra óptica, com ênfase em fibras de perfil D, visando sua utilização em simulações eletromagnéticas e aplicações em sensoriamento.

## Identificação

- **Discente:** Lucas David Lima Ferreira
- **Orientador:** Prof. Jehan Fonseca do Nascimento
- **Instituição:** Universidade Federal de Pernambuco
- **Unidade:** Núcleo Interdisciplinar de Ciências Exatas e da Natureza
- **Programa:** PIBITI/UFPE/CNPq
- **Vigência:** 01/09/2025 a 31/08/2026
- **Projeto associado:** Desenvolvimento de Sensor de Campo Magnético à Base de Fibra Óptica para Atividades em Ciência e Tecnologia Verde
- **TRL atual:** 3
- **ODS relacionado:** ODS 9 — Indústria, Inovação e Infraestrutura

## Objetivo

Desenvolver modelos paramétricos 2D e 3D de dispositivos em fibra óptica compatíveis com plataformas de simulação física, permitindo representar, modificar e validar diferentes configurações geométricas antes de etapas experimentais e de prototipagem.

Entre os objetivos específicos estão:

- construir geometrias 2D de fibras com e sem curvatura;
- gerar modelos 3D por operações paramétricas;
- exportar geometrias em formatos **DXF** e **STEP**;
- reconstruir e validar as geometrias no **COMSOL Multiphysics**;
- analisar a influência da curvatura sobre a resposta óptica;
- organizar os modelos e resultados em um repositório digital reutilizável.

## Ferramentas utilizadas

### Autodesk Inventor

Utilizado para a modelagem paramétrica das estruturas 2D e 3D.

Embora o plano inicial previsse o uso do FreeCAD, durante a execução do projeto o Autodesk Inventor foi adotado como ferramenta CAD principal, mantendo os objetivos metodológicos originalmente propostos.

### COMSOL Multiphysics

Utilizado para a reconstrução das geometrias 2D e realização das simulações eletromagnéticas.

A física utilizada foi:

- **Electromagnetic Waves, Frequency Domain (ewfd)**
- formulação **Full Field**
- estudo **Frequency Domain**
- resolução das três componentes do vetor campo elétrico
- número de onda fora do plano igual a zero

## Fluxo de desenvolvimento

```text
Modelagem paramétrica no Autodesk Inventor
                ↓
         Modelo tridimensional
                ↓
        Exportação em STEP
                ↓
      Extração de faces do modelo
                ↓
        Exportação em DXF
                ↓
 Conversão das curvas em expressões analíticas
                ↓
 Reconstrução da geometria 2D no COMSOL
                ↓
      Validação das interfaces
                ↓
      Simulação eletromagnética
                ↓
       Análise dos resultados
```

## Geometria da fibra

A estrutura analisada corresponde a uma fibra óptica de perfil D.

| Parâmetro | Valor |
|---|---:|
| Diâmetro do núcleo | 8,2 µm |
| Raio do núcleo | 4,1 µm |
| Diâmetro da casca | 20 µm |
| Raio da casca | 10 µm |
| Casca residual na região polida | aproximadamente 3 µm |
| Profundidade de polimento | aproximadamente 2,9 µm |
| Material metálico | Alumínio |
| Espessura metálica | 10 nm |

A camada metálica foi representada no COMSOL por uma **Transition Boundary Condition**, evitando a necessidade de criar e malhar explicitamente um domínio com apenas 10 nm de espessura.

Para o alumínio foram utilizados:

- `n = 1,5785`
- `k = 15,658`

## Configurações de curvatura

Foram avaliados quatro casos:

- fibra sem curvatura;
- `R = 2,1 cm`;
- `R = 1,4 cm`;
- `R = 0,7 cm`.

Nas estruturas curvas foi mantido aproximadamente **1 mm de comprimento de arco**, variando-se o raio de curvatura.

A comparação entre essas configurações permite avaliar como a curvatura modifica o confinamento do campo eletromagnético e a resposta óptica da fibra de perfil D.

## Parâmetros das simulações

- **Comprimento de onda:** 1,55 µm
- **Frequência aproximada:** `1,9341 × 10¹⁴ Hz`
- **Faixa do índice de refração externo:** `1,42 ≤ n_ext ≤ 1,50`

Passos utilizados na varredura:

- sem curvatura e `R = 2,1 cm`: `Δn_ext = 2,5 × 10⁻³`
- `R = 1,4 cm` e `R = 0,7 cm`: `Δn_ext = 2,5 × 10⁻⁴`

As simulações refinadas utilizaram 321 valores de índice de refração.

## Condições de contorno

- **Perfect Electric Conductor**
- **Scattering Boundary Condition**
- **Transition Boundary Condition**

A Transition Boundary Condition foi utilizada para representar a fina camada de alumínio associada ao mecanismo de **Ressonância de Plásmon de Superfície (RPS)**.

## Malha

A discretização combinou **Free Triangular** e **Mapped Mesh**.

Um dos critérios utilizados para o refinamento foi:

```text
h_max = λ / 12
```

## Cálculo da transmissão

A transmissão normalizada foi obtida a partir da razão entre integrais do módulo quadrático das componentes do campo elétrico nas seções final e inicial da fibra.

### Onda P

```text
T_P = intop2(|E_y|²) / intop1(|E_y|²)
```

### Onda S

```text
T_S = intop2(|E_z|²) / intop1(|E_z|²)
```

onde `intop1` corresponde à seção inicial da fibra e `intop2` à seção final.

## Principais resultados

Para a componente P foram observados aproximadamente os seguintes mínimos de transmissão:

| Configuração | n_ext no mínimo | Transmissão mínima |
|---|---:|---:|
| Sem curvatura | 1,4775 | 0,01339 |
| R = 2,1 cm | 1,4750 | 0,08334 |
| R = 1,4 cm | 1,4755 | 0,03132 |
| R = 0,7 cm | 1,47375 | 0,12045 |

Os resultados indicam que a modificação do raio de curvatura altera tanto a posição quanto a profundidade da região de mínimo da transmissão da onda P.

Esse comportamento é relevante para dispositivos de sensoriamento porque a fibra de perfil D aproxima o campo óptico da região externa da estrutura. A presença da curvatura modifica a distribuição do campo e sua interação com a interface metal-meio externo, afetando a resposta associada à Ressonância de Plásmon de Superfície.

## Mapas de campo elétrico

| Configuração | n_ext |
|---|---:|
| Sem curvatura | 1,47 |
| R = 2,1 cm | 1,49 |
| R = 1,4 cm | 1,49 |
| R = 0,7 cm | 1,50 |

Esses mapas permitem visualizar a redistribuição espacial do campo eletromagnético causada pela alteração da geometria.

## Relação com o sensor de campo magnético

Nesta etapa do PIBITI não foram incorporados diretamente ferrofluidos, materiais magneto-ópticos ou parâmetros dependentes de campo magnético.

O trabalho concentrou-se na construção da infraestrutura geométrica e computacional da fibra de perfil D, incluindo modelagem paramétrica, curvaturas, camada metálica, meio externo, reconstrução no COMSOL, simulação eletromagnética e análise da transmissão.

Essa base poderá ser utilizada em etapas futuras para incorporar materiais ou mecanismos cuja resposta dependa do campo magnético.

## Estrutura sugerida do repositório

```text
.
├── CAD/
│   ├── Inventor/
│   └── STEP/
├── DXF/
├── COMSOL/
├── Equacoes/
├── Resultados/
│   ├── Sem_curva/
│   ├── R_2_1/
│   ├── R_1_4/
│   └── R_0_7/
├── Figuras/
├── Documentacao/
└── README.md
```

## Limitações atuais

O trabalho permanece classificado em **TRL 3**.

Foram realizadas modelagem, simulações computacionais e validações numéricas das geometrias propostas. Entretanto, ainda não foi realizada validação experimental em ambiente laboratorial suficiente para caracterizar formalmente a evolução para TRL 4.

Em uma das varreduras com 321 valores de `n_ext`, o tempo total de solução foi de aproximadamente **11 h 31 min**, com uso de cerca de **29,4 GB de memória física**.

## Próximas etapas

- validação experimental das estruturas;
- comparação quantitativa entre simulação e experimento;
- incorporação de materiais sensíveis ao campo magnético;
- estudo de sensibilidade e resolução do sensor;
- refinamento das geometrias e da malha;
- avaliação de diferentes materiais metálicos;
- avanço do projeto em direção ao TRL 4.

## Considerações éticas

O projeto foi desenvolvido por meio de modelagem computacional, elaboração de geometrias CAD, simulações eletromagnéticas e análise de dados numéricos.

Não houve participação de seres humanos, uso de animais ou tratamento de dados pessoais. Portanto, não foi necessária submissão ao Comitê de Ética em Pesquisa ou à Comissão de Ética no Uso de Animais.

## Referências

- ALVES, H. P. *Fibra Óptica de Perfil D: Fabricação e Aplicação em Sensoriamento*. Tese, PPGEE-UFPE, 2020.
- FILHO, A. A. *Elementos Finitos: A Base da Tecnologia CAE*. São Paulo: Érica, 2013.
- FREITAS-FILHO, P. J. *Introdução à Modelagem e Simulação de Sistemas*. Editora Arena, 2008.
- MIRANDA-CASTRO, R. et al. *Biosensors*, v. 6, 2016.
- SILVA, M. S. P. *Sensor Distribuído de Temperatura à Fibra Óptica Baseado em Espalhamento Raman*. Dissertação, PPGEE-UFPE, 2018.
- YUDONG, S. et al. *Sensors*, v. 18, 2018.
- YUAN, Y. et al. *Sensors and Actuators B: Chemical*, v. 161, p. 269-273, 2012.

## Autoria

**Lucas David Lima Ferreira**  
PIBITI/UFPE/CNPq

**Orientador:** Prof. Jehan Fonseca do Nascimento  
**Instituição:** Universidade Federal de Pernambuco

---

Este repositório é destinado à organização, rastreabilidade e reutilização dos modelos, arquivos geométricos, simulações e resultados desenvolvidos durante o projeto.
