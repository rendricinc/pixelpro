## 1. Visão Geral

O Pixel Pro pode enviar dados em tempo real sobre leads e eventos para URLs externas configuradas por você (Webhooks). Isso permite integrar o Pixel Pro com outras ferramentas, CRMs, planilhas ou sistemas personalizados.

Quando ativados e configurados corretamente nas Configurações Avançadas do Pixel Pro, os webhooks serão enviados periodicamente (geralmente a cada hora, via Action Scheduler) ou podem ser disparados manualmente.

## 2. Informações Gerais

Todos os webhooks enviados pelo Pixel Pro compartilham as seguintes características:

*   **Método HTTP:** `POST`
*   **Content-Type:** `application/json`
*   **User-Agent:** Segue o padrão do WordPress (ex: `WordPress/6.4.3; https://seudominio.com.br`)
*   **URL de Destino:** Definida por você na seção "Webhooks" das Configurações Avançadas do Pixel Pro.

## 3. Tipos de Webhooks

O Pixel Pro envia dois tipos principais de webhooks: `lead` e `event`. O tipo é identificado pelo campo `type` no corpo da requisição JSON.

### 3.1 Webhook de Lead (`type: "lead"`)

Este webhook é enviado com informações sobre os leads capturados ou atualizados recentemente pelo Pixel Pro.

**Estrutura do Payload:**

O corpo da requisição contém um objeto JSON com a seguinte estrutura:

```json
{
  "type": "lead",
  "data": [
    {
      // Objeto Lead 1
    },
    {
      // Objeto Lead 2 (se houver mais de um no lote)
    }
    // ...
  ]
}
```

O campo `data` é um **array** contendo um ou mais objetos, cada um representando um lead.

**Campos do Objeto Lead:**

