# Especificação do Formato de Arquivo OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Visão Geral

O arquivo OLAMIP (`/olamip.json`) é um documento JSON estruturado que fornece resumos selecionados das páginas mais importantes do seu site. Ele foi projetado para ser facilmente interpretado por modelos de linguagem de grande escala (LLMs), permitindo que compreendam, priorizem e utilizem seu conteúdo com clareza, precisão e intenção.

## Localização do Arquivo

O arquivo OLAMIP deve estar hospedado na raiz do domínio:

`https://yourdomain.com/olamip.json`

## Declaração da Localização do Arquivo OLAMIP

Para garantir que os sistemas possam localizar o arquivo OLAMIP de forma confiável, adicione tanto uma tag `<link>` quanto uma tag `<meta>` na seção `<head>` do seu site.

### Descoberta Principal: `<link rel="olamip">`

- Prática padronizada: crawlers já analisam tags `<link>`.
- Amigável para máquinas: define relação formal com o arquivo OLAMIP.
- Interoperabilidade: compatível com padrões web existentes.

### Descoberta Alternativa: `<meta name="olamip-location">`

- Fácil de entender para humanos.
- Compatível com diversas ferramentas.
- Funciona como fallback.

### Implementação Recomendada

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## Estrutura do Arquivo

Deve ser um JSON válido (UTF‑8) contendo:

- `protocol`
- `version`
- `identity`
- `content`
- `metadata`

### Estrutura Geral

```json
{
  "protocol": "OLAMIP",
  "version": "1.0",
  "identity": {},
  "content": {},
  "metadata": {}
}
```

## Objeto Identity

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---:|---|
| `name` | `string` | Sim | Nome do site ou organização |
| `type` | `string` | Sim | Tipo (ex: company, blog) |
| `canonical_description` | `string` | Sim | Descrição do site |
| `tags` | `array<string>` | Não | Palavras-chave |

## Objeto Content

Contém:

- `overview`
- `sections`

### Overview

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---:|---|
| `summary` | `string` | Sim | Resumo do site |

## Section

Representa uma categoria de conteúdo.

### Campos

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---:|---|
| `title` | `string` | Sim | Título |
| `summary` | `string` | Sim | Descrição |
| `url` | `string` | Sim | URL |
| `section_type` | `string` | Sim | Tipo |
| `policy` | `string` | Não | `"allow"` ou `"forbid"` |
| `entries` | `array<Entry>` | Sim | Entradas |
| `subsections` | `array<Section>` | Não | Subseções |

## Política

- `"allow"` — permitido
- `"forbid"` — proibido

Padrão: `"allow"`

### Herança

1. Entry
2. Subsection
3. Section
4. Padrão

## Entry

Unidade mínima de conteúdo.

### Campos

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---:|---|
| `title` | `string` | Sim | Título |
| `summary` | `string` | Sim | Descrição |
| `url` | `string` | Sim | URL absoluta |
| `content_type` | `string` | Sim | Tipo |

## Tipos de Conteúdo

- `page`
- `blog_article`
- `news_article`
- `product`
- `doc_page`
- `research_paper`

## Arrays

Exemplo:

```json
"tags": ["ai", "search", "llm"]
```

## Metadata

```json
{
  "last_updated": "2026-01-21",
  "language": "pt-BR"
}
```

## Prioridade

- `"high"`
- `"medium"`
- `"low"`

## Suporte Multilíngue

Use BCP‑47:

- `pt`
- `pt-BR`
- `en`

## Validação

- JSON válido
- URLs absolutas
- Sem vírgulas finais

## Tags

Regras:

- Minúsculas
- Palavra única
- Hífens permitidos

Exemplo:

- `machine-learning`
- `data-science`

## Versionamento

- Ignorar campos desconhecidos
- Usar esquema mais recente

## Alinhamento Semântico

OLAMIP complementa schema.org, sitemaps e sistemas de rastreamento, fornecendo uma camada estruturada otimizada para IA.
