
# Modelos Cisco SIP – CNF/XML (79xx & 78xx)

Este repositório contém **templates genéricos** para provisionamento de telefones Cisco via SIP, baseados em exemplos reais porém **completamente sanitizados** (sem IPs/hosts, usuários, senhas ou ramais reais).

## Estrutura
```
.
├─ TEMPLATE_78xx.cnf.xml
├─ TEMPLATE_79xx.cnf.xml
├─ examples/                  # (opcional) coloque aqui amostras fictícias
├─ README.md
└─ .gitignore
```

### Transporte SIP (IMPORTANTE para 7911G)
- `{{TRANSPORT_LAYER_PROTOCOL}}`
  - `1` = TCP  ← **Use isto para Cisco 7911G**
  - `2` = UDP
  - `3` = TLS

O modelo **Cisco 7911G só registrou no Asterisk usando TCP**.  
Portanto, ajuste `transport=tcp` no Asterisk e configure `transportLayerProtocol` como `1`.

## Como usar
1. Copie o modelo correspondente ao seu aparelho (78xx ou 79xx) para um novo arquivo nomeado como `SEP<MAC>.cnf.xml`.
2. Substitua todos os **placeholders** `{{...}}` pelos valores do seu ambiente:
   - **SIP / Registrar**: `{{SIP_REGISTRAR_OR_IP}}`, `{{SIP_PORT}}`, `{{SIPS_PORT}}` (se TLS for usado)
   - **Credenciais**: `{{SIP_USERNAME}}`, `{{SIP_AUTH_USER}}`, `{{SIP_PASSWORD}}`
   - **Apresentação**: `{{DISPLAY_NAME}}`, `{{LINE_LABEL}}`, `{{SIP_CONTACT}}`, `{{VOICEMAIL_NUMBER}}`
   - **Infra**: `{{NTP_SERVER}}`, `{{TIME_ZONE}}`
   - **Arquivos/Firmware**: `{{FIRMWARE_LOAD}}`, `{{DIAL_TEMPLATE_FILE}}`, `{{SOFTKEY_FILE}}`, `{{RINGLIST_FILE}}`, `{{TONE_SCHEME_FILE}}`
   - **Transporte**: `{{TRANSPORT_LAYER_PROTOCOL}}` (`1`=TCP, `2`=UDP, `3`=TLS – ver compatibilidade do firmware)
3. Publique o arquivo final no seu servidor **TFTP/HTTP** e reinicie o telefone.

> **Dica:** para múltiplas linhas no mesmo aparelho, duplique o bloco `<line button="1">…</line>` e ajuste o atributo `button="2"`, `button="3"`, etc., com as credenciais/labels de cada linha.

## Segurança
- **Nunca** faça commit de credenciais reais ou IPs internos. 
- Use segredos fora do repositório (ex.: cofre de segredos) e mantenha apenas os `TEMPLATE_*.xml` versionados.
- Este repo inclui um `.gitignore` que ignora automaticamente `SEP*.cnf.xml`.

## Compatibilidade e notas
- `TEMPLATE_79xx.cnf.xml`: voltado a terminais da série **79xx (term11)**.
- `TEMPLATE_78xx.cnf.xml`: voltado à série **78xx**; inclui chaves opcionais como `softKeyFile`, `ringList`, `toneScheme` e `securedSipPort`.
- Parâmetros como `transportLayerProtocol`, portas SIP e timers podem variar conforme **firmware** do telefone e **servidor SIP**.

## Licença
Distribuído como **modelo** sem garantias. Use por sua conta e risco.