| Campo                    | Tipo   | Descrição                                                                                                 | Exemplo (Contexto Brasileiro)       |
| :----------------------- | :----- | :-------------------------------------------------------------------------------------------------------- | :---------------------------------- |
| `lead_uid`               | string | Identificador Único Universal (UUID) gerado pelo Pixel Pro para o lead.                                     | `a1b2c3d4-e5f6-4a7b-8c9d-0e1f2a3b4c5d` |
| `lead_id`                | number | ID do Post (do tipo Lead) no WordPress.                                                                   | `123`                               |
| `name`                   | string | Nome completo do lead.                                                                                    | `João da Silva`                     |
| `email`                  | string | Endereço de e-mail do lead.                                                                               | `joao.silva@example.com.br`         |
| `phone`                  | string | Número de telefone do lead (apenas dígitos).                                                              | `5511998765432`                     |
| `document`               | string | Documento (CPF/CNPJ) do lead.                                                                             | `11122233344`                       |
| `ip`                     | string | Endereço IP associado ao lead no momento da captura/atualização.                                          | `177.10.20.30`                      |
| `device`                 | string | String User-Agent do navegador/dispositivo.                                                               | `Mozilla/5.0 (Linux; Android 13; SM-G991B) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Mobile Safari/537.36` |
| `adress_street`          | string | Rua do endereço do lead.                                                                                  | `Avenida Paulista`                  |
| `adress_street_number`   | string | Número do endereço.                                                                                       | `1500`                              |
| `adress_complement`      | string | Complemento do endereço.                                                                                  | `Apto 52 Bloco C`                   |
| `adress_city`            | string | Cidade do lead (baseado na geolocalização do IP ou em dados capturados em formulários ou integrações de checkout).                                                         | `Sao Paulo`                         |
| `adress_state`           | string | Estado/Região do lead (baseado na geolocalização do IP ou em dados capturados em formulários ou integrações de checkout).                                                  | `Sao Paulo`                         |
| `adress_zipcode`         | string | Código Postal (CEP) do lead (baseado na geolocalização do IP ou em dados capturados em formulários ou integrações de checkout).                                              | `01310-200`                         |
| `adress_country_name`    | string | Nome do país do lead (baseado na geolocalização do IP ou em dados capturados em formulários ou integrações de checkout).                                                   | `Brazil`                            |
| `adress_country`         | string | Código do país (ISO 3166-1 alpha-2).                                                                      | `BR`                                |
| `first_fbc`              | string | Valor do cookie `_fbc` capturado no *primeiro* acesso do lead.                                              | `fb.1.1698330000123.ABCdef123ghiJKL` |
| `first_utm_source`       | string | Parâmetro `utm_source` do *primeiro* acesso.                                                              | `google`                            |
| `first_utm_medium`       | string | Parâmetro `utm_medium` do *primeiro* acesso.                                                              | `cpc`                               |
| `first_utm_campaign`     | string | Parâmetro `utm_campaign` do *primeiro* acesso.                                                            | `verao_2024_sp`                     |
| `first_utm_id`           | string | Parâmetro `utm_id` do *primeiro* acesso.                                                                  | `CMP54321`                          |
| `first_utm_content`      | string | Parâmetro `utm_content` do *primeiro* acesso.                                                             | `anuncio_texto_praia`               |
| `first_utm_term`         | string | Parâmetro `utm_term` do *primeiro* acesso.                                                                | `viagem_nordeste`                   |
| `first_src`              | string | Parâmetro `src` do *primeiro* acesso.                                                                     | `afiliado_viagem`                   |
| `first_sck`              | string | Parâmetro `sck` do *primeiro* acesso.                                                                     | `SCK7788`                           |
| `fbp`                    | string | Valor *atual* do cookie `_fbp` associado ao lead.                                                         | `fb.1.1698330005678.9876543210`      |
| `fbc`                    | string | Valor *atual* do cookie `_fbc` associado ao lead.                                                         | `fb.1.1698330000123.ABCdef123ghiJKL` |
| `src`                    | string | Valor *atual* do parâmetro `src`.                                                                         | `afiliado_viagem`                   |
| `sck`                    | string | Valor *atual* do parâmetro `sck`.                                                                         | `SCK7788`                           |
| `utm_source`             | string | Valor *atual* do parâmetro `utm_source`.                                                                  | `google`                            |
| `utm_medium`             | string | Valor *atual* do parâmetro `utm_medium`.                                                                  | `cpc`                               |
| `utm_campaign`           | string | Valor *atual* do parâmetro `utm_campaign`.                                                                | `verao_2024_sp`                     |
| `utm_id`                 | string | Valor *atual* do parâmetro `utm_id`.                                                                      | `CMP54321`                          |
| `utm_content`            | string | Valor *atual* do parâmetro `utm_content`.                                                                 | `anuncio_texto_praia`               |
| `utm_term`               | string | Valor *atual* do parâmetro `utm_term`.                                                                    | `viagem_nordeste`                   |
| `first_lead`             | string | Timestamp do primeiro evento de Lead registrado.                                                          | `1698330010`                        |
| `first_purchase`         | string | Timestamp do primeiro evento de Compra registrado.                                                        | `1698330500`                        |
| `first_refund`           | string | Timestamp do primeiro evento de Reembolso registrado.                                                     | `1698331000`                        |
| `last_lead`              | string | Timestamp do último evento de Lead registrado.                                                            | `1698330015`                        |
| `last_purchase`          | string | Timestamp do último evento de Compra registrado.                                                          | `1698330505`                        |
| `last_refund`            | string | Timestamp do último evento de Reembolso registrado.                                                       | `1698331005`                        |
| `purchase_count`         | number | Número total de compras registradas para este lead.                                                       | `1`                                 |
| `purchase_amount`        | number | Valor total acumulado das compras registradas para este lead.                                               | `299.90`                            |

**Exemplo de Payload (Lead Brasileiro):**

```json
{
  "type": "lead",
  "data": [
    {
      "lead_uid": "a1b2c3d4-e5f6-4a7b-8c9d-0e1f2a3b4c5d",
      "lead_id": 123,
      "name": "João da Silva",
      "email": "joao.silva@example.com.br",
      "phone": "5511998765432",
      "document": "11122233344",
      "ip": "177.10.20.30",
      "device": "Mozilla/5.0 (Linux; Android 13; SM-G991B) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Mobile Safari/537.36",
      "adress_street": "Avenida Paulista",
      "adress_street_number": "1500",
      "adress_complement": "Apto 52 Bloco C",
      "adress_city": "Sao Paulo",
      "adress_state": "Sao Paulo",
      "adress_zipcode": "01310-200",
      "adress_country_name": "Brazil",
      "adress_country": "BR",
      "first_fbc": "fb.1.1698330000123.ABCdef123ghiJKL",
      "first_utm_source": "google",
      "first_utm_medium": "cpc",
      "first_utm_campaign": "verao_2024_sp",
      "first_utm_id": "CMP54321",
      "first_utm_content": "anuncio_texto_praia",
      "first_utm_term": "viagem_nordeste",
      "first_src": "afiliado_viagem",
      "first_sck": "SCK7788",
      "fbp": "fb.1.1698330005678.9876543210",
      "fbc": "fb.1.1698330000123.ABCdef123ghiJKL",
      "src": "afiliado_viagem",
      "sck": "SCK7788",
      "utm_source": "google",
      "utm_medium": "cpc",
      "utm_campaign": "verao_2024_sp",
      "utm_id": "CMP54321",
      "utm_content": "anuncio_texto_praia",
      "utm_term": "viagem_nordeste",
      "first_lead": "1698330010",
      "first_purchase": "1698330500",
      "first_refund": "1698331000",
      "last_lead": "1698330015",
      "last_purchase": "1698330505",
      "last_refund": "1698331005",
      "purchase_count": 1,
      "purchase_amount": 299.90
    }
    // ... outros leads no lote, se houver
  ]
}
```

