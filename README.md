# okinawa-cam-recorder

Robô gravador da box **Okinawa's Cam** do WebApp JK.

Ele grava trechos curtos das câmeras públicas ao vivo de Okinawa, sobe os
vídeos pro **Google Drive** do dono (pasta `OkinawaCams`) e atualiza o
manifesto `recordings.json` — é esse arquivo que o app lê pra montar a
área **🎬 Gravações** da box. Custo zero: minutos gratuitos do GitHub
Actions + espaço gratuito do Drive.

## Agenda

| Quando (JST) | Câmera | Duração |
|---|---|---|
| Todo dia 17:33 | Fusaki Beach (Ishigaki) | 15 min — o pôr do sol |
| Todo dia 13:00 | Baía de Kabira (Ishigaki) | 10 min — o mar do meio-dia |

Também dá pra gravar na hora: **Actions → Gravar câmeras de Okinawa →
Run workflow** (escolhe Fusaki, Kabira ou Kumejima e os minutos).

## Cortes e teto por câmera

- Cada gravação sai em **cortes de 5 minutos** (arquivos separados,
  um cartão por corte no app).
- Cada câmera tem um **teto de ~500 MB** na pasta do Drive: quando ela
  passa do teto, os cortes mais antigos **dela** são apagados sozinhos
  (vai sobrepondo) — as outras câmeras não são tocadas.
- No manifesto ficam as últimas **60** gravações.

## Como funciona

1. O `yt-dlp` resolve a live atual do canal (o endereço troca a cada
   sessão) e o `ffmpeg` grava o trecho sem recodificar (leve).
2. O `rclone` envia o MP4 pra pasta `OkinawaCams` e gera o link público
   de visualização.
3. O manifesto `recordings.json` ganha a gravação nova no topo e é
   commitado de volta neste repositório.

## Segredo necessário

`RCLONE_CONF_B64` (Settings → Secrets and variables → Actions): a
configuração do rclone ligada ao Google Drive do dono, em base64.
Sem esse segredo o robô não acessa o Drive e a gravação não sai.

## Limites honestos

- O YouTube às vezes bloqueia IPs de datacenter ("confirme que você não
  é um robô"): nesses dias a rodada termina sem gravar, com aviso no log.
- A Kumejima só transmite mais ou menos das 6h às 20h.
- Gravações de uso pessoal.
