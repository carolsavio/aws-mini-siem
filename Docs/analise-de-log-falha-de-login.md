
# Analise de log - Falha de Login
{
  "eventTime": "2026-04-27T23:34:29Z",
  "eventName": "ConsoleLogin",
  "userIdentity": {
    "type": "IAMUser",
    "userName": "<USERNAME>"
  },
  "sourceIPAddress": "XXX.XX.XXX.XX",
  "userAgent": "Mozilla/5.0 (Linux; x86_64) Chrome",
  "awsRegion": "us-east-2",
  "errorMessage": "Failed authentication",
  "responseElements": {
    "ConsoleLogin": "Failure"
  },
  "additionalEventData": {
    "MFAUsed": "Yes"
  },
  "eventType": "AwsConsoleSignIn"
}

## Análise de Evento: Falha de Login no Console AWS

### Resumo

Este log representa uma tentativa de login falha no console da AWS utilizando um usuário IAM.

---

### Principais Observações

- **userIdentity.type: IAMUser**  
    Indica tentativa de autenticação utilizando um usuário IAM.
    
- **eventName: ConsoleLogin**  
    Evento relacionado ao login no console da AWS.
    
- **responseElements.ConsoleLogin: Failure**  
    Confirma que a autenticação falhou.
    
- **errorMessage: Failed authentication**  
    Indica falha no processo de autenticação (credenciais inválidas ou erro no MFA).
    
- **MFAUsed: Yes**  
    Mostra que MFA estava habilitado no fluxo de autenticação.
    
- **sourceIPAddress**  
    Origem da tentativa de login (mascarada).  
    Deve ser monitorada para identificar padrões suspeitos.
    
- **userAgent**  
    Indica acesso via navegador.  
    ⚠️ Pode ser falsificado e não deve ser considerado evidência confiável isoladamente.
    

---

### Possíveis Cenários

- Erro legítimo do usuário (senha ou MFA incorretos)
    
- Tentativa de acesso não autorizado
    
- Início de ataque de força bruta
    

---

### Impacto de Segurança

Falhas de login isoladas podem não representar risco imediato, porém:

- Múltiplas falhas em sequência podem indicar tentativa de ataque
    
- Devem ser correlacionadas com IP, horário e frequência
    

---

### Detecção

Este evento pode ser detectado com a seguinte regra:

```json
{ $.eventName = ConsoleLogin && $.errorMessage = "Failed authentication" }
```

---

### Notas:

- Eventos de falha de login podem variar dependendo do tipo de erro (senha vs MFA)
    
- Nem todas as falhas são registradas de forma consistente
    
- Correlação de eventos é essencial para detectar comportamento malicioso
    

---

### Conclusão

Este tipo de evento é fundamental para identificar tentativas de acesso indevido e deve ser monitorado continuamente como parte de uma estratégia de detecção de ameaças.