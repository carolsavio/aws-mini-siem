# Regras de Detecção

## 1. Root Login (CRÍTICO)

### O que detecta:

Login usando conta root

### Filter Pattern:

```
{ $.userIdentity.type = "Root" && $.eventType = "AwsConsoleSignIn" }
```

### Por que é importante:

- Root tem acesso TOTAL
- Uso deve ser raríssimo


---

## 2. IAM User Created

### O que detecta:

Criação de novo usuário IAM

### Filter Pattern:

```
{ $.eventName = CreateUser }
```

### Por que é importante:

- Pode indicar criação não autorizada
- Possível persistência de atacante

---

## 3. Failed Login Attempts

### O que detecta:

Falhas de autenticação no console

### Filter Pattern:

```
{ $.eventName = ConsoleLogin && $.errorMessage = "Failed authentication" }
```

### Por que é importante:

- Pode indicar brute force
- Tentativa de acesso indevido

⚠️ Observação (importante):  
Esse evento pode variar dependendo de MFA, tipo de erro, etc.

![Print](screenshots/alarms.png)