# Diagnóstico Profissão PRF — Checklist de publicação

Quiz-diagnóstico que vende o **ingresso do Profissão PRF (R$ 19,90 · 20-21 jul · 20h)**.
Motor idêntico ao diagnóstico vencedor (`../quiz-engine.html`), com pitch, diagnóstico e compliance reformulados.

## O que já está pronto
- 10 perguntas (mesmas do template vencedor) em 3 fases + barra de progresso
- **Desqualificação**: quem marca "Ensino Médio" recebe tela honesta SEM oferta. Só chega ao pitch quem tem superior completo/tecnólogo/pós/mestrado ou está **cursando** faculdade
- 5 diagnósticos por dor: Sem Tempo · Iniciante Perdido · Síndrome da Espera · Candidato Maduro · Qualificado Sem Método — todos fazendo ponte pro evento
- Medidor de prontidão 86–97% (mesma lógica do vencedor)
- Pitch completo do ingresso: 2 noites, kit R$ 640 ancorado, sorteios (camisa + mentoria), garantia 7 dias
- Compliance: 5.000 vagas SEMPRE como "previsão/solicitado"; salário "acima de R$ 12 mil iniciais, podendo passar de R$ 20 mil na carreira"
- Webhook envia lead com `evento`, `escolaridade`, `resultado`, `score` e `tags` de segmentação (`dor_tempo`, `dor_metodo`, `dor_idade`, `dor_edital`, `dor_medo`, `dor_financeiro`, `formacao_*`, `perfil_*`, `pronto_agora`, `precisa_norte`, `esperando_momento`, `medo_de_comecar`, `resultado_*`)
- Pixel Meta (297518201669282): PageView, Lead, InitiateCheckout
- Checkout **Hubla** — mesmo link das páginas de venda (`pay.hub.la/sPZoheN2QGCFXAsdLG4J`) com `utm_content=quiz01` pra identificar vendas vindas do quiz; UTMs do anúncio são repassadas e o `sck` é remontado da origem

## TODO antes de publicar (constantes no topo do `<script>`)
1. `VTURB_PLAYER_ID` + `VTURB_SCRIPT_SRC` — colar o código do player Vturb do vídeo-ponte (30–45s) quando estiver hospedado
2. `BUTTON_DELAY_SECONDS` — está em 40s (testar 30–45s pro ingresso de R$ 19,90)
3. **Compra-teste na Hubla** e conferir SRC/UTM no relatório antes de rodar tráfego (erro histórico nº 1)

Obs.: a venda NÃO passa pelo n8n — o checkout é direto na Hubla. `LEAD_WEBHOOK_URL` fica vazio (leads rastreados só pelo Pixel); se um dia quiser mandar os leads + tags pra algum CRM, é só apontar a constante.

## Tracking já embutido (não precisa colar nada)
- **Pixel da Meta** (`297518201669282`) já está no `<head>`: PageView no carregamento, Lead no envio do formulário, InitiateCheckout no clique do botão de compra
- **Script de UTMs da Hubla**: a lógica oficial (repassa query params + monta `sck=source|medium|campaign|term|content`) já está embutida na função `appendTracking()`. Ela roda no carregamento E de novo quando o pitch abre — necessário porque o botão de checkout só ganha o link depois do diagnóstico, então o script oficial da Hubla (que roda uma única vez no load) não pegaria o botão. Sem UTMs na página, o link sai com `utm_content=quiz01` pra identificar venda vinda do quiz.

## Publicar no GitHub Pages
```bash
cd diagnostico-profissao-prf
git init -b main && git add . && git commit -m "Diagnóstico Profissão PRF"
gh repo create diagnostico-profissao-prf --public --source=. --remote=origin --push
gh api -X POST /repos/fabiomsam-cloud/diagnostico-profissao-prf/pages -f "source[branch]=main" -f "source[path]=/"
# URL: https://fabiomsam-cloud.github.io/diagnostico-profissao-prf/
```
