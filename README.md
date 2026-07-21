# app-remote-config

Configurações remotas do app Mais Saúde, servidas via GitHub Raw.

O app busca estes arquivos em tempo de execução (com cache local). Alterações neste repositório entram em vigor sem precisar publicar nova versão do app.

**URL base:** `https://raw.githubusercontent.com/grupomaissaude/app-remote-config/main`  
(Configurada no app via `EXPO_PUBLIC_REMOTE_CONFIG_URL`. Sem essa variável, o app usa só fallbacks embutidos.)

## Estrutura

```
biometric/
  background-lock-minutes.txt     — Minutos de inatividade para exigir biometria novamente
```

## Biometria

### `biometric/background-lock-minutes.txt`

Número inteiro positivo: minutos com o app em background/inativo após os quais a biometria é solicitada de novo ao voltar.

Fallback embutido no app: `30`.

Exemplo:

```
30
```

## Como atualizar

1. Edite o arquivo desejado neste repositório
2. Faça commit e push para `main`
3. O app sincroniza na próxima abertura (ou ao atualizar a tela que consome a variável)

## Remover um domínio

Se uma feature for removida do app, apague a pasta correspondente aqui, atualize este README e faça push em `main`. Não deixe arquivos órfãos.
