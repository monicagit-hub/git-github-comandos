# Guia de Commits - Conventional Commits

Usar uma convenção clara para mensagens de commit melhora a legibilidade, rastreabilidade e colaboração em equipe. Abaixo estão os tipos mais comuns de commits segundo a convenção **Conventional Commits**:

| Tipo      | Descrição                                                                 |
|-----------|---------------------------------------------------------------------------|
| 🎉 `feat`     | Adição de um novo recurso ou funcionalidade ao código.                  |
| 🐛 `fix`      | Correção de bugs.                                                       |
| 🎨 `style`    | Alterações que não afetam a lógica do código (ex: formatação, espaços). |
| 🔨 `refactor` | Refatorações que não corrigem bugs nem adicionam funcionalidades.       |
| ✅ `test`     | Adição ou correção de testes.                                           |
| 📚 `docs`     | Mudanças apenas na documentação.                                        |
| 🔧 `chore`    | Atualizações de ferramentas, dependências ou scripts auxiliares.        |
| ⚡ `perf`     | Melhorias de desempenho.                                                |
| 🏗️ `build`    | Mudanças no sistema de build ou nas dependências.                      |
| 🔁 `ci`       | Alterações em scripts de integração contínua.                           |

---

## Exemplo de mensagens de commit

- `feat: adicionar botão de login`
- `fix: corrigir bug no cálculo de desconto`
- `style: remover espaços em branco`
- `refactor: simplificar função de validação`
- `test: adicionar testes para componente Header`
- `docs: atualizar README com instruções de instalação`
- `chore: atualizar versão do ESLint`
- `perf: otimizar carregamento da página inicial`
- `build: configurar Webpack para produção`
- `ci: ajustar workflow do GitHub Actions`

---

🧠 **Dica:** Use sempre o formato `tipo: descrição`, com verbos no infinitivo, curtos e objetivos.