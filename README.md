# 📊 Diagnóstico de Falhas RAG - Lab 004 🚀

## 📌 Objetivo

Diagnosticar falhas comuns em pipelines RAG:

- ❗ **Chunk mal dimensionado**
- ❗ **Retrieval mismatch (vocabulário divergente)**
- ❗ **Alucinação por prompt sem âncora**

## 🔧 Setup

- Reuse `client`, `LLM_MODEL`, `embed_fn` configurados no **LAB-002** (qualquer opção).
- **Provider**: Groq (`llama-3.1-8b-instant`) + embeddings locais `all-MiniLM-L6-v2`.
- **Opção A (Gemini)**: Use `gemini-embedding-001` ou `sentence-transformers/all-MiniLM-L6-v2` para forçar Cenário B.

## 📂 Estrutura do Projeto

```
04-diagnostico-falhas.ipynb
02-pipeline-rag.ipynb
data/
   corpus/
```

## 🧪 Cenários e Fixes

### 🟢 Cenário A – Chunk mal dimensionado

- **Problema**: Chunks de 200 chars sem overlap fragmentam conceitos longos → **faithfulness** baixa.
- **Fix**: Re‑indexar com `chunk_size=800` e `overlap=100` (valores do LAB‑002 original).
- **Resultado esperado**: Aumento da faithfulness e chunks maiores com menos fragmentação.

### 🟡 Cenário B – Retrieval mismatch

- **Problema**: Vocabulário coloquial vs técnico (ex.: “Como o computador aprende sozinho?” vs “unsupervised learning”).
- **Fix**: Expandir queries usando **query expansion** (tradução pt→en via LLM) e re‑escrever com termos técnicos do corpus.
- **Métrica**: `context_precision` sobe quando os hits são relevantes.

### 🔴 Cenário C – Alucinação por prompt sem âncora

- **Problema**: Prompt sem instrução de ancoragem → LLM inventa respostas (ex.: “Quantos parâmetros tem o GPT‑5?”).
- **Fix**: Usar **prompt ancorado** com fallback “Não encontrado” (`RAG_PROMPT_ANCHORED`).
- **Resultado**: Resposta correta de falha (não inventa número).

## 📋 Tabela de Diagnóstico (preencha após execução)

| Cenário    | Falha Observada             | Métrica Afetada        | Fix Aplicado                                      | Métrica Depois           |
| ----------- | --------------------------- | ----------------------- | ------------------------------------------------- | ------------------------- |
| **A** | Fragmentação de conceitos | faithfulness baixa      | chunk_size=800, overlap=100                       | faithfulness sobe         |
| **B** | Top‑5 irrelevantes         | context_precision baixa | query rewrite com termos técnicos                | context_precision sobe    |
| **C** | Resposta inventada          | faithfulness = 0        | prompt com âncora e fallback “Não encontrado” | resposta correta de falha |

> **⚠️ Atenção**: Cada cenário deve ter diagnóstico textual (≥ 3 linhas) e a tabela final deve estar preenchida.

## ✅ Checklist de Verificação

- [X] Os três cenários foram executados e as métricas registradas
- [X] Diagnóstico textual para cada cenário (≥ 3 linhas) identificando a causa
- [X] Um fix por cenário aplicado e métrica medida novamente
- [X] Tabela final preenchida

## 🛠️ Troubleshooting

- **Cenário A**: Use perguntas com explicações > 400 caracteres para forçar fragmentação.
- **Cenário B**: Troque para `all-MiniLM-L6-v2` ou use BM25 puro para intensificar o mismatch.
- **Cenário C**: Troque para modelos mais antigos (ex.: `gpt-3.5-turbo`) para reproduzir alucinação.

---

## 👩‍💻 Autora

Danielle Magalhães Ballester
Residência Tecnológica do SiDi — Turma de IA Aplicada

---

🚀 **Bom trabalho e boa exploração das falhas RAG!** 🎓
