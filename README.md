# app-remote-config

Configurações remotas do app Mais Saúde, servidas via GitHub Raw.

O app busca estes arquivos em tempo de execução (com cache local). Alterações neste repositório entram em vigor sem precisar publicar nova versão do app.

**URL base:** `https://raw.githubusercontent.com/grupomaissaude/app-remote-config/main`  
(Configurada no app via `EXPO_PUBLIC_REMOTE_CONFIG_URL`. Sem essa variável, o app usa só fallbacks embutidos.)

## Convenção de pastas

Pastas espelham o **componente** ou a **tela** do app (`app-cms-v2`), em kebab-case:

| Pasta remota | Espelho no app |
|--------------|----------------|
| `biometric/` | Biometria (`SessionProvider`) |
| `unit-picker-modal/` | `src/components/UnitPickerModal.tsx` |
| `whatsapp-contract-plan/` | `src/utils/openWhatsAppContractPlan.ts` |
| `auth/` | `app/(auth)/index.tsx` |
| `inicio/` | `app/(tabs)/index.tsx` (`InicioScreen`) |

## Estrutura

```
biometric/
  background-lock-minutes.txt
unit-picker-modal/
  unidades.txt
whatsapp-contract-plan/
  numero.txt
  mensagem-template.txt
  telefone-voz.txt
auth/
  cobertura-mensagem.txt
inicio/
  cartao-central-telefone.txt
  cartao-site-url.txt
```

## Biometria (`biometric/`)

### `background-lock-minutes.txt`

Número inteiro positivo: minutos com o app em background/inativo após os quais a biometria é solicitada de novo ao voltar.

Fallback embutido no app: `30`.

## UnitPickerModal (`unit-picker-modal/`)

### `unidades.txt`

Uma opção por linha: `id|label`. Linhas vazias são ignoradas.

O id **`outros`** é reservado: não abre WhatsApp; mostra `auth/cobertura-mensagem.txt`.  
Se a lista remota não tiver `outros`, o app acrescenta o fallback embutido.

Fallback: Tramandaí, Osório, Capão da Canoa, Outros.

```
tramandai|Tramandaí - RS
osorio|Osório - RS
capao|Capão da Canoa - RS
outros|Outros
```

## WhatsApp contratar plano (`whatsapp-contract-plan/`)

### `numero.txt`

E.164 **somente dígitos**. Fallback: `555108001239919`.

### `mensagem-template.txt`

Placeholder `{{unidade_bloco}}` → ` - unidade {label}` ou vazio.

Fallback: `Olá! Gostaria de contratar um plano do Cartão Mais Saúde{{unidade_bloco}}.`

### `telefone-voz.txt`

Texto no alerta se o WhatsApp não abrir. Fallback: `0800 123 9919`.

## Auth (`auth/`)

### `cobertura-mensagem.txt`

Diálogo ao escolher a opção `outros` no modal. Fallback: texto de cobertura no RS.

## Início (`inicio/`)

Rodapé do cartão na home.

### `cartao-central-telefone.txt`

Fallback: `0800 661 1307`.

### `cartao-site-url.txt`

Texto de exibição (sem obrigar `https://`). Fallback: `www.cartaoms.com.br`.

## Como atualizar

1. Edite o arquivo na pasta do componente/tela correspondente
2. Commit e push em `main`
3. O app sincroniza na próxima abertura

## Remover um domínio

Se a feature/tela sair do app, apague a pasta aqui, atualize este README e faça push. Não deixe arquivos órfãos.
