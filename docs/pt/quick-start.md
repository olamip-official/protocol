# Início Rápido OLAMIP

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Este início rápido mostra como implementar o OLAMIP em um site da forma mais simples possível.

## 1. Crie `olamip.json`

Coloque um arquivo `olamip.json` válido na raiz do seu site:

`https://yourdomain.com/olamip.json`

Comece com um snapshot pequeno e limpo das suas páginas mais importantes.

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "Um site sobre tecnologia e atualizações de produtos.",
    "tags": ["tecnologia", "blog"]
  },
  "content": {
    "overview": {
      "summary": "Um blog de tecnologia cobrindo notícias de produtos, tutoriais e artigos de opinião."
    },
    "sections": [
      {
        "title": "Blog",
        "summary": "Artigos e guias.",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "Bem-vindo ao Blog",
            "summary": "Uma introdução ao conteúdo e propósito do site.",
            "url": "https://yourdomain.com/blog/welcome/",
            "content_type": "blog_article",
            "tags": ["intro", "blog"],
            "priority": "high",
            "language": "pt"
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-04-06",
    "language": "pt",
    "source_url": "https://yourdomain.com/",
    "copyright": "© 2026 Example Site"
  }
}
```

## 2. Adicione tags de descoberta ao HTML

Adicione estas tags à seção `<head>` da sua homepage e páginas principais:

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. Mantenha os resumos concisos

Escreva resumos curtos e factuais que expliquem sobre o que é a página e por que ela importa. Evite linguagem de marketing, texto duplicado e descrições vagas.

Bom:

- "Guia para iniciantes em fotografia de longa exposição."
- "Detalhes do produto e preços do plano empresarial."

Evite:

- "A melhor página da internet."
- "Conteúdo incrível que você vai adorar."

## 4. Use os tipos de conteúdo corretos

Escolha o `content_type` mais específico possível:

- `page`
- `landing_page`
- `legal_page`
- `blog_article`
- `news_article`
- `product`
- `service`
- `doc_page`
- `research_paper`
- `dataset`
- `project`
- `media_item`
- `resource`

## 5. Organize com seções

Use `sections` para grupos de conteúdo e `entries` para páginas individuais. Se necessário, adicione `subsections` para criar uma estrutura mais profunda.

Exemplo:

- Seção: Blog
- Subseção: Tutoriais
- Entrada: Como Usar OLAMIP

## 6. Defina a prioridade intencionalmente

Use:

- `high` para suas páginas mais importantes.
- `medium` para páginas normais.
- `low` para páginas de nicho ou antigas.

Não marque tudo como `high`.

## 7. Use policy para controlar a ingestão da IA

O campo `policy` informa aos sistemas de IA se eles podem ingerir uma seção, subseção ou entrada.

- Use `"allow"` para permitir explicitamente a ingestão.
- Use `"forbid"` para dizer aos sistemas de IA para evitarem uma seção, subseção ou entrada específica.

Se você omitir o campo `policy`, o OLAMIP herda a política do ancestral mais próximo. Se nenhum ancestral definir uma política, a política efetiva é `"allow"`. Para a maioria dos sites, você pode deixar `policy` de fora e usar apenas `"forbid"` onde deseja que os sistemas de IA pulem o conteúdo.

Exemplos:

- Pular uma seção inteira:

  ```json
  {
    "title": "Guias Descontinuados",
    "summary": "Conteúdo desatualizado, não para IA.",
    "url": "https://yourdomain.com/old-guides/",
    "section_type": "doc_category",
    "policy": "forbid"
  }
  ```

- Pular uma única página:

  ```json
  {
    "title": "Página de Teste Interna",
    "summary": "Página usada apenas para staging.",
    "url": "https://yourdomain.com/test/",
    "content_type": "page",
    "policy": "forbid"
  }
  ```

## 8. Adicione metadados de idioma

Use códigos BCP-47 como:

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

Defina o idioma no nível do arquivo, seção ou entrada conforme necessário.

## 9. Atualize o arquivo regularmente

Sempre que adicionar, alterar ou remover páginas importantes, atualize o `olamip.json`.

Se o seu site muda frequentemente, também mantenha `olamip-delta.json` no mesmo diretório para que os sistemas de IA possam se manter atualizados entre atualizações completas.

## 10. Valide antes de publicar

Antes de entrar em produção, certifique-se de que:

- O JSON é válido.
- As URLs são absolutas.
- Os campos obrigatórios estão presentes.
- Os resumos são claros e dentro do comprimento recomendado.
- As tags estão normalizadas e consistentes.
- O arquivo é servido do caminho raiz.

## Lista de verificação mínima de implementação

- [ ] Criar `olamip.json`.
- [ ] Hospedá-lo na raiz do site.
- [ ] Adicionar tags de descoberta ao `<head>` do HTML.
- [ ] Incluir seções e entradas para conteúdo principal.
- [ ] Adicionar metadados e campos de idioma.
- [ ] Manter resumos curtos e precisos.
- [ ] Atualizá-lo sempre que o site mudar.
- [ ] Adicionar `olamip-delta.json` se quiser atualizações incrementais.

## Ponto de partida sugerido

Se você não souber por onde começar, implemente o OLAMIP primeiro apenas para suas páginas principais:

- Homepage.
- Índice do blog.
- Melhores artigos.
- Páginas principais de produtos ou serviços.
- Páginas importantes de documentação.

Depois expanda a partir daí.
