# app-remote-config

Configurações remotas do app Mais Saúde, servidas via GitHub Raw.

O app busca estes arquivos em tempo de execução (com cache local). Alterações neste repositório entram em vigor sem precisar publicar nova versão do app.

**URL base (padrão):** `https://raw.githubusercontent.com/grupomaissaude/app-remote-config/main`

## Estrutura

```
biometric/
  background-lock-minutes.txt     — Minutos de inatividade para exigir biometria novamente
promocao-selos/
  termos.txt                      — Termos / política da promoção de selos
  whatsapp-retirada-template.txt  — Mensagem padrão do WhatsApp para retirada de brinde
```

## Biometria

### `biometric/background-lock-minutes.txt`

Número inteiro positivo: minutos com o app em background/inativo após os quais a biometria é solicitada de novo ao voltar.

Também alimenta o texto na tela **Plano**:
`Solicitar biometria após X minutos de inatividade.`

Fallback embutido no app: `30`.

Exemplo:

```
30
```

## Promoção de selos

### `promocao-selos/termos.txt`

Texto exibido na tela de aceite da promoção. Arquivo em texto puro (UTF-8).

### `promocao-selos/whatsapp-retirada-template.txt`

Template da mensagem enviada ao abrir o WhatsApp na retirada do brinde.

Placeholders disponíveis:

| Placeholder | Descrição |
|-------------|-----------|
| `{{brinde}}` | Nome do brinde resgatado |
| `{{unidade}}` | Nome da unidade de retirada |
| `{{endereco_bloco}}` | Endereço formatado (com `\n` na frente), ou vazio |
| `{{codigo_bloco}}` | Linha `Código do voucher: …`, ou vazio |

Exemplo:

```
Olá, quero marcar um horário para retirar meu brinde.
```

Placeholders opcionais (se quiser incluir contexto na mensagem):

| Placeholder | Descrição |
|-------------|-----------|
| `{{brinde}}` | Nome do brinde resgatado |
| `{{unidade}}` | Nome da unidade de retirada |
| `{{endereco_bloco}}` | Endereço formatado (com `\n` na frente), ou vazio |
| `{{codigo_bloco}}` | Linha `Código do voucher: …`, ou vazio |

## Como atualizar

1. Edite o arquivo desejado neste repositório
2. Faça commit e push para `main`
3. O app sincroniza na próxima abertura (ou ao atualizar a tela que consome a variável)
