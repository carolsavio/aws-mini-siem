## Lições Aprendidas

### 1. Métricas customizadas não aparecem imediatamente no CloudWatch

Após a criação dos _Metric Filters_, as métricas customizadas não ficaram disponíveis imediatamente na seção de criação de alarmes.

Observações:

- As métricas só se tornaram visíveis após a geração de eventos correspondentes (ex: login root ou criação de usuário IAM).
    
- Em alguns casos, houve atraso significativo na propagação (até quase 1 hora.).
    
- Isso indica que métricas customizadas no CloudWatch dependem da ingestão de eventos reais (_datapoints_) para se tornarem selecionáveis.
---

### 2. Dependência de eventos reais para validação

Foi necessário executar ações específicas para validar as detecções:

- Login com conta root para acionar `RootLogin`
    
- Criação de usuário IAM para acionar `CreateUser`

Sem esses eventos, as métricas não eram exibidas ou atualizadas corretamente.

---

### 3. Comportamento do fluxo de autenticação com MFA

Durante testes de login com múltiplas tentativas falhas:

- Após várias tentativas incorretas, a AWS pode exigir verificações adicionais (comportamento de proteção contra abuso).
    
- Esse mecanismo pode variar conforme contexto, IP e sessão.

Observação importante:

- Abrir uma nova aba ou iniciar uma nova sessão pode reiniciar o fluxo de autenticação.
    
- Isso não representa uma falha de segurança direta, mas sim comportamento esperado de gerenciamento de sessão.

---

### 4. Limitações na detecção de falhas de login

Foi observado que eventos de falha de login nem sempre acionaram alertas conforme esperado.

Possíveis causas:

- Nem todas as falhas de autenticação são registradas da mesma forma no AWS CloudTrail
    
- O campo `errorMessage` pode variar dependendo do tipo de erro (senha vs MFA)
    
- Eventos de autenticação com MFA podem ter estrutura diferente dos eventos sem MFA

Além disso:

- O alarme foi acionado em métricas/dashboards, mas não gerou notificação via SNS em alguns testes

---

### 5. Considerações sobre SNS e Alarmes

Em alguns cenários:

- A métrica era atualizada corretamente
    
- O alarme mudava de estado
    
- Porém, a notificação SNS não era enviada

Possíveis explicações:

- Configuração incorreta do trigger (estado ALARM vs OK)
    
- SNS não associado corretamente ao alarme
    
- Subscription não confirmada
    
- Delay na avaliação do alarme

---

### Conclusão

Esses testes demonstram que:

- Monitoramento em cloud depende fortemente de eventos reais
    
- Existe latência e comportamento assíncrono nos serviços da AWS
    
- Nem todos os eventos de autenticação são trivialmente detectáveis
    
- Testes práticos são essenciais para validar regras de detecção

Este laboratório reforçou a importância de validação empírica em ambientes de segurança cloud.
