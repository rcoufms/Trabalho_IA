# Trabalho_IA

# Pipeline de Recuperação de Informação (RI) para ZK-Compliance

Este repositório contém o projeto prático desenvolvido para a disciplina de Inteligência Artificial da Pós-Graduação em Ciência da Computação (PPGCC/FACOM) na Universidade Federal de Mato Grosso do Sul (UFMS).

O objetivo do trabalho é aplicar e avaliar dois paradigmas de Recuperação de Informação — Busca Esparsa (Léxica) e Busca Densa (Semântica) — sobre um corpus altamente especializado de artigos científicos focado em **Provas de Conhecimento Zero (ZKP), Auditoria de Dados e Conformidade (GDPR/LGPD)**, denominado **ZK-Compliance**.

---

## Estrutura do Repositório

O projeto segue estritamente a estrutura de diretórios e padrões de nomenclatura exigidos no enunciado:

*   `./baseline_bm25.ipynb`: Notebook unificado contendo o pipeline completo (Coleta, BM25, KNN e Avaliação).
*   `./eval/`: Diretório contendo os arquivos de insumo e gabaritos de avaliação.
    *   `queries.tsv`: Lista com as 11 consultas customizadas do domínio.
    *   `qrels.tsv`: Gabarito oficial de julgamento de relevância gerado via técnica de *Pooling* (Top-3).
*   `./runs/`: Arquivos de saída formatados no padrão regulamentar da TREC.
    *   `bm25.trec`: Resultados de busca do modelo esparso (Baseline).
    *   `knn.trec`: Resultados de busca do modelo denso (Avançado).

---

## Tecnologias e Modelos Utilizados

1.  **Coleta de Dados:** Consumo automatizado da API oficial do ArXiv para estruturação do corpus com **673 artigos científicos únicos**.
2.  **Modelo Esparso (Baseline):** Algoritmo **BM25Okapi** alimentado com um pré-processador customizado em Regex (limpeza de pontuação e remoção de *stopwords* via NLTK). Hiperparâmetros: $k_1 = 1.5$ e $b = 0.75$.
3.  **Modelo Denso (Avançado):** Rede neural **SPECTER2** (`allenai/specter2_base`) para geração de embeddings científicos de 768 dimensões, com indexação e busca por similaridade de cosseno via **FAISS (KNN)**.
4.  **Avaliação Mecânica:** Biblioteca **`pytrec_eval`** computando as métricas macro-médias oficiais do ecossistema TREC.

---

## Resultados Obtidos

Diferente do comportamento observado em bases de dados genéricas, o domínio estrito e terminológico de criptografia e ZK-Compliance favoreceu o casamento exato de termos do modelo léxico:

| Métrica | Baseline (BM25) | Avançado (KNN Denso) |
| :--- | :---: | :---: |
| **P@5 (Precisão no Top-5)** | **0.654545** | 0.545455 |
| **MAP (Mean Average Precision)** | **0.589960** | 0.544868 |
| **NDCG@10 (Ganho Cumulativo)** | **0.687313** | 0.648586 |

### Breve Discussão:
O **BM25** superou o KNN Denso devido à presença de jargões matemáticos e siglas de alta especificidade (como *ZK-SNARK, ZK-STARK, Bulletproofs*). O modelo denso sofreu com a diluição semântica no espaço hiperdimensional, aproximando conceitos criptográficos correlatos, mas conceitualmente distintos, o que gerou falsos positivos no topo do ranking.

---

## Como Reproduzir este Projeto

1.  Clone este repositório em sua máquina ou faça o upload do arquivo `baseline_bm25.ipynb` diretamente no Google Colab.
2.  Certifique-se de que a estrutura de pastas `/eval` e `/runs` esteja mapeada corretamente no seu ambiente de execução.
3.  Execute as células sequencialmente. O próprio script encarregar-se-á de instalar as dependências obrigatórias (`sentence-transformers`, `faiss-cpu`, `pytrec_eval`), baixar o modelo SPECTER2 e gerar as métricas na tela.

---

## 📹 Defesa Metodológica em Vídeo

[INSIRA O LINK DO SEU VÍDEO DO YOUTUBE/DRIVE AQUI]
