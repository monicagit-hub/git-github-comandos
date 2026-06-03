# Guia de Commits - Conventional Commits

Usar uma convenção clara para mensagens de commit melhora a legibilidade, rastreabilidade e colaboração em equipe. Este guia é baseado na especificação **[Conventional Commits v1.0.0](https://www.conventionalcommits.org/pt-br/v1.0.0/)**.

---

## Estrutura de um commit

```
<tipo>[escopo opcional][! opcional]: <descrição curta>

[corpo opcional]

[rodapé(s) opcional(is)]
```

- **tipo** — categoria da mudança (obrigatório)
- **escopo** — área do código afetada, entre parênteses (opcional)
- **!** — indica breaking change (opcional)
- **descrição** — resumo curto no imperativo (obrigatório)
- **corpo** — explicação mais detalhada, separado por linha em branco (opcional)
- **rodapé** — metadados como referências e breaking changes (opcional)

---

## Tipos de commit

| Tipo          | Descrição                                                                 |
|---------------|---------------------------------------------------------------------------|
| 🎉 `feat`     | Adição de um novo recurso ou funcionalidade.                              |
| 🐛 `fix`      | Correção de bug.                                                          |
| 📚 `docs`     | Mudanças apenas na documentação.                                          |
| 🎨 `style`    | Alterações que não afetam a lógica do código (formatação, espaços, ponto e vírgula). |
| 🔨 `refactor` | Refatoração que não corrige bug nem adiciona funcionalidade.              |
| ✅ `test`     | Adição ou correção de testes.                                             |
| 🔧 `chore`    | Atualizações de ferramentas, dependências ou scripts auxiliares.          |
| ⚡ `perf`     | Melhoria de desempenho.                                                   |
| 🏗️ `build`   | Mudanças no sistema de build ou nas dependências externas.                |
| 🔁 `ci`       | Alterações em arquivos de configuração de integração contínua.            |
| ↩️ `revert`   | Reversão de um commit anterior.                                           |

---

## Escopo (scope)

O escopo indica qual parte do projeto foi afetada. É opcional, mas recomendado em projetos maiores.

```
feat(auth): adicionar login com Google
fix(api): corrigir timeout na requisição de usuários
docs(readme): atualizar instruções de instalação
refactor(cart): simplificar cálculo de frete
```

---

## Breaking Changes

Mudanças que quebram compatibilidade com versões anteriores devem ser sinalizadas de duas formas:

**Com `!` após o tipo/escopo:**
```
feat!: remover suporte ao endpoint legado /v1/users
feat(api)!: renomear campo userId para user_id
```

**Com footer `BREAKING CHANGE:`:**
```
feat(api): migrar para autenticação OAuth 2.0

BREAKING CHANGE: o fluxo de autenticação foi completamente reescrito.
Tokens JWT anteriores deixam de ser aceitos.
```

> Ambas as formas podem ser combinadas.

---

## Corpo e Rodapé

Use o **corpo** para explicar o *porquê* da mudança, não o *o quê* (o código já mostra isso).

Use o **rodapé** para metadados como referências a issues, breaking changes ou co-autores.

```
fix(checkout): corrigir cálculo de desconto cumulativo

Descontos aplicados em sequência estavam sendo somados em vez de
compostos, resultando em valores incorretos para múltiplos cupons.

Refs: #87
Reviewed-by: João Silva
```

---

## Exemplos completos

```bash
# Simples
feat: adicionar botão de login
fix: corrigir bug no cálculo de desconto
docs: atualizar README com instruções de instalação
style: remover espaços em branco desnecessários
refactor: simplificar função de validação de CPF
test: adicionar testes para componente Header
chore: atualizar versão do ESLint para 9.x
perf: otimizar carregamento da página inicial
build: configurar Webpack para produção
ci: ajustar workflow do GitHub Actions
revert: feat: adicionar botão de login experimental

# Com escopo
feat(auth): adicionar autenticação com biometria
fix(cart): corrigir arredondamento no total do pedido
perf(images): implementar lazy loading nas imagens do catálogo

# Breaking change
feat(api)!: remover endpoints depreciados da v1

# Com corpo e rodapé
fix(payments): corrigir falha no processamento de reembolso

O cálculo do reembolso parcial não considerava itens com
desconto aplicado, gerando valores negativos em alguns casos.

Refs: #210
Co-authored-by: Maria Costa <maria@empresa.com>
```

---

## Boas práticas

- Use verbos no **imperativo**: `adicionar`, `corrigir`, `remover`, `atualizar`
- Limite a descrição a **72 caracteres**
- Não use ponto final na descrição
- Separe o corpo e o rodapé da descrição com **uma linha em branco**
- Prefira commits pequenos e focados em uma única mudança
- Escreva em **português** ou **inglês** — escolha um padrão e mantenha

---

> Referência oficial: [conventionalcommits.org](https://www.conventionalcommits.org/pt-br/v1.0.0/)
