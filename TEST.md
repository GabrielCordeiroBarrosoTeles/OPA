### 🧪 **Testando os Padrões na Prática — `design_patterns_demo.php`**

---

## 🔷 **SEÇÃO 1: TESTANDO O BUILDER**

### ✅ O que foi testado:

| Teste                        | Verifica                                    |
| ---------------------------- | ------------------------------------------- |
| `setTitle`, `setPrice`, etc. | Se cada método existe e funciona            |
| Encadeamento (Fluência)      | Cada método retorna `$this`                 |
| `build()`                    | Valida dados antes de criar `Advertisement` |
| `makeUrgent()`               | Modifica o título automaticamente           |
| Reutilização do builder      | Mesmo builder → vários objetos diferentes   |

### 💡 Resultado:

```bash
✅ Anúncio criado: 🔥 URGENTE: iPhone 15 Pro - R$ 5000
📦 Criados 3 anúncios em lote usando o mesmo builder
```

---

## 🔷 **SEÇÃO 2: TESTANDO OPEN/CLOSED**

### ✅ O que foi testado:

| Teste                    | Verifica                                       |
| ------------------------ | ---------------------------------------------- |
| `addNotification()`      | Adiciona diferentes notificações               |
| `create()`               | Dispara `sendNotifications()` automaticamente  |
| Polimorfismo             | Todas as notificações chamam `send()`          |
| Extensão sem modificação | Adição do WhatsApp sem alterar código anterior |

### 💡 Resultado esperado:

```bash
📧 Email enviado: Novo anúncio '🔥 URGENTE: iPhone 15 Pro' publicado!
📱 SMS enviado: Anúncio '🔥 URGENTE: iPhone 15 Pro' - R$ 5000
💬 WhatsApp enviado: '🔥 URGENTE: iPhone 15 Pro' na categoria Eletrônicos
```

---

## 🔷 **SEÇÃO 3: TESTANDO A INTEGRAÇÃO TOTAL**

### 🚀 Builder + Open/Closed Juntos

| Verificação                  | Resultado esperado                      |
| ---------------------------- | --------------------------------------- |
| `createWithBuilder()` existe | Usa o builder internamente              |
| Fluxo completo               | Cria anúncio + envia todas notificações |

### 💡 Resultado final:

```bash
📧 Email enviado: Novo anúncio 'Drone Profissional' publicado!
📱 SMS enviado: Anúncio 'Drone Profissional' - R$ 2800
💬 WhatsApp enviado: 'Drone Profissional' na categoria Eletrônicos
```

---

## 🏆 CONCLUSÃO FINAL DO TESTE

| Padrão          | Verificação                               | Resultado            |
| --------------- | ----------------------------------------- | -------------------- |
| **Builder**     | Criação fluente, validação e reutilização | ✅ Perfeito           |
| **Open/Closed** | Extensível sem alterar código base        | ✅ Confirmado         |
| **Integração**  | Ambos os padrões usados em harmonia       | ✅ Funcionando juntos |

---

## 🎤 Frase para encerrar sua explicação:

> “Esse teste não só confirma que o código funciona — ele mostra que usamos design patterns da forma correta: com fluência, validação, extensibilidade e desacoplamento. É código limpo que cresce sem quebrar.”
