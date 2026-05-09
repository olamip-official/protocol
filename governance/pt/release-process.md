# Processo de Lançamento

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

Este documento define o processo padrão que eu uso para criar, testar e publicar lançamentos da especificação OLAMIP e ferramentas relacionadas. Por enquanto, todos os lançamentos são gerenciados e de minha responsabilidade.

## 1. Versionamento

Eu sigo [Semantic Versioning 2.0.0](https://semver.org/):

- **MAJOR** quando faço mudanças incompatíveis no formato ou protocolo.
- **MINOR** quando adiciono nova funcionalidade ou seções de forma compatível com versões anteriores.
- **PATCH** quando corrijo bugs, melhoro redações ou esclareço regras sem alterar comportamento.

Exemplo: `1.0.0` → `1.0.1` (patch), `1.1.0` (minor), `2.0.0` (major).

## 2. Modelo de Ramificação

Eu uso um fluxo Git simples:

- `main` — a branch estável, sempre pronta para lançamento.
- Branches de feature ou correção (ex.: `spec/delta-formats`, `fix/tag-rules`) — branches de curta duração para mudanças.
- Branches de lançamento (ex.: `release/v1.1.0`) — criadas imediatamente antes de publicar um lançamento.

## 3. Pré-requisitos para um Lançamento

Antes de iniciar um lançamento, eu garanto:

- Todas as mudanças planejadas para este lançamento foram mescladas em `main`.
- Todas as verificações automatizadas (linting, validação de schema, scripts de build) passam.
- Todas as issues e notas pertencentes a este lançamento foram fechadas ou documentadas.
- `CHANGELOG.md` está atualizado com entradas claras e legíveis para esta versão.
- Documentação está atualizada, incluindo:
  - `file-format-specification.md`
  - `olamip-delta-file-format-specification.md`
  - `docs/introduction.md`
  - `faq.md`
- Arquivos de exemplo `olamip.json` e `olamip-delta.json` ainda validam contra as regras mais recentes.

## 4. Criando um Candidato de Lançamento

1. Criar uma branch de lançamento do `main`:

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. Atualizar números de versão (se usados em ferramentas, scripts ou assets marcados, não apenas texto da spec).
3. Atualizar `CHANGELOG.md` com um cabeçalho para o lançamento e notas para esta versão.
4. Fazer commit das mudanças:

   ```bash
   git add .
   git commit -m "chore(release): preparar v1.1.0"
   ```

5. Fazer push da branch:

   ```bash
   git push origin release/vX.Y.Z
   ```

6. Executar validação local ou CI novamente e garantir que tudo passe.

## 5. Testando o Candidato de Lançamento

Como apenas uma pessoa gerencia lançamentos, meu teste pessoal é o principal controle:

- Valido que os arquivos de exemplo `olamip.json` e `olamip-delta.json` ainda conformam com o schema.
- Verifico que todos os exemplos nos dois arquivos de especificação ainda correspondem às regras descritas.
- Leio as seções atualizadas na spec e docs para capturar inconsistências.

Se encontrar problemas significativos:

- Corrijo-os na branch `release`.
- Atualizo `CHANGELOG.md` e commito novamente.
- Revalido antes de prosseguir.

## 6. Finalizando o Lançamento

Quando o candidato está estável:

1. Marcar o lançamento no Git:

   ```bash
   git tag -a vX.Y.Z -m "Lançamento v1.1.0"
   git push origin vX.Y.Z
   ```

2. Criar um lançamento no GitHub (ou equivalente):

   - Usar a tag `vX.Y.Z`.
   - Colar a seção relevante do `CHANGELOG.md` na descrição do lançamento.
   - Anexar artefatos gerados (ex.: arquivos de schema, ferramentas CLI, tarballs de snapshot) se aplicável.

## 7. Passos Pós-Lançamento

Após o lançamento ser publicado:

- Atualizo notas internas ou roadmap sobre o que foi enviado.
- Movo itens adiados para um backlog ou notas para o próximo lançamento.
- Se houver um canal de divulgação (ex.: Twitter, newsletter, GitHub Discussions), opcionalmente anuncio a nova versão e destaco mudanças.

## 8. Governança e Supervisão

Mesmo com um único mantenedor, mantenho uma camada de governança:

- Qualquer mudança que afete o schema principal, campos obrigatórios ou versão do protocolo deve ser claramente documentada no changelog.
- Mudanças significativas no próprio processo de lançamento (ex.: mudança para um novo esquema de versionamento ou modelo de ramificação) devem ser documentadas em uma breve nota de governança ou registro de decisão em `governance/decision-records/`.

## 9. Rollback ou Patches

Se um lançamento contiver um bug crítico:

- Crio um lançamento de patch o mais rápido possível (ex.: `v1.1.1`).
- Documento claramente a correção no `CHANGELOG.md`.
- Se necessário, atualizo documentação ou canais de comunicação para notar que a versão anterior está depreciada ou desencorajada.

## 10. Automação e Escala Futura

Sempre que possível, automatizo:

- Passos repetitivos como:
  - Verificar validade JSON.
  - Validar arquivos de exemplo contra o schema.
  - Gerar ou lintar texto estilo changelog.
- Mantenho o processo de lançamento simples para que, se contribuidores se juntarem depois, eu possa transferir ou compartilhar os mesmos passos.

Este processo de lançamento é de minha responsabilidade e propriedade e pode ser atualizado via workflow normal sempre que eu melhorar o próprio processo.
