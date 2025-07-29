🎓 EXPLICAÇÃO COMPLETA DO OPEN/CLOSED - LINHA POR LINHA
Vou te explicar TUDO sobre o princípio Open/Closed para você dominar completamente na apresentação.

📁 ARQUIVO 1: NotificationInterface.php

4 files read
CABEÇALHO (Linhas 1-7):
<?php
declare(strict_types=1);
namespace App\Service\Notification;
use App\Entity\Advertisement;

Copy
php
Linha 1: Abertura do PHP

Linha 3: Força tipagem estrita

Linha 5: Define namespace App\Service\Notification

Linha 7: Importa a classe Advertisement

DECLARAÇÃO DA INTERFACE (Linha 9):
interface NotificationInterface

Copy
php
interface = Palavra-chave que cria um "contrato"

NotificationInterface = Nome da interface

Interface ≠ Classe: Interface só DECLARA métodos, não implementa

MÉTODO DA INTERFACE (Linha 11):
public function send(Advertisement $advertisement): void;

Copy
php
Linha por linha:
public function send = Declara que deve existir um método público chamado send

Advertisement $advertisement = O método deve receber um parâmetro do tipo Advertisement

: void = O método não deve retornar nada

; = IMPORTANTE! Interface termina com ; (não tem implementação)

O que isso significa:

"Qualquer classe que implementar esta interface DEVE ter um método send() com essa assinatura exata"

📁 ARQUIVO 2: EmailNotification.php
CABEÇALHO (Linhas 1-7):
<?php
declare(strict_types=1);
namespace App\Service\Notification;
use App\Entity\Advertisement;

Copy
php
Mesmo padrão das outras classes

DECLARAÇÃO DA CLASSE (Linha 9):
class EmailNotification implements NotificationInterface

Copy
php
Linha por linha:
class EmailNotification = Cria uma classe chamada EmailNotification

implements NotificationInterface = CRUCIAL! Promete seguir o contrato da interface

Significa: "Esta classe promete ter todos os métodos que a interface exige"

IMPLEMENTAÇÃO DO MÉTODO (Linhas 11-18):
public function send(Advertisement $advertisement): void
{
    // Simula envio de email
    echo "📧 Email enviado: Novo anúncio '{$advertisement->getTitle()}' publicado!\n";
    
    // Aqui seria a implementação real do envio de email
    // mail($to, $subject, $message, $headers);
}

Copy
php
Linha por linha:

Linha 11: public function send(Advertisement $advertisement): void

DEVE ter a mesma assinatura da interface

Público, nome send, recebe Advertisement, retorna void

Linha 14: echo "📧 Email enviado: Novo anúncio '{$advertisement->getTitle()}' publicado!\n";

echo = imprime na tela (simula envio)

$advertisement->getTitle() = pega o título do anúncio

\n = quebra de linha

Linhas 16-17: Comentários sobre implementação real

Na vida real, usaria mail() para enviar email de verdade

📁 ARQUIVO 3: SmsNotification.php
ESTRUTURA IDÊNTICA AO EMAIL:
class SmsNotification implements NotificationInterface
{
    public function send(Advertisement $advertisement): void
    {
        echo "📱 SMS enviado: Anúncio '{$advertisement->getTitle()}' - R$ {$advertisement->getPrice()}\n";
        // $smsService->send($phoneNumber, $message);
    }
}

Copy
php
Diferenças do Email:

Linha 9: class SmsNotification = Nome diferente

Linha 14: Mensagem diferente (inclui preço: $advertisement->getPrice())

Linha 17: Comentário sobre API de SMS

Mesma interface, implementação diferente!

📁 ARQUIVO 4: WhatsAppNotification.php
MAIS UMA IMPLEMENTAÇÃO:
class WhatsAppNotification implements NotificationInterface
{
    public function send(Advertisement $advertisement): void
    {
        echo "💬 WhatsApp enviado: '{$advertisement->getTitle()}' na categoria {$advertisement->getCategory()->getName()}\n";
        // $whatsappApi->sendMessage($phoneNumber, $message);
    }
}

Copy
php
Diferenças:

Linha 9: class WhatsAppNotification = Nome diferente

Linha 14: Mensagem inclui categoria: $advertisement->getCategory()->getName()

Linha 17: Comentário sobre API do WhatsApp

Mesma interface, mais uma implementação diferente!

📁 ARQUIVO 5: AdvertisementService.php (PARTE OPEN/CLOSED)
PROPRIEDADE (Linha 17):
private array $notifications = [];

Copy
php
private array = Array privado da classe

$notifications = Nome da propriedade

= [] = Inicializa como array vazio

Propósito: Guarda todas as notificações registradas

MÉTODO addNotification (Linhas 19-22):
public function addNotification(NotificationInterface $notification): void
{
    $this->notifications[] = $notification;
}

Copy
php
Linha por linha:

Linha 19: public function addNotification(NotificationInterface $notification): void

Método público para adicionar notificações

NotificationInterface $notification = CRUCIAL! Só aceita objetos que implementam a interface

: void = Não retorna nada

Linha 21: $this->notifications[] = $notification;

