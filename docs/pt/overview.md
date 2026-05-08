# Visão geral do OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP é um padrão aberto que permite que sites se comuniquem de forma clara e intencional com sistemas de IA. Em vez de forçar grandes modelos de linguagem (LLMs) a adivinhar o significado a partir de HTML, CSS e JavaScript ruidosos, o OLAMIP fornece uma representação limpa, estruturada e interpretável por máquinas do conteúdo mais importante do seu site.

Este documento oferece uma introdução de alto nível ao propósito, à filosofia, ao formato do arquivo e ao modelo de atualização do OLAMIP.

---

## 1. Por que o OLAMIP foi criado

Os sites modernos são construídos para navegadores, não para IA. Como resultado, os LLMs enfrentam dificuldades com:

- HTML com muito layout  
- Conteúdo renderizado por JavaScript  
- Menus de navegação, anúncios e texto repetitivo  
- Páginas duplicadas ou irrelevantes  

Os LLMs *já* rastreiam o seu site, mas muitas vezes o interpretam de forma incorreta. O OLAMIP resolve isso ao fornecer aos sistemas de IA exatamente o que eles precisam:

- Um arquivo JSON leve (`/olamip.json`)  
- Resumos limpos, curados por humanos  
- Metadados estruturados  
- URLs canônicas  
- Uma hierarquia clara de seções, subseções e entradas  

Ao adotar o OLAMIP, você assume o controle de como a IA entende e representa o seu conteúdo.

---

## 2. Filosofia central: amigável para humanos + amigável para máquinas

O OLAMIP foi projetado como uma ponte de comunicação entre humanos e IA. Cada parte do protocolo deve ser:

| Principle | Human‑Friendly | Machine‑Friendly |
|-----------|----------------|------------------|
| File Format | Fácil de escrever e validar | JSON estrito e previsível |
| Field Names | Intuitivos e autoexplicativos | Em minúsculas, compatíveis com schema |
| Summaries | Linguagem clara, alinhada à marca | Concisa, factual e semanticamente rica |
| Priority System | Etiquetas simples (`"high"`, `"medium"`, `"low"`) | Mapeável para pesos de ranqueamento |
| Documentation | Guias em linguagem simples | JSON schemas, regras de validação |
| Tooling | Geradores, plugins de CMS | Ferramentas CLI, parsers, suites de teste |

- **Para humanos:** o OLAMIP deve ser fácil de adotar sem necessidade de conhecimento técnico profundo.  
- **Para máquinas:** o OLAMIP deve ser estruturado, previsível e sem ambiguidade.

---

## 3. O arquivo OLAMIP (`/olamip.json`)

Um arquivo OLAMIP é um documento JSON estruturado hospedado em:

`https://yourdomain.com/olamip.json`

Ele contém:

- `protocol` — deve ser `"OLAMIP"`  
- `version` — versão do protocolo  
- `identity` — quem você é  
- `content` — a hierarquia estruturada do seu site  
- `metadata` — idioma, última atualização e outros campos globais  

### 3.1 Tags de descoberta

Para garantir que os sistemas de IA consigam localizar seu arquivo OLAMIP de forma confiável, inclua ambos:

