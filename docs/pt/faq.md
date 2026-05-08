# Frequently Asked Questions

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## O que é OLAMIP?

OLAMIP significa **Open Language‑Aligned Machine‑Interpretable Protocol**. É um formato aberto e simples que permite que sites publiquem resumos estruturados do seu conteúdo para que os modelos de linguagem grandes (LLMs) possam entendê-lo e aprender com ele de forma mais eficaz.

## Por que devo usar OLAMIP?

Porque os LLMs já rastreiam o seu site, mas têm dificuldade em interpretá-lo. Com o OLAMIP, você pode:

- Ensinar à IA exatamente sobre o que são suas páginas.
- Destacar seu conteúdo mais valioso.
- Melhorar a descobribilidade em ferramentas e buscas com IA.
- Reduzir interpretações incorretas e alucinações.

É a diferença entre a IA adivinhar e a IA entender.

## O OLAMIP expõe algo privado?

Não. O OLAMIP apenas resume conteúdo que já é público no seu site. Ele não revela nada além do que já está visível em seu HTML, metadados ou marcação SEO.

## Concorrentes podem usar meu arquivo OLAMIP?

Não mais do que já usam suas páginas públicas, metadados ou sitemap. Na prática, o OLAMIP dá a você mais controle sobre como os sistemas de IA interpretam seu conteúdo, e não menos.

## Como é um arquivo `olamip.json`?

