# Analise de Log - Root Login

{
  "eventTime": "2026-04-27T18:48:42Z",
  "eventName": "ConsoleLogin",
  "userIdentity": {
    "type": "Root"
  },
  "sourceIPAddress": "XXX.XX.XXX.XX",
  "userAgent": "Mozilla/5.0 (Linux; x86_64) Chrome",
  "awsRegion": "us-east-1",
  "responseElements": {
    "ConsoleLogin": "Success"
  },
  "additionalEventData": {
    "MFAUsed": "Yes"
  },
  "eventType": "AwsConsoleSignIn"
}

## Análise de Evento: Login com Conta Root

### Resumo

Este log representa um login bem-sucedido no console da AWS utilizando a conta root.

---

### Principais Observações

- **userIdentity.type: Root**  
    Indica uso da conta root, que possui privilégios administrativos totais.
    
- **eventName: ConsoleLogin**  
    Confirma tentativa de autenticação no console da AWS.
    
- **responseElements.ConsoleLogin: Success**  
    O login foi realizado com sucesso.
    
- **MFAUsed: Yes**  
    Autenticação multifator foi utilizada, reduzindo o risco do acesso.
    
- **sourceIPAddress**  
    Origem da requisição (mascarada).  
    Deve ser analisada para identificar acessos suspeitos ou fora do padrão.
    
- **userAgent**  
    Indica que o acesso foi realizado via navegador.  
    ⚠️ Pode ser facilmente falsificado e não deve ser usado como única evidência.
    

---

### Impacto de Segurança

O uso da conta root é considerado um **evento de alto risco**, mesmo quando protegido por MFA.

Possíveis riscos incluem:

- Comprometimento total da conta AWS
    
- Uso inadequado em vez de identidades IAM (má prática)
    

---

### Detecção

Este evento é identificado pela seguinte regra de detecção:

```json
{ $.userIdentity.type = "Root" && $.eventType = "AwsConsoleSignIn" }
```

Quando detectado:

- Uma métrica customizada (`RootLogin`) é incrementada
    
- Um CloudWatch Alarm é avaliado
    
- Uma notificação é enviada via SNS

---

### Notas:

- O uso da conta root deve ser restrito ao mínimo necessário
    
- MFA reduz significativamente o risco, mas NÃO elimina completamente
    
- Eventos desse tipo devem ser sempre revisados manualmente
---

### Conclusão

Esta detecção demonstra como eventos críticos de autenticação podem ser identificados utilizando serviços nativos da AWS, permitindo resposta rápida a possíveis riscos de segurança.