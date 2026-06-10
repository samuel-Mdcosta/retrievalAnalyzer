# RagLens

Toolkit de avaliação de sistemas RAG (Retrieval-Augmented Generation). Mede o quão **fiel**, **relevante** e **bem recuperado** é o que o seu RAG responde — com ou sem ground truth.

> ⚠️ Projeto em construção. Estou desenvolvendo do zero como estudo de engenharia de IA.

## Por quê

Avaliar RAG na mão é lento e subjetivo. O RagLens transforma qualidade de resposta em **número reproduzível**, usando LLM-as-judge e métricas de retrieval. Plugue o seu RAG via um adapter e rode.

## Métricas

| Métrica | O que mede | Precisa de ground truth? |
|---|---|---|
| **Faithfulness** | A resposta é sustentada pelo contexto recuperado (anti-alucinação) | Não |
| **Answer Relevance** | A resposta de fato responde à pergunta | Não |
| **Context Relevance** | Fração dos chunks recuperados que são relevantes (ruído do retrieval) | Não |
| **Context Recall** | Dos fatos da referência, quantos estavam no contexto | Sim |
| **Context Precision + MRR** | Qualidade do ranking dos chunks recuperados | Sim |

## Como funciona

```
Seu RAG ──(RagAdapter)──> Sample ──> Métricas ──> EvalRun
   retrieve()              question     (juiz LLM       agregados +
   generate()              contexts      + embeddings)   por pergunta
                           answer
```

Você implementa um `RagAdapter` com `retrieve()` e `generate()`. O RagLens monta os `Sample`, roda as métricas e devolve os resultados agregados.

## Instalação

```bash
git clone https://github.com/<seu-user>/xxxxxx
cd xxxxx/packages/xxx-core
pip install -e .
```

## Uso

```bash
# rodar a avaliação sobre um dataset
xxxx run --dataset datasets/smoke.jsonl
```

Para avaliar o seu próprio RAG, implemente a interface `RagAdapter` (veja `examples/`).

## Stack

- **Core:** Python 3.11+, Pydantic v2
- **Juiz:** Gemini (`temperature=0`, prompt versionado)
- **Embeddings:** Nomic (768 dims)
- **Testes:** pytest (métricas testadas com juiz/embedder mockados — sem gastar API)

## Estrutura

```
packages/raglens-core/   # biblioteca (schemas, adapters, métricas, runner, CLI)
apps/                    # API + dashboard (semana 2)
examples/                # adapters de exemplo
datasets/                # datasets de avaliação (.jsonl)
```

## Roadmap

- [x] Contrato de dados + adapter pattern
- [ ] 3 métricas sem ground truth (faithfulness, answer/context relevance)
- [ ] Runner + CLI ponta a ponta
- [ ] Dataset golden + métricas com ground truth
- [ ] Persistência (SQLite/MongoDB) + API FastAPI
- [ ] Dashboard (React) com tela de comparação de runs
- [ ] Validação do juiz contra rótulos humanos
- [ ] Docker Compose + publicação no PyPI

## Licença

MIT
