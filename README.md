# app-remote-config

Configurações remotas do app Mais Saúde, servidas via GitHub Raw.

O app busca estes arquivos em tempo de execução (com cache local). Alterações neste repositório entram em vigor sem precisar publicar nova versão do app.

**URL base:** `https://raw.githubusercontent.com/grupomaissaude/app-remote-config/main`  
(Configurada no app via `EXPO_PUBLIC_REMOTE_CONFIG_URL`. Sem essa variável, o app usa só fallbacks embutidos.)

## Convenção de pastas

Pastas por **fluxo / tela** do app (`app-cms-v2`), em kebab-case:

| Pasta remota | Espelho no app |
|--------------|----------------|
| `contratar-plano/` | Fluxo “Faça já o seu” / “Contratar um Plano” (`UnitPickerModal`, WhatsApp, diálogo Outros) |
| `inicio/` | `app/(tabs)/index.tsx` (`InicioScreen`) — rodapé do cartão |
| `biometric/` | Biometria (`SessionProvider`) |

## Estrutura

```
contratar-plano/
  unidades.txt
  whatsapp-numero.txt
  whatsapp-mensagem-template.txt
  telefone-voz.txt
  cobertura-mensagem.txt
inicio/
  cartao-verso-telefone.txt
  cartao-verso-site.txt
biometric/
  background-lock-minutes.txt
```

## Contratar plano (`contratar-plano/`)

Fluxo na tela de auth: escolher unidade → WhatsApp **ou** diálogo “Outros”.

### `unidades.txt`

Uma opção por linha: `id|label`. O id **`outros`** não abre WhatsApp; mostra `cobertura-mensagem.txt`.  
Se a lista remota não tiver `outros`, o app acrescenta o fallback embutido.

```
tramandai|Tramandaí - RS
osorio|Osório - RS
capao|Capão da Canoa - RS
outros|Outros
```

### `whatsapp-numero.txt`

E.164 **somente dígitos**. Fallback: `555108001239919`.

### `whatsapp-mensagem-template.txt`

Placeholder `{{unidade_bloco}}` → ` - unidade {label}` ou vazio.

Fallback: `Olá! Gostaria de contratar um plano do Cartão Mais Saúde{{unidade_bloco}}.`

### `telefone-voz.txt`

Texto no alerta se o WhatsApp não abrir. Fallback: `0800 123 9919`.

### `cobertura-mensagem.txt`

Só ao tocar em **Outros**. Fallback: texto de cobertura no RS.

## Início (`inicio/`)

Rodapé do cartão na home.

### `cartao-verso-telefone.txt`

Fallback: `0800 661 1307`.

### `cartao-verso-site.txt`

Texto de exibição. Fallback: `www.cartaoms.com.br`.

## Biometria (`biometric/`)

### `background-lock-minutes.txt`

Minutos de inatividade em background para pedir biometria de novo. Fallback: `30`.

## Como atualizar

1. Edite o arquivo na pasta do fluxo/tela correspondente
2. Commit e push em `main`
3. O app sincroniza na próxima abertura

## Remover um domínio

Se o fluxo/tela sair do app, apague a pasta aqui, atualize este README e faça push. Não deixe arquivos órfãos.