### 3.2 Webhook de Evento (`type: "event"`)

Este webhook é enviado com informações detalhadas sobre os eventos rastreados pelo Pixel Pro (como PageView, ViewContent, Purchase, etc.).

**Estrutura do Payload:**

O corpo da requisição contém um objeto JSON com a seguinte estrutura:

```json
{
  "type": "event",
  "data": [
    {
      // Objeto Evento 1
    },
    {
      // Objeto Evento 2
    },
    // ... (Pode conter múltiplos eventos em um único envio)
  ]
}
```

O campo `data` é um **array** contendo um ou mais objetos, cada um representando um evento rastreado. Os eventos podem ser enviados em lotes.

**Campos do Objeto Evento:**

| Campo                | Tipo   | Descrição                                                                                                 | Exemplo (Contexto Brasileiro)       |
| :------------------- | :----- | :-------------------------------------------------------------------------------------------------------- | :---------------------------------- |
| `event_id`           | number | ID do Post (do tipo Evento) no WordPress.                                                                 | `456`                               |
| `event_uid`          | string | Identificador Único Universal (UUID) gerado pelo Pixel Pro para o evento.                                   | `b2c3d4e5-f6a7-4b8c-9d0e-1f2a3b4c5d6e` |
| `event_date`         | string | Data e hora do evento no fuso horário configurado no WordPress (formato `Y-m-d H:i:s`).                    | `2023-11-10 10:15:30`               |
| `event_date_gmt`     | string | Data e hora do evento em GMT (formato `Y-m-d H:i:s`).                                                      | `2023-11-10 13:15:30`               |
| `lead_id`            | string | ID do Post do Lead associado a este evento no WordPress.                                                  | `123`                               |
| `geo_ip`             | string | Endereço IP de onde o evento foi originado.                                                               | `189.50.60.70`                      |
| `geo_device`         | string | String User-Agent do navegador/dispositivo do evento.                                                     | `Mozilla/5.0 (iPhone; CPU iPhone OS 17_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.1 Mobile/15E148 Safari/604.1` |
| `geo_country_name`   | string | Nome do país (baseado na geolocalização do IP).                                                           | `Brazil`                            |
| `geo_country`        | string | Código do país (ISO 3166-1 alpha-2).                                                                      | `BR`                                |
| `geo_state`          | string | Estado/Região (baseado na geolocalização do IP).                                                          | `Rio de Janeiro`                    |
| `geo_city`           | string | Cidade (baseada na geolocalização do IP).                                                                 | `Rio de Janeiro`                    |
| `geo_currency`       | string | Código da moeda (ISO 4217) associado ao evento (padrão BRL).                                              | `BRL`                               |
| `geo_zipcode`        | string | Código Postal (CEP) (baseado na geolocalização do IP).                                                    | `20040-001`                         |
| `page_id`            | string | ID da página no WordPress onde o evento ocorreu. Pode ser `"REMOTE"` para eventos de fontes externas.       | `789`                               |
| `page_title`         | string | Título da página onde o evento ocorreu.                                                                   | `Página do Produto Fantástico`      |
| `content_name`       | string | Nome do conteúdo associado ao evento (ex: para ViewContent).                                                | `Detalhes Camiseta Modelo XPTO`     |
| `product_name`       | string | Nome do produto associado ao evento.                                                                        | `Camiseta Modelo XPTO`              |
| `product_id`         | string | ID do produto associado ao evento.                                                                          | `CAM-XPTO-AZ-M`                     |
| `offer_ids`          | string | IDs das ofertas associadas ao evento (separados por vírgula).                                               | `OFERTA_LANCAMENTO,FRETE_GRATIS`    |
| `product_value`      | number | Valor do produto/oferta associado ao evento.                                                                | `89.90`                             |
| `predicted_ltv`      | number | Lifetime Value (LTV) previsto associado ao evento.                                                          | `150.00`                            |
| `event_name`         | string | Nome do evento (padrão do Facebook como `PageView`, `ViewContent`, `Purchase`, ou nome customizado).         | `ViewContent`                       |
| `event_day`          | string | Dia da semana em que o evento ocorreu (em inglês).                                                        | `Friday`                            |
| `event_day_in_month` | string | Dia do mês (numérico) em que o evento ocorreu.                                                            | `10`                                |
| `event_month`        | string | Mês em que o evento ocorreu (em inglês).                                                                  | `November`                          |
| `event_time`         | string | Timestamp Unix (em segundos) do momento do evento.                                                        | `1699618530`                        |
| `event_time_interval`| string | Intervalo de hora em que o evento ocorreu (formato `HH-(HH+1)` ex: `10-11`).                               | `10-11`                             |
| `event_url`          | string | URL completa onde o evento ocorreu.                                                                       | `https://seudominio.com.br/produto/camiseta-xpto` |
| `traffic_source`     | string | Fonte de tráfego detectada (ex: `google`, `facebook`).                                                    | `facebook`                          |
| `utm_source`         | string | Parâmetro `utm_source` associado ao evento.                                                               | `facebook`                          |
| `utm_medium`         | string | Parâmetro `utm_medium` associado ao evento.                                                               | `cpc_feed`                          |
| `utm_campaign`       | string | Parâmetro `utm_campaign` associado ao evento.                                                             | `black_friday_roupas`               |
| `utm_id`             | string | Parâmetro `utm_id` associado ao evento.                                                                   | `ADSETBF456`                        |
| `utm_term`           | string | Parâmetro `utm_term` associado ao evento.                                                                 | `camiseta_algodao_masculina`        |
| `utm_content`        | string | Parâmetro `utm_content` associado ao evento.                                                              | `imagem_produto_azul`               |
| `src`                | string | Parâmetro `src` associado ao evento.                                                                      | `blog_post_review`                  |
| `sck`                | string | Parâmetro `sck` associado ao evento.                                                                      | `SCKBF123`                          |
| `fbc`                | string | Valor do cookie `_fbc` no momento do evento.                                                              | `fb.1.1699618500555.MnoPqr789stuVWX` |
| `fbp`                | string | Valor do cookie `_fbp` no momento do evento.                                                              | `fb.1.1699618500111.0123456789`      |
| `fb_server_response` | string | **String JSON** contendo a resposta da API de Conversões do Facebook para este evento específico.           | `{"1122334455667788":{"events_received":1,"messages":[],"fbtrace_id":"DEFghiJKLmnoPQRstuVWXyz"}}` |

