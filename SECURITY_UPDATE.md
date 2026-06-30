# Security Update — Atualização de Dependências Vulneráveis

**Data:** 2026-06-30
**Branch:** `claude/security-dependency-updates-rr6cdr`
**Node.js do ambiente:** v22.22.2 · **npm:** 10.9.7

Este relatório documenta a correção das dependências sinalizadas pelo GitHub
Security, atualizando-as para versões sem vulnerabilidades conhecidas.

## Escopo

O repositório possui dois manifests:

| Local | Papel |
| --- | --- |
| `package.json` (raiz) | Declara `picomatch`, `rollup`, `vite` como dependências diretas |
| `ecomerce-platform/package.json` | Aplicação React/Vite (deps diretas + transitivas) |

A maioria dos pacotes-alvo (`postcss`, `esbuild`, `@babel/core`, `rollup`,
`picomatch`) são **dependências transitivas** trazidas por `vite` e
`@vitejs/plugin-react`. Por isso a fixação das versões seguras foi feita via
`overrides` no `package.json`, além do bump direto de `vite`.

## Pacotes corrigidos

> Observação: o alerta listava `piconcatch`, que não é um pacote publicado no
> npm. Trata-se de erro de digitação de **`picomatch`** — o pacote realmente
> presente no projeto — e foi tratado como tal.

### Antes → Depois (versões resolvidas no lock)

| Pacote | Faixa vulnerável | Versão alvo | Raiz (antes → depois) | `ecomerce-platform` (antes → depois) |
| --- | --- | --- | --- | --- |
| `rollup` | `>=4.0.0 <4.59.0` | `>=4.59.0` | 4.62.2 → 4.62.2 ✓ | 4.53.3 → **4.62.2** |
| `picomatch` | `>=4.0.0 <4.0.4` | `>=4.0.4` | 4.0.4 → 4.0.4 ✓ | 4.0.3 → **4.0.4** |
| `vite` | `>=7.1.0 <=7.3.1` | `>=7.3.2` | 7.3.5 → **7.3.6** | 7.2.4 → **7.3.6** |
| `postcss` | `<8.5.10` | `>=8.5.10` | 8.5.15 → 8.5.15 ✓ | 8.5.6 → **8.5.16** |
| `@babel/core` | `<=7.29.0` | `>=7.29.6` | n/a | 7.28.5 → **7.29.7** |
| `esbuild` | `>=0.27.3 <0.28.1` | `>=0.28.1` | 0.27.7 → **0.28.1** | 0.25.12 → **0.28.1** |

Todas as versões resultantes **atendem ou superam** a versão alvo exigida.

## Alterações nos manifests

### `package.json` (raiz)
- `vite`: `^7.3.5` → `^7.3.6` (faixa que aceita `esbuild ^0.28.0`).
- Novo bloco `overrides` fixando `esbuild ^0.28.1`, `rollup ^4.62.2`,
  `picomatch ^4.0.4`, `postcss ^8.5.10`.

### `ecomerce-platform/package.json`
- `vite` (devDep): `^7.2.4` → `^7.3.6`.
- `@vitejs/plugin-react`: `^4.3.0` → `^4.7.0` (versão cujo peer aceita Vite 7;
  já era a resolvida na prática).
- `overrides` ampliado:
  - `postcss`: `^8.4.31` → `^8.5.10`
  - adicionados: `@babel/core ^7.29.6`, `esbuild ^0.28.1`, `rollup ^4.59.0`,
    `picomatch ^4.0.4`, `vite ^7.3.6`
  - mantidos: `json5 ^2.2.3`, `@babel/traverse ^7.23.2`

## Validação

| Verificação | Comando | Resultado |
| --- | --- | --- |
| Lock files atualizados | `npm install` (raiz e subpasta) | ✅ `package-lock.json` atualizado em ambos |
| Auditoria de segurança (raiz) | `npm audit` | ✅ **found 0 vulnerabilities** |
| Auditoria de segurança (app) | `npm audit` | ✅ **found 0 vulnerabilities** |
| Compatibilidade / build | `npm run build` (ecomerce-platform) | ✅ `vite v7.3.6` — *83 modules transformed, built in 1.49s* |

> **Testes:** o projeto não define script `test` (`npm test` → *Missing script:
> "test"*). A validação de compatibilidade foi feita pelo build de produção do
> Vite, que compila e empacota toda a aplicação com sucesso.

## Breaking changes

Nenhuma breaking change relevante foi identificada:

- **Vite 7.3.5 → 7.3.6** e demais bumps são releases **patch/minor** dentro da
  mesma major, voltadas a correções; sem alterações de API que afetem este
  projeto.
- **esbuild 0.28.x** está explicitamente declarado como compatível pelo Vite
  7.3.6 (`esbuild: "^0.27.0 || ^0.28.0"`).
- **@babel/core 7.29.x** permanece na major 7, compatível com
  `@vitejs/plugin-react`.
- O build de produção foi executado sem erros nem avisos de incompatibilidade.

## Resumo

- ✅ Todas as dependências do alerta atualizadas para versões seguras.
- ✅ `package-lock.json` regenerado nos dois manifests.
- ✅ `npm audit` reporta **0 vulnerabilidades**.
- ✅ Build da aplicação validado sem breaking changes.
