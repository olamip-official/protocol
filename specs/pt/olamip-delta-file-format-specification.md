# Especificação do Formato de Arquivo OLAMIP-DELTA

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

O protocolo OLAMIP-DELTA define um formato de arquivo delta baseado em JSON para atualizações incrementais do `olamip.json`. Esta especificação é voltada para webmasters e implementadores familiarizados com dados estruturados, HTML e publicação no estilo sitemap.

## 1. Visão Geral

O `olamip.json` é a representação autoritativa e completa da estrutura e do conteúdo do seu site. O `olamip-delta.json` é um arquivo complementar opcional que descreve as alterações nesse snapshot desde a última atualização.

Juntos, eles formam um fluxo de mudanças interpretável por máquina:

- `olamip.json`: snapshot completo e estruturado.
- `olamip-delta.json`: log de alterações incrementais.

---

## 2. Conceitos Centrais

- **Entry (Entrada)** — um item de conteúdo de nível folha (ex: artigo de blog, página de produto, página de doc).
- **Section / Subsection (Seção / Subseção)** — um agrupamento hierárquico (ex: categoria de blog, coleção de produtos).
- **Operation (Operação)** — uma das três: `added` (adicionado), `updated` (atualizado), `removed` (removido).
- **Delta** — um conjunto de alterações com escopo de data contendo os campos de operação.

---

## 3. Localização e Descoberta do Arquivo

### 3.1 Localização do Arquivo

O arquivo olamip.json deve ser hospedado na raiz do seu domínio:
https://seudominio.com/olamip.json

O arquivo olamip-delta.json deve ser hospedado no mesmo diretório:
https://seudominio.com/olamip-delta.json

Deltas versionados (opcional) residem no mesmo diretório:
/olamip.json
/olamip-delta-AAAA-MM-DD.json

### 3.2 Mecanismo de Descoberta

Webmasters devem declarar a localização do olamip.json usando ambas as tags no <head> do site:

<link rel="olamip" href="https://seudominio.com/olamip.json">
<meta name="olamip-location" content="https://seudominio.com/olamip.json">

---

## 4. Estrutura de Nível Superior

No nível superior, o olamip-delta.json é um objeto JSON com os seguintes campos:

{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-27",
      "added": [
        {
          "title": "Exemplo de entrada",
          "url": "https://exemplo.com/pagina/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://exemplo.com/pagina/",
          "summary": "Resumo atualizado"
        }
      ],
      "removed": [
        {
          "url": "https://exemplo.com/pagina-removida/"
        }
      ]
    }
  ]
}

### 4.1 Campos Obrigatórios

- protocol (string, obrigatório): Deve ser "OLAMIP-DELTA".
- version (string, obrigatório): Deve corresponder ao esquema de versão (ex: "1.0.0").
- window_days (integer, opcional): Número de dias na janela móvel.
- last_updated (string, obrigatório): Data da última modificação (ISO-8601).
- deltas (array, obrigatório): Lista de objetos delta com date, added, updated e removed.

---

## 5. Estrutura do Objeto Delta

Para clareza e consistência, os webmasters DEVEM incluir todos os três campos de operação (added, updated, removed) em cada objeto delta, mesmo que alguns estejam vazios.

---

## 6. Operações

### 6.1 added
Um item em added é um objeto completo. Os campos devem corresponder à especificação principal do formato OLAMIP.

### 6.2 updated
Um item em updated deve incluir a url e pode ser um objeto parcial (apenas campos alterados) ou um objeto completo.

### 6.3 removed
Um item removido é minimalista e contém apenas a identidade:
{ "url": "https://exemplo.com/pagina-para-remover/" }

---

## 7. Identidade do Objeto e URL
O OLAMIP-DELTA usa URLs canônicas como a identidade primária. Alterar uma URL requer um comando de remoção (remove) seguido de uma adição (add), e não uma atualização de URL in-place.

---

## 8. Janela Delta Móvel (Recomendado)
Neste modelo, o arquivo contém as alterações dos últimos window_days (geralmente 7–30 dias). Isso garante que sistemas de IA não percam atualizações caso o rastreamento sofra atrasos.

---

## 9. Deltas Versionados (Alternativa)
Webmasters podem optar por arquivos individuais por data: olamip-delta-AAAA-MM-DD.json. Sistemas de IA devem descobrir e aplicar esses arquivos em ordem cronológica.

---

## 10. Mudanças Estruturais
As mesmas regras de delta se aplicam a seções e subseções. Quando uma seção é removida, todos os seus descendentes (subseções e entradas) são logicamente removidos.

---

## 11. Conformidade

### 11.1 Regras OBRIGATÓRIAS (MUST)
- protocol: "OLAMIP-DELTA".
- Incluir os campos added, updated e removed em todos os objetos delta.
- URLs devem ser absolutas e canônicas.
- Deltas ordenados por data em ordem crescente.

---

## 12. Melhores Práticas
1. Automatize a geração de deltas via CMS ou pipeline de deploy.
2. Mantenha o olamip.json sempre atualizado e autoritativo.
3. Use URLs estáveis para evitar churn desnecessário nos deltas.

---

## 13. Recursos Relacionados
- Especificação de Formato OLAMIP: https://olamip.org/file-format-specification/
- Perguntas Frequentes (FAQ): https://olamip.org/frequently-asked-questions/
- Por que OLAMIP é superior ao LLMs.txt: https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/

---
*Este documento serve como referência para geração e consumo de arquivos olamip-delta.json em produção.*
