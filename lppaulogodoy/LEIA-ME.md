# LP Dr. Paulo Godoy — "O homem por inteiro"

Página única, autocontida (`index.html`), mobile-first, sem dependências além do Google Fonts e do GTM já existente.

---

## ⚠ Antes de publicar — pendências que NÃO podem ser inventadas

| Item | Onde está | O que fazer |
|---|---|---|
| **RQE** | 1ª dobra (`.hero-legal`) e rodapé | Substituir `RQE [CONFIRMAR]` pelo número real. Só anunciar Andrologia se houver RQE efetivo para ela. |
| **CRM** | idem | Está `CRM-SP 207232` (herdado do site antigo). Confirmar antes de subir. |
| **Coordenadas** | JSON-LD `geo` | `-23.4966 / -46.8560` é aproximação de Alphaville. Puxar do Perfil da Empresa no Google. |
| **CEP** | JSON-LD `postalCode` | Confirmar o CEP exato do Medic Life. |
| **Avaliações** | seção `#avaliacoes` | Os 3 depoimentos são **provisórios**. Trocar pelos textos reais do Google. Sem antes/depois e sem promessa de resultado (CFM 2.336/2023). |
| **Domínio** | `canonical`, `og:url`, JSON-LD | Trocar `paulogodoy.com.br` pelo domínio final. |
| **Políticas** | rodapé e formulário | Criar `/politica-de-privacidade` e `/politica-de-cookies`. Hoje os links apontam para páginas que ainda não existem. |
| **Concierge** | seção Alphaville | Está "Júlia". No briefing aparecem Júlia e Juliana — confirmar o nome. |

---

## Assets a substituir

```
img/logo.png        logo real
img/foto-1.jpg      poster do hero (fallback do vídeo)
img/foto-2.jpg      retrato / thumb do vídeo de 45s
img/foto-3.jpg      foto de fechamento (sem jaleco, olhando para a câmera)
img/local-1..3.jpg  recepção, consultório, chegada
img/og-paulo-godoy.jpg  imagem de compartilhamento (1200×630)
video/hero.mp4      6–10s, mudo, loop, cortes lentos — comprimir bem (< 2 MB)
video/apresentacao.mp4  45s, Dr. Paulo falando
```

As imagens entregues são **placeholders gerados** só para o layout não quebrar. Todas as tags têm `onerror` — se o arquivo faltar, o bloco degrada sem quebrar a página.

---

## Configuração rápida

No topo do `<script>` final:

```js
var CFG = {
  wa: '5511964750178',   // WhatsApp da equipe
  endpointCaixa: '',     // URL do CRM/backend p/ a Caixa Preta
  endpointLead: ''       // URL do CRM/backend p/ leads do teste
};
```

Com os endpoints vazios, a Caixa Preta e o teste **só disparam evento no dataLayer** — nada é armazenado. Para guardar as perguntas (e cumprir LGPD com segurança), apontar para um endpoint próprio com HTTPS, acesso restrito e retenção definida.

---

## SEO local (Alphaville / Barueri)

- `<title>` e H2 com "Alphaville" e "Barueri"; H1 mantém o conceito da marca.
- JSON-LD: `Physician` + `MedicalBusiness` com endereço, geo, horário, `areaServed` (Alphaville, Barueri, Santana de Parnaíba, Osasco, Cotia, São Paulo) e `availableService`.
- `FAQPage` marcado — as 10 perguntas do accordion podem gerar rich result.
- `geo.region` / `geo.placename` / ICBM.
- Canonical, OG e Twitter Card.

**Fora da página, o que realmente move o ranking local:** Perfil da Empresa no Google 100% preenchido no endereço de Alphaville, categoria primária "Urologista", fotos reais, posts semanais e fluxo constante de avaliações. A LP sozinha não rankeia o mapa.

---

## Rastreamento (dataLayer → GTM)

| Evento | Quando |
|---|---|
| `lp_carregada` | load, com `entrada`, `fonte`, `campanha` |
| `selecionou_intencao` | clique num dos 8 cards |
| `abriu_territorio` | abertura do mapa da saúde |
| `abriu_faq` | abertura de uma pergunta |
| `abriu_contato` | abertura do menu de contato, com `origem` |
| `clique_whatsapp` | **conversão principal** — `intencao_contato`, `origem`, `fonte`, `campanha` |
| `caixa_preta_envio` | envio da Caixa Preta |
| `quiz_iniciado` / `quiz_concluido` | teste de atenção |
| `scroll_profundidade` | 25 / 50 / 75 / 90% |
| `consentimento_cookies` | aceite ou "só essenciais" |
| `play_video`, `click_como_chegar`, `click_google_reviews` | cliques auxiliares |

No GTM: criar Meta Pixel + GA4 + conversão do Google Ads sobre `clique_whatsapp`.

### Entradas por origem

A página lê `utm_source`, `utm_campaign`, `utm_medium`, `utm_content` **e** o último segmento da URL. Isso cobre os dois formatos do briefing:

- `/alphaville`, `/instagram`, `/caixapreta`, `/flyer`, `/google`, `/sago`, `/indicacao` (mesma página, servida no path)
- `?utm_source=instagram&utm_campaign=caixapreta`

A origem vai **dentro da mensagem do WhatsApp**. A equipe recebe:

```
Assunto: Agendar consulta presencial em Alphaville
O que me trouxe: Quero ser pai.
Origem: seletor_intencao · instagram
Campanha: caixapreta
```

---

## Decisões de conteúdo (e por quê)

- **Sem pop-up de saída.** O briefing pede o contrário do site antigo: a conversão vem depois da experiência. A barra fixa no mobile aparece só depois de 70% da primeira tela.
- **Sem escassez fabricada.** A barra de "73% das vagas" do site antigo saiu — é o tipo de alegação que não se sustenta.
- **Sem promessa de resultado.** Nenhum texto garante energia, libido ou performance. O quiz devolve orientação, não diagnóstico.
- **Caixa Preta com aviso de urgência.** Canal não atende emergência e diz isso explicitamente.
- **Consentimento LGPD** no formulário e banner de cookies com opção "só essenciais".

---

## Acessibilidade

- Contraste AA verificado em todos os pares de texto (mínimo aferido: 4,8:1).
- Foco visível, skip link, `aria-expanded`/`aria-pressed`, `aria-live` nas áreas dinâmicas, focus trap no menu de contato, ESC fecha.
- Alvos de toque ≥ 52px. Inputs com `font-size:16px` (evita zoom no iOS).
- `prefers-reduced-motion` desliga as animações.
- Sem overflow horizontal em 360, 390, 430 e 1440px.

---

## O que ficou para a fase 2

- IA de atendimento ("Pergunte à equipe Paulo Godoy") com transferência humana — precisa de backend.
- Integração com agenda e CRM.
- Mapa da saúde como ilustração interativa (hoje é accordion — mais rápido e mais acessível).
- Heatmap/session replay: configurar com mascaramento total dos campos, já que a Caixa Preta trafega dado sensível.