```html
//yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

Usar ambos fornece redundância, compatibilidade e preparação para o futuro.

---

## 4. Estrutura do arquivo

### 4.1 Objeto Identity

Descreve o site ou a organização:

- `name` — obrigatório  
- `type` — obrigatório  
- `canonical_description` — obrigatório  
- `tags` — opcional  

### 4.2 Objeto Content

Contém:

- Um `overview`  
- `sections`  
- `subsections` opcionais  
- `entries` (as unidades de conteúdo mais granulares)  

Isso suporta profundidade de aninhamento ilimitada.

### 4.3 Sections

Uma Section agrupa conteúdo relacionado. Os campos obrigatórios incluem:

- `title`  
- `summary`  
- `url`  
- `section_type`  

Os campos opcionais incluem:

- `policy` (`"allow"` ou `"forbid"`)  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Herança de policy:**  
Se omitida, a policy é herdada dos ancestrais. O padrão é `"allow"`.

### 4.4 Entries

Entries representam itens individuais de conteúdo, como:

- Artigos de blog  
- Notícias  
- Produtos  
- Páginas de documentação  
- Trabalhos de pesquisa  
- Itens de mídia  

Campos obrigatórios:

- `title`  
- `summary`  
- `url`  
- `content_type`  

Campos opcionais:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

As URLs são obrigatórias porque identificam o conteúdo de forma única e permitem que os sistemas de IA verifiquem, deduplicem e cruzem referências de páginas.

---

## 5. Metadados e suporte a idiomas

O objeto metadata pode incluir:

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### Suporte multilíngue

O idioma pode ser definido em:

- Nível de arquivo  
- Nível de seção  
- Nível de entrada  

Use códigos BCP‑47 (por exemplo, `en`, `es`, `pt-BR`, `zh-CN`).

Isso ajuda os sistemas de IA a:

- Escolher os tokenizadores corretos  
- Evitar mistura de idiomas  
- Melhorar a precisão de recuperação  
- Reduzir alucinações  

---

## 6. Tags e prioridade

### 6.1 Tags

As tags fornecem sinais semânticos leves. Elas devem ser:

- Em minúsculas  
- De uma única palavra  
- ASCII  
- Com hífen para conceitos com várias palavras  
- Consistentes em todas as entradas  

Exemplos:

| Concept | Valid Tag |
|---------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

As tags ajudam os sistemas de IA a agrupar, desambiguar e recuperar conteúdo com mais precisão.

### 6.2 Priority

Valores:

- `high` — conteúdo principal  
- `medium` — padrão  
- `low` — conteúdo de nicho ou desatualizado  

Use `high` com moderação (5–10% das entradas).

---

## 7. Atualizações delta (`olamip-delta.json`)

Os sites evoluem constantemente. O OLAMIP suporta atualizações incrementais por meio de um arquivo complementar opcional:

`/olamip-delta.json`

Este arquivo contém apenas:

- Entradas adicionadas  
- Entradas atualizadas  
- URLs removidas  

### 7.1 Por que as atualizações delta importam

- Os sistemas de IA permanecem atualizados sem reprocessar o arquivo completo.  
- Grandes catálogos continuam gerenciáveis.  
- As atualizações se propagam rapidamente.  
- Nenhuma mudança é perdida.  

### 7.2 Janela delta contínua (recomendada)

Mantenha todos os deltas dos últimos 7–30 dias em um único arquivo.  
Os sistemas de IA os aplicam cronologicamente para permanecer sincronizados.

### 7.3 Arquivos delta versionados (alternativa)

Publique arquivos separados:

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...  

Ideal para sites de alto volume (notícias, e-commerce, documentação).

### 7.4 Descoberta

Somente o arquivo principal do OLAMIP precisa de tags `>` e `<meta>`.  
Os sistemas de IA procuram automaticamente arquivos delta no mesmo diretório.

---

## 8. OLAMIP vs sitemaps tradicionais

- Sitemap XML: “Aqui estão minhas páginas.”  
- OLAMIP: “Aqui está o que meu site significa.”  

Sitemaps apenas listam URLs.  
OLAMIP fornece:

- Resumos  
- Tipos de conteúdo  
- Hierarquia de seções  
- Tags  
- Metadados  
- Prioridades  
- Políticas de ingestão  

Junto com schema.org, o OLAMIP oferece aos sistemas de IA uma compreensão completa e curada por humanos do seu site.

---

## 9. Perguntas frequentes (resumo)

### O OLAMIP expõe informações privadas?

Não. Ele apenas resume conteúdo disponível publicamente.

### Concorrentes podem usar meu arquivo OLAMIP?

Não mais do que já usam suas páginas públicas.

### Qual o tamanho que um arquivo OLAMIP pode ter?

1–10 MB é o mais comum.  
Mesmo 3.000+ entradas é normal e seguro.

### Com que frequência devo atualizá-lo?

Sempre que o conteúdo mudar — trate-o como um sitemap para IA.

### Preciso atualizar o arquivo principal se usar deltas?

Sim. `olamip.json` é sempre o snapshot autoritativo.

### Os LLMs realmente vão ler isso?

Sim — esse é o propósito do protocolo.  
Você pode testar com:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. Resumo

O OLAMIP transforma o seu site em um mapa de conhecimento interpretável por máquinas:

- `olamip.json` — seu snapshot completo e estruturado  
- `olamip-delta.json` — seu log incremental de mudanças  
- Sections, subsections, entries — uma hierarquia clara  
- Summaries, tags, priorities — significado curado por humanos  
- Tags de descoberta — ingestão confiável  
- Suporte multilíngue — prontidão global  

Ao adotar o OLAMIP, você garante que os sistemas de IA entendam seu conteúdo da maneira como você pretende — com precisão, consistência e em escala.