**Exemplo de Payload (Evento Brasileiro):**

```json
{
  "type": "event",
  "data": [
    {
      "event_id": 456,
      "event_uid": "b2c3d4e5-f6a7-4b8c-9d0e-1f2a3b4c5d6e",
      "event_date": "2023-11-10 10:15:30",
      "event_date_gmt": "2023-11-10 13:15:30",
      "lead_id": "123",
      "geo_ip": "189.50.60.70",
      "geo_device": "Mozilla/5.0 (iPhone; CPU iPhone OS 17_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.1 Mobile/15E148 Safari/604.1",
      "geo_country_name": "Brazil",
      "geo_country": "BR",
      "geo_state": "Rio de Janeiro",
      "geo_city": "Rio de Janeiro",
      "geo_currency": "BRL",
      "geo_zipcode": "20040-001",
      "page_id": "789",
      "page_title": "Página do Produto Fantástico",
      "content_name": "Detalhes Camiseta Modelo XPTO",
      "product_name": "Camiseta Modelo XPTO",
      "product_id": "CAM-XPTO-AZ-M",
      "offer_ids": "OFERTA_LANCAMENTO,FRETE_GRATIS",
      "product_value": 89.90,
      "predicted_ltv": 150.00,
      "event_name": "ViewContent",
      "event_day": "Friday",
      "event_day_in_month": "10",
      "event_month": "November",
      "event_time": "1699618530",
      "event_time_interval": "10-11",
      "event_url": "https://seudominio.com.br/produto/camiseta-xpto",
      "traffic_source": "facebook",
      "utm_source": "facebook",
      "utm_medium": "cpc_feed",
      "utm_campaign": "black_friday_roupas",
      "utm_id": "ADSETBF456",
      "utm_term": "camiseta_algodao_masculina",
      "utm_content": "imagem_produto_azul",
      "src": "blog_post_review",
      "sck": "SCKBF123",
      "fbc": "fb.1.1699618500555.MnoPqr789stuVWX",
      "fbp": "fb.1.1699618500111.0123456789",
      "fb_server_response": "{\"1122334455667788\":{\"events_received\":1,\"messages\":[],\"fbtrace_id\":\"DEFghiJKLmnoPQRstuVWXyz\"}}"
    },
    {
      "event_id": 457,
      "event_uid": "c3d4e5f6-a7b8-4c9d-0e1f-2a3b4c5d6e7f",
      "event_date": "2023-11-10 10:14:55",
      "event_date_gmt": "2023-11-10 13:14:55",
      "lead_id": "123",
      "geo_ip": "189.50.60.70",
      "geo_device": "Mozilla/5.0 (iPhone; CPU iPhone OS 17_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.1 Mobile/15E148 Safari/604.1",
      "geo_country_name": "Brazil",
      "geo_country": "BR",
      "geo_state": "Rio de Janeiro",
      "geo_city": "Rio de Janeiro",
      "geo_currency": "BRL",
      "geo_zipcode": "20040-001",
      "page_id": "789",
      "page_title": "Página do Produto Fantástico",
      "content_name": "Visita Geral",
      "product_name": "Camiseta Modelo XPTO",
      "product_id": "CAM-XPTO-AZ-M",
      "offer_ids": "OFERTA_LANCAMENTO",
      "product_value": 89.90,
      "predicted_ltv": 150.00,
      "event_name": "PageView",
      "event_day": "Friday",
      "event_day_in_month": "10",
      "event_month": "November",
      "event_time": "1699618495",
      "event_time_interval": "10-11",
      "event_url": "https://seudominio.com.br/produto/camiseta-xpto",
      "traffic_source": "facebook",
      "utm_source": "facebook",
      "utm_medium": "cpc_feed",
      "utm_campaign": "black_friday_roupas",
      "utm_id": "ADSETBF456",
      "utm_term": "camiseta_algodao_masculina",
      "utm_content": "imagem_produto_azul",
      "src": "blog_post_review",
      "sck": "SCKBF123",
      "fbc": "fb.1.1699618500555.MnoPqr789stuVWX",
      "fbp": "fb.1.1699618500111.0123456789",
      "fb_server_response": "{\"1122334455667788\":{\"events_received\":1,\"messages\":[],\"fbtrace_id\":\"GHIjklMNOpqrSTUvwxYZabc\"}}"
    }
  ]
}
```

## 4. Considerações Importantes

*   **Configuração:** Certifique-se de que a URL do seu webhook esteja corretamente configurada na seção "Webhooks" das Configurações Avançadas do Pixel Pro. Você pode configurar múltiplos webhooks, e cada um receberá os dados de acordo com o tipo selecionado ("Dados de Eventos" ou "Dados de Leads").
*   **Valores Nulos/Vazios:** Embora este exemplo preencha todos os campos, na prática, alguns campos podem vir como `null` ou string vazia (`""`) dependendo dos dados disponíveis. Seu sistema de integração deve estar preparado para lidar com esses casos.
*   **Lotes de Eventos:** O webhook de evento pode conter múltiplos eventos no array `data`. Seu sistema deve iterar sobre este array para processar cada evento individualmente.
*   **`fb_server_response`:** Este campo contém uma *string* que é, ela mesma, um JSON. Você precisará fazer um *parse* adicional dessa string se quiser acessar os detalhes da resposta da API do Facebook (como o `fbtrace_id` para debug).
*   **Datas:** Os campos `event_date` e `event_date_gmt` fornecem o horário local e GMT, respectivamente. O campo `event_time` fornece o timestamp Unix. Escolha o formato que melhor se adapta à sua necessidade.
