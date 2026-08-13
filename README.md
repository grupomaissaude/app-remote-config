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
| `contrato-status/` | Contrato sem status ativo (`ContractAccessGate`, `ContractStatusBanner`) |
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
  whatsapp-renovacao-por-unidade.txt
contrato-status/
  whatsapp-renovacao.txt
  whatsapp-pagamento.txt
  whatsapp-assinatura.txt
  whatsapp-suporte.txt
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

Rodapé do cartão na home + WhatsApp do card de renovação.

### `cartao-verso-telefone.txt`

Fallback: `0800 661 1307`.

### `cartao-verso-site.txt`

Texto de exibição. Fallback: `www.cartaoms.com.br`.

### `whatsapp-renovacao-por-unidade.txt`

Mapa `fantasia|numero` (E.164, só dígitos) para o botão **Quero renovar** na home.  
O app compara com `contrato.unidade.fantasia` (sem acento, case-insensitive).

Linha especial `__fallback__|…` = número padrão quando a fantasia não bater (ou unidade ausente).

```
__fallback__|555108001239949
TRAMANDAÍ - CARTÃO MAIS SAÚDE|555121601525
OSÓRIO - CARTÃO MAIS SAÚDE|555121601470
CAT - CARTÃO MAIS SAÚDE|555108001239949
CAPÃO DA CANOA - CARTÃO MAIS SAÚDE|555108001239949
UNIDADE DE TESTES DO TI|555108001239949
```

Consumidor: `PlanRenewalUrgencyCard` / `openWhatsAppRenewPlan` em `app-cms-v2`.

## Contrato sem status ativo (`contrato-status/`)

Mensagens do WhatsApp quando o plano não está ativo — o app fica em acesso limitado e o CTA leva o cliente ao atendimento. O número é o mesmo roteamento por unidade de `inicio/whatsapp-renovacao-por-unidade.txt`.

Placeholders (mesma sintaxe dos demais templates):

| Placeholder | Vira |
|---|---|
| `{{status_label}}` | Selo do status (`VENCIDO`, `PENDENTE`, `SUSPENSO`, `INATIVO`, `CANCELADO`, `INDISPONÍVEL`) |
| `{{matricula_bloco}}` | ` Matrícula: 1001.` — vazio quando a API omite a matrícula (todo status fora de `active`) |

O `{{matricula_bloco}}` já inclui o espaço inicial: escreva `...Saúde.{{matricula_bloco}}` sem espaço antes.

### `whatsapp-renovacao.txt`

Status `overdue`/`expired` (CTA **Quero renovar**), também usado pelo card de renovação na home.

Fallback: `Olá! Quero renovar meu plano do Cartão Mais Saúde.{{matricula_bloco}}`

### `whatsapp-pagamento.txt`

Status `billed` (CTA **Falar sobre pagamento**).

Fallback: `Olá! Preciso de ajuda com o pagamento do meu plano do Cartão Mais Saúde ({{status_label}}).{{matricula_bloco}}`

### `whatsapp-assinatura.txt`

Status `awaiting_signature` quando o titular não tem link da Autentique. Dependente não vê CTA de WhatsApp: a tela pede que ele avise o titular.

Fallback: `Olá! Preciso assinar meu contrato do Cartão Mais Saúde ({{status_label}}).{{matricula_bloco}}`

### `whatsapp-suporte.txt`

Demais status (`suspended`, `inactive`, `canceled` e desconhecidos) — CTA **Falar com a unidade**.

Fallback: `Olá! Preciso de ajuda com meu contrato do Cartão Mais Saúde ({{status_label}}).{{matricula_bloco}}`

Consumidores: `openWhatsAppContractStatusSupport` e `openWhatsAppRenewPlan` em `app-cms-v2`.

## Biometria (`biometric/`)

### `background-lock-minutes.txt`

Minutos de inatividade em background para pedir biometria de novo. Fallback: `30`.

## Como atualizar

1. Edite o arquivo na pasta do fluxo/tela correspondente
2. Commit e push em `main`
3. O app sincroniza na próxima abertura

## Remover um domínio

Se o fluxo/tela sair do app, apague a pasta aqui, atualize este README e faça push. Não deixe arquivos órfãos.
