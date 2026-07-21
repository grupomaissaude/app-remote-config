# app-remote-config

Configurações remotas do app Mais Saúde, servidas via GitHub Raw.

O app busca estes arquivos em tempo de execução (com cache local). Alterações neste repositório entram em vigor sem precisar publicar nova versão do app.

**URL base:** `https://raw.githubusercontent.com/grupomaissaude/app-remote-config/main`  
(Configurada no app via `EXPO_PUBLIC_REMOTE_CONFIG_URL`. Sem essa variável, o app usa só fallbacks embutidos.)

## Estrutura

```
biometric/
  background-lock-minutes.txt     — Minutos de inatividade para exigir biometria novamente
contato/
  whatsapp-numero.txt             — Número WhatsApp (E.164, só dígitos) para contratar plano
  whatsapp-mensagem-template.txt  — Template da mensagem pré-preenchida no WhatsApp
  telefone-voz.txt                — Telefone de voz no alerta se o WhatsApp falhar
  unidades.txt                    — Itens do modal (id|label); id `outros` = opção genérica
  cobertura-mensagem.txt          — Texto do diálogo ao escolher a opção `outros`
  central-telefone.txt            — Telefone da central no verso do cartão
  site-url.txt                    — Site exibido no verso do cartão
```

## Biometria

### `biometric/background-lock-minutes.txt`

Número inteiro positivo: minutos com o app em background/inativo após os quais a biometria é solicitada de novo ao voltar.

Fallback embutido no app: `30`.

Exemplo:

```
30
```

## Contato / contratar plano

Consumidores no app: `openWhatsAppContractPlan`, `UnitPickerModal`, tela de auth, verso do cartão na home.

### `contato/whatsapp-numero.txt`

Número em E.164 **somente dígitos** (sem `+` ou espaços), usado em `wa.me` / `whatsapp://`.

Fallback embutido: `555108001239919`.

### `contato/whatsapp-mensagem-template.txt`

Texto UTF-8. Placeholder:

| Placeholder | Descrição |
|-------------|-----------|
| `{{unidade_bloco}}` | Se houver unidade: ` - unidade {label}`; senão string vazia |

Fallback embutido: `Olá! Gostaria de contratar um plano do Cartão Mais Saúde{{unidade_bloco}}.`

### `contato/telefone-voz.txt`

Texto exibido no alerta quando o WhatsApp não abre (ex.: `0800 123 9919`).

Fallback embutido: `0800 123 9919`.

### `contato/unidades.txt`

Uma opção por linha no formato `id|label`. Linhas vazias são ignoradas.

O id **`outros`** é reservado: não abre WhatsApp; mostra `cobertura-mensagem.txt`.

Fallback embutido: Tramandaí, Osório, Capão da Canoa, Outros.

Exemplo:

```
tramandai|Tramandaí - RS
osorio|Osório - RS
capao|Capão da Canoa - RS
outros|Outros
```

### `contato/cobertura-mensagem.txt`

Mensagem do diálogo ao escolher a opção com id `outros` no modal.

Fallback embutido: texto atual de cobertura no RS.

### `contato/central-telefone.txt`

Telefone da central no rodapé do cartão na home.

Fallback embutido: `0800 661 1307`.

### `contato/site-url.txt`

Site no rodapé do cartão (texto de exibição, sem obrigar `https://`).

Fallback embutido: `www.cartaoms.com.br`.

## Como atualizar

1. Edite o arquivo desejado neste repositório
2. Faça commit e push para `main`
3. O app sincroniza na próxima abertura (ou ao atualizar a tela que consome a variável)

## Remover um domínio

Se uma feature for removida do app, apague a pasta correspondente aqui, atualize este README e faça push em `main`. Não deixe arquivos órfãos.
