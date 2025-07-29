## 🎓 EXPLICAÇÃO COMPLETA DO OPEN/CLOSED PRINCIPLE (OCP)

---

### 🔍 O QUE DIZ O PRINCÍPIO?

> **“Classes devem estar abertas para extensão, mas fechadas para modificação.”**

Ou seja:

* Você **pode adicionar funcionalidades novas**.
* Mas **não precisa alterar o código já existente** para isso.

---

### 🧠 EXEMPLO PRÁTICO NO SEU SISTEMA

#### 📦 **1. Interface `NotificationInterface`**

```php
interface NotificationInterface
{
    public function send(Advertisement $advertisement): void;
}
```

* Essa interface **define um contrato**
* Toda notificação (e-mail, SMS, WhatsApp, etc.) **deve implementar o método `send()`**
* É a **base do Open/Closed**: cria um ponto de extensão comum

---

#### 📩 **2. Implementações concretas**

##### ✅ Exemplo: EmailNotification

```php
class EmailNotification implements NotificationInterface
{
    public function send(Advertisement $advertisement): void
    {
        echo "📧 Email enviado: {$advertisement->getTitle()}";
    }
}
```

##### ✅ Outro exemplo: SmsNotification

```php
class SmsNotification implements NotificationInterface
{
    public function send(Advertisement $advertisement): void
    {
        echo "📱 SMS: {$advertisement->getTitle()} - R$ {$advertisement->getPrice()}";
    }
}
```

##### ✅ Outro exemplo: WhatsAppNotification

```php
class WhatsAppNotification implements NotificationInterface
{
    public function send(Advertisement $advertisement): void
    {
        echo "💬 WhatsApp: {$advertisement->getTitle()} na categoria {$advertisement->getCategory()->getName()}";
    }
}
```

🔧 Você pode **criar quantas quiser** — desde que implementem a interface.

---

#### 🧰 **3. Alterações no `AdvertisementService`**

##### A. Guardar notificações registradas:

```php
private array $notifications = [];
```

##### B. Método para registrar notificações:

```php
public function addNotification(NotificationInterface $notification): void
{
    $this->notifications[] = $notification;
}
```

##### C. Método para enviar:

```php
private function sendNotifications(Advertisement $advertisement): void
{
    foreach ($this->notifications as $notification) {
        $notification->send($advertisement);
    }
}
```

##### D. Uso no método `create()`:

```php
public function create(Advertisement $advertisement): void
{
    // salva no banco...
    $this->sendNotifications($advertisement);
}
```

---

### ✅ O QUE ISSO PERMITE?

#### **🔓 Aberto para extensão**

* Basta criar uma nova classe de notificação
* E adicionar assim:

```php
$service->addNotification(new TelegramNotification());
```

#### **🔒 Fechado para modificação**

* Você **não altera o método `create()`**
* Você **não toca em `sendNotifications()`**
* O código **continua funcionando com novas notificações sem mudar nada**

---

### 🆚 COMPARAÇÃO

#### ❌ Antes (sem Open/Closed):

```php
public function create(Advertisement $ad)
{
    if ($tipo === 'email') { ... }
    elseif ($tipo === 'sms') { ... }
    elseif ($tipo === 'whatsapp') { ... } // +1 modificação
    elseif ($tipo === 'telegram') { ... } // +1 modificação
}
```

* Toda nova notificação **exige editar essa função**
* A função cresce infinitamente

#### ✅ Depois (com Open/Closed):

```php
foreach ($this->notifications as $notification) {
    $notification->send($ad);
}
```

* **Nunca mais modifica esse trecho**
* Só adiciona classes novas — **extensão limpa**

---

### 🏆 BENEFÍCIOS PRÁTICOS

| Benefício         | Explicação breve                                  |
| ----------------- | ------------------------------------------------- |
| 🔄 **Extensível** | Novas notificações sem mudar código antigo        |
| 🧱 **Estável**    | Código já testado e em produção não sofre impacto |
| 🔬 **Testável**   | Cada notificação pode ser testada isoladamente    |
| 🧩 **Flexível**   | Pode combinar várias notificações livremente      |

---

### 📌 FRASE DE IMPACTO PARA SUA APRESENTAÇÃO

> “Open/Closed não é só sobre escrever menos — é sobre escrever melhor. Ao invés de mudar código que já funciona, eu crio novas peças que se encaixam perfeitamente.”