$this->notifications = Acessa o array da classe

[] = Adiciona no final do array

= $notification = Adiciona o objeto recebido

Exemplo de execução:

// Primeira chamada:
$service->addNotification(new EmailNotification());
// $this->notifications = [EmailNotification]

// Segunda chamada:
$service->addNotification(new SmsNotification());
// $this->notifications = [EmailNotification, SmsNotification]

Copy
php
MÉTODO create (Linhas 47-53):
public function create(Advertisement $advertisement): void
{
    // Aqui seria a persistência no banco
    // $entityManager->persist($advertisement);
    // $entityManager->flush();
    
    // Enviar notificações usando o princípio Open/Closed
    $this->sendNotifications($advertisement);
}

Copy
php
Linha por linha:

Linha 47: public function create(Advertisement $advertisement): void

Método público para criar anúncio

Recebe um Advertisement já construído

Linhas 49-51: Comentários sobre persistência

Na vida real, salvaria no banco aqui

Linha 53: $this->sendNotifications($advertisement);

Chama método privado que envia todas as notificações

Passa o anúncio como parâmetro

MÉTODO sendNotifications (Linhas 113-117) - O CORAÇÃO DO OPEN/CLOSED:
private function sendNotifications(Advertisement $advertisement): void
{
    foreach ($this->notifications as $notification) {
        $notification->send($advertisement);
    }
}

Copy
php
Linha por linha:

Linha 113: private function sendNotifications(Advertisement $advertisement): void

Método privado (só a própria classe usa)

Recebe um Advertisement para enviar nas notificações

Linha 115: foreach ($this->notifications as $notification)

foreach = Loop que percorre cada item do array

$this->notifications = Array com todas as notificações registradas

as $notification = A cada volta, $notification recebe um objeto diferente

Linha 116: $notification->send($advertisement);

POLIMORFISMO! Chama o método send() do objeto atual

Não sabe se é Email, SMS ou WhatsApp

Só sabe que tem método send() (garantido pela interface)

Passa o $advertisement como parâmetro

Exemplo de execução:

// Se $this->notifications = [EmailNotification, SmsNotification, WhatsAppNotification]

// Primeira volta do loop:
// $notification = EmailNotification
// $notification->send($advertisement) → EmailNotification::send() → "📧 Email enviado..."

// Segunda volta do loop:
// $notification = SmsNotification
// $notification->send($advertisement) → SmsNotification::send() → "📱 SMS enviado..."

// Terceira volta do loop:
// $notification = WhatsAppNotification
// $notification->send($advertisement) → WhatsAppNotification::send() → "💬 WhatsApp enviado..."

Copy
php
🔄 FLUXO COMPLETO DO OPEN/CLOSED
1. CONFIGURAÇÃO (Extensão):
$service = new AdvertisementService();
// $this->notifications = []

$service->addNotification(new EmailNotification());
// $this->notifications = [EmailNotification]

$service->addNotification(new SmsNotification());
// $this->notifications = [EmailNotification, SmsNotification]

$service->addNotification(new WhatsAppNotification());
// $this->notifications = [EmailNotification, SmsNotification, WhatsAppNotification]

Copy
php
2. USO:
$anuncio = new Advertisement('iPhone 15', 'Novo', 5000.00);
$service->create($anuncio);

Copy
php
3. EXECUÇÃO INTERNA:
// create() é chamado
public function create(Advertisement $advertisement): void
{
    // Salva no banco...
    $this->sendNotifications($advertisement); // ← Chama este método
}

// sendNotifications() executa
private function sendNotifications(Advertisement $advertisement): void
{
    // foreach ([EmailNotification, SmsNotification, WhatsAppNotification] as $notification)
    
    // Primeira volta: EmailNotification->send() → "📧 Email enviado..."
    // Segunda volta: SmsNotification->send() → "📱 SMS enviado..."
    // Terceira volta: WhatsAppNotification->send() → "💬 WhatsApp enviado..."
}

Copy
php
🎯 POR QUE ISSO É OPEN/CLOSED
🔒 FECHADO PARA MODIFICAÇÃO:
O método sendNotifications() NUNCA MUDA

O foreach SEMPRE FUNCIONA IGUAL

A classe AdvertisementService PERMANECE ESTÁVEL

🔓 ABERTO PARA EXTENSÃO:
// Posso criar nova notificação:
class TelegramNotification implements NotificationInterface
{
    public function send(Advertisement $advertisement): void
    {
        echo "📨 Telegram enviado: {$advertisement->getTitle()}";
    }
}

// E usar sem modificar NADA do código existente:
$service->addNotification(new TelegramNotification());
// O foreach continua funcionando!

Copy
🏆 PONTOS-CHAVE PARA SUA APRESENTAÇÃO
1. Interface como Contrato:
"A interface garante que todas as notificações tenham o método send()"

2. Polimorfismo:
"O código não sabe qual tipo específico, só sabe que tem send()"

3. Extensibilidade:
"Novas notificações = novas classes, sem modificar código existente"

4. Estabilidade:
"O método sendNotifications() nunca precisa mudar"

Agora você domina completamente o Open/Closed! 🚀
