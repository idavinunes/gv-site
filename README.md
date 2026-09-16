# Site — GV Consultoria

Site institucional da **GV Consultoria** (crédito consignado — INSS e servidor federal),
empresa irmã da **MMS Consignados** e da **MHM Solution**.

Criado a partir de `idavinunes/mhm-site`, que por sua vez veio do site da MMS.
Mesma estrutura: `src/` é a **fonte de verdade**, `build.py` gera o `index.html`,
e o `Dockerfile` roda o build numa etapa própria — então **o deploy sempre sai do `src/`**.

```bash
python3 build.py           # regera o index.html
python3 build.py --check   # falha se o index.html estiver defasado
```

Detalhes de cada arquivo em [`src/README.md`](src/README.md).

---

## ⚠️ ANTES DE PUBLICAR — pendências bloqueantes

### 1. Contatos ainda são placeholder (`src/props.json`)

| Campo | Valor atual | Situação |
|---|---|---|
| `email` | `contato@gvconsignados.com.br` | ✅ domínio correto |
| `tel0800` | `0800 000 0000` | ❌ **placeholder** — a GV tem 0800 real |
| `phone` | `(00) 00000-0000` | ❌ **placeholder** |
| `whatsapp` | `5500000000000` | ❌ **placeholder** |

> 🔴 **O site da MHM foi ao ar com esses mesmos placeholders e o e-mail no domínio errado
> — e segue assim desde agosto/2026.** Não repetir: trocar os três antes de divulgar.
> O WhatsApp aparece em ~11 pontos da página; todos saem do `props.json`.

### 2. Marca ainda é a da MHM
- O logo no header/rodapé é o **PNG dourado da MHM** (`src/assets/logo-mhm.png`).
- A paleta inteira é a da MHM (dourado `#F4C958`/`#8A6A14` sobre preto quente `#0E0A08`).
- O favicon e o apple-touch-icon **já são o monograma GV**, mas com as cores provisórias da MHM.
- 🔴 Quando o logo da GV chegar: extrair a paleta dos pixels dele, recolorir e
  **rodar a auditoria de contraste** (na MHM a primeira passada deu **15 reprovações WCAG AA**).

### 3. DNS não foi apontado (decisão: fica pra depois)
`gvconsignados.com.br` está na **HostGator** (`ns688`/`ns689`), apex em `108.179.193.3`
servindo **403** — não há site a perder.

> 🔴 **O domínio tem e-mail ativo.** Ao publicar, mexer **somente no `A` do apex e no `www`**.
> **Não tocar** em `MX`, nem nos `A` de `mail`/`webmail`/`cpanel`/`ftp`, nem no SPF —
> foi exatamente esse o cuidado tomado na MHM.

---

## Conteúdo herdado a revisar com o cliente
Textos que vieram da MMS/MHM e podem não descrever a GV:

- **Tagline** "Sua vida financeira mais leve" — é da MMS, herdada pela MHM.
- **Bancos parceiros** — a esteira de logos (BB, Caixa, Itaú, Bradesco, Santander) veio da MMS.
- **Disclaimer do rodapé** — diz que a GV atua como correspondente/assessoria e não
  empresta diretamente. Confirmar que descreve a GV corretamente.
- **As 3 estatísticas do topo** (84x, 35%, R$ 0) são **fatos do produto**, não da empresa —
  valem para qualquer correspondente. Foi assim que a MHM resolveu não usar números da MMS.

> ℹ️ **Prova social da MMS já não está aqui:** depoimentos com nome de cliente, selo
> RA1000 do Reclame Aqui e estatísticas de atendimento foram removidos ainda na MHM.
> Prova social é de quem conquistou — se a GV tiver a dela, entra; caso contrário, não volta.

## Simulador
Usa Price com taxa fixa por convênio (`rate()`/`pmt()` em `src/component.js`).
Gaps conhecidos, herdados das irmãs: sem tabela de coeficientes por convênio × prazo,
sem margem consignável (35%), sem IOF, sem carência, sem CET, e oferece 96x inclusive
para INSS (cujo teto é 84).
