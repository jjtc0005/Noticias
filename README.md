# Asistente de noticias

Cada mañana recoge noticias de feeds RSS, le pide a Claude que elija y resuma las más relevantes según `intereses.txt`, y te las envía al móvil por Telegram (o ntfy).

## Probarlo en local

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
export ANTHROPIC_API_KEY="tu-clave"
python noticias.py --dry-run     # imprime el resumen en consola, sin enviar nada
```

## Recibirlo en el móvil con Telegram

1. En Telegram, habla con **@BotFather**, crea un bot con `/newbot` y guarda el token.
2. Escribe cualquier mensaje a tu bot nuevo.
3. Abre `https://api.telegram.org/bot<TOKEN>/getUpdates` y copia el `chat.id` que aparece.
4. Exporta las variables y ejecuta:

```bash
export TELEGRAM_BOT_TOKEN="..."
export TELEGRAM_CHAT_ID="..."
python noticias.py
```

Alternativa: **ntfy**. Instala la app ntfy, suscríbete a un topic con nombre largo y aleatorio y define `NTFY_TOPIC`. Cualquiera que conozca el nombre del topic puede leerlo.

## Automatizarlo con GitHub Actions

1. Sube esta carpeta a un repositorio (privado, mejor).
2. En *Settings → Secrets and variables → Actions* crea `ANTHROPIC_API_KEY`, `TELEGRAM_BOT_TOKEN` y `TELEGRAM_CHAT_ID`.
3. Lanza el workflow una vez a mano desde la pestaña *Actions* (*Run workflow*) para comprobar que funciona.

## Ajustes (variables de entorno opcionales)

| Variable | Por defecto | Qué hace |
|---|---|---|
| `MODEL` | `claude-sonnet-5-5` | Modelo a usar. Uno más pequeño (p. ej. `claude-haiku-4-5-20251001`) sale más barato |
| `N_NOTICIAS` | `8` | Noticias por resumen |
| `HORAS` | `30` | Antigüedad máxima de las noticias |

Para cambiar fuentes edita `fuentes.yaml`; para cambiar gustos, `intereses.txt`.