Um arquivo `olamip.json` é um documento estruturado em formato JSON que descreve o conteúdo, a hierarquia e os metadados do seu site de uma forma que os sistemas de IA podem interpretar facilmente. Se quiser explorar a estrutura técnica completa, você pode visitar a página [File Format Specification](https://olamip.org/file-format-specification) ou ver um exemplo real revisando o caso de uso do [TimeLAX.com](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/).

## Onde devo hospedar o arquivo?

Na raiz do seu domínio: `https://yourdomain.com/olamip.json`

Você também pode adicionar tags de descoberta à sua página inicial:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## O que o campo “priority” faz?

Ele sinaliza o quão importante uma página é para os LLMs. Use-o com moderação e intenção:

- `high` → conteúdo central e crítico.
- `medium` → padrão para a maioria das páginas.
- `low` → conteúdo de nicho, desatualizado ou de baixo valor.

Se tudo for marcado como `high`, nada será.

## Por que preciso incluir URLs se os resumos já são fornecidos?

Os resumos dizem à IA o que uma página significa, mas as URLs dizem à IA o que uma página é. Um resumo é descritivo, mas não é um identificador único. A URL é a única referência estável e canônica para a página real no seu site.

Os sistemas de IA usam URLs para:

- Buscar e verificar o conteúdo completo.
- Evitar indexar duplicatas.
- Detectar quando páginas mudam de lugar ou são alteradas.
- Conectar dados OLAMIP com marcação schema.org, sitemaps e crawlers.
- Citar ou referenciar a página ao gerar respostas.

Sem URLs, duas páginas diferentes com resumos parecidos seriam indistinguíveis. Incluir a URL garante que o OLAMIP permaneça ancorado na estrutura real do seu site e que os sistemas de IA possam interpretar e recuperar seu conteúdo com confiabilidade.

## O que o campo “policy” faz, e eu preciso defini-lo?

O campo `policy` controla se os sistemas de IA têm permissão para ingerir uma seção, subseção ou entrada. Os valores válidos são `"allow"` e `"forbid"`. Se você omitir o campo, o OLAMIP herda automaticamente a policy do ancestral mais próximo. Se nenhum ancestral definir uma policy, o padrão será `"allow"`.

A maioria dos sites pode simplesmente omitir esse campo por completo. Use `"forbid"` apenas quando quiser excluir explicitamente certas páginas ou seções da ingestão por IA.

## Qual é a diferença entre OLAMIP e sitemaps tradicionais?

Enquanto um sitemap é um índice de navegação, o OLAMIP é um mapa de conhecimento interpretável por máquina.

### Sitemap XML

- Lista URLs.
- Fornece dicas de crawl.
- Ajuda mecanismos de busca a descobrir páginas.
- Não tem significado semântico.
- Não tem hierarquia de conteúdo além da estrutura de URLs.

### OLAMIP

- Define tipos de conteúdo (`page`, `project`, `doc_page`, etc.).
- Define tipos de seção (`project_group`, `doc_category`, etc.).
- Fornece descrições canônicas.
- Fornece resumos otimizados para LLMs.
- Fornece tags e metadados.
- Estabelece hierarquia explícita.
- Oferece uma representação estruturada de todo o site.

Em outras palavras:

- XML Sitemap = “Aqui estão minhas páginas.”
- OLAMIP = “Aqui está o que meu site significa.”

É por isso que os sistemas de IA podem usar OLAMIP de forma muito mais eficaz do que um sitemap tradicional. Ele não é apenas uma lista de URLs; é um mapa de conteúdo legível por máquina que elimina ambiguidades e ajuda a IA a interpretar o site como um humano faria.

## Um arquivo OLAMIP com mais de 3.000 URLs é grande demais?

De jeito nenhum. Um arquivo com 3.000 entradas está bem dentro de limites seguros. Mesmo com resumos longos, o tamanho total ficaria em torno de 6 MB, o que é trivial para servidores, navegadores e sistemas de IA modernos.

## Qual é o tamanho típico de uma entrada OLAMIP?

| Summary Length | Approx. Size per Entry |
|---|---|
| Short | 300–600 bytes |
| Long | 1–2 KB |

## O que torna o OLAMIP escalável apesar de tamanhos grandes de arquivo?

- Estrutura hierárquica: sections → subsections → entries reduzem repetição.
- Resumos concisos: normalmente abaixo de 500 caracteres.
- Compressão eficiente: Gzip ou Brotli podem reduzir o tamanho do arquivo em 70–90%.

## Até que tamanho um arquivo OLAMIP pode crescer antes de causar problemas de desempenho?

| File Size | Approx. URLs | Impact |
|---|---:|---|
| 1–10 MB | 500–5,000 | Perfeitamente ok. Normal. |
| 10–25 MB | 5,000–12,000 | Ainda ok. Um pouco mais lento para carregar. |
| 25–50 MB | 12,000–25,000 | Grande, mas administrável. |
| 50–100 MB | 25,000–50,000 | Pesado. Alguns sistemas podem ficar lentos. |
| 100+ MB | 50,000+ | Grande demais para muitos pipelines de LLM. |

Para a maioria dos webmasters, arquivos OLAMIP com dezenas de milhares de entradas ainda permanecem bem dentro de limites seguros e escaláveis.

## Com que frequência devo atualizá-lo?

Sempre que publicar novo conteúdo ou revisar páginas existentes. Trate-o como um sitemap, mas para IA.

## Preciso atualizar o arquivo principal se eu usar `olamip-delta.json`?

Sim. O `olamip.json` principal deve sempre refletir o estado atual do seu site. O arquivo opcional `olamip-delta.json` contém apenas alterações recentes, como novas páginas, atualizações ou remoções. Os sistemas de IA usam o arquivo completo como snapshot autoritativo e aplicam os deltas para se manterem atualizados entre atualizações completas.

## Preciso adicionar uma tag `<link>` para `olamip-delta.json` no meu HTML?

Não. Apenas o arquivo principal `olamip.json` precisa ser referenciado na seção `<head>` do seu site. Essa referência serve como mecanismo de descoberta que informa aos sistemas de IA onde estão os seus metadados OLAMIP. O arquivo delta é diferente; ele é um fluxo opcional de atualização suplementar que os sistemas de IA procuram automaticamente assim que sabem onde está o seu arquivo principal. Desde que `olamip-delta.json` esteja no mesmo diretório que `olamip.json`, os sistemas de IA o detectarão e usarão sem exigir tags `<link>` adicionais.

## Os LLMs realmente vão ler isso?

Sim, esse é o objetivo. À medida que a adoção crescer, os LLMs priorizarão fontes estruturadas como OLAMIP para treinamento e recuperação. Os primeiros adotantes moldarão a forma como a IA entende a web.

## Como posso testar se um bot de IA consegue rastrear meu arquivo `olamip.json`?

Tente isto para OpenAI, Anthropic e Perplexity, respectivamente:

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
