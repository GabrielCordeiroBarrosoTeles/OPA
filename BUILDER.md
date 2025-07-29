
## 🎓 **EXPLICAÇÃO COMPLETA DO BUILDER (linha por linha)**

**Arquivo:** `AdvertisementBuilder.php`

---

### 📦 **Cabeçalho**

```php
<?php
declare(strict_types=1);
namespace App\Builder;
use App\Entity\Advertisement;
use App\Entity\Category;
```

* `<?php` → Início do arquivo PHP
* `declare(strict_types=1);` → Ativa **tipagem estrita** (protege contra erros de tipo)
* `namespace App\Builder;` → Define onde essa classe "mora" no projeto
* `use ...` → Importa classes necessárias para o Builder funcionar

---

### 🧱 **Declaração da Classe**

```php
class AdvertisementBuilder
```

* Define o **Builder de anúncios**, responsável por construir `Advertisement` passo a passo.

---

### 🔐 **Propriedades Privadas**

```php
private string $title = '';
private ?string $description = null;
private ?float $price = null;
private int $advertiserId = 0;
private ?Category $category = null;
```

Essas variáveis guardam os dados **temporariamente** antes de criar o anúncio:

| Propriedade    | Tipo        | Valor Inicial | Descrição                         |
| -------------- | ----------- | ------------- | --------------------------------- |
| `title`        | `string`    | `''`          | Título do anúncio                 |
| `description`  | `?string`   | `null`        | Descrição (opcional)              |
| `price`        | `?float`    | `null`        | Preço (opcional)                  |
| `advertiserId` | `int`       | `0`           | ID do anunciante                  |
| `category`     | `?Category` | `null`        | Objeto da categoria (obrigatório) |

---

### 🛠️ **Métodos Setters**

```php
public function setTitle(string $title): self
{
    $this->title = $title;
    return $this;
}
```

Todos os outros métodos (`setDescription`, `setPrice`, etc.) seguem a mesma estrutura:

* Recebem 1 parâmetro
* Atribuem à propriedade correspondente
* Retornam `$this` → Isso permite **encadeamento fluente**

**Exemplo encadeado:**

```php
$builder->setTitle()->setPrice()->setCategory();
```

---

### 🚨 **makeUrgent**

```php
public function makeUrgent(): self
{
    $this->title = '🔥 URGENTE: ' . $this->title;
    return $this;
}
```

* Modifica o título, adicionando destaque
* Permite regras de negócio **direto no Builder**

---

### 🌟 **makePremium**

```php
public function makePremium(): self
{
    $this->title = '⭐ PREMIUM: ' . $this->title;
    return $this;
}
```

* Mesmo conceito do `makeUrgent()`
* Demonstra que o Builder pode ter lógica adicional, **não apenas setters**

---

### 🧪 **build(): Validação + Criação**

```php
public function build(): Advertisement
{
    if (empty($this->title)) {
        throw new \InvalidArgumentException('Título é obrigatório!');
    }
    if ($this->advertiserId === 0) {
        throw new \InvalidArgumentException('Advertiser ID é obrigatório!');
    }
    if ($this->category === null) {
        throw new \InvalidArgumentException('Categoria é obrigatória!');
    }

    $advertisement = new Advertisement($this->title, $this->description, $this->price);
    $advertisement->setAdvertiserId($this->advertiserId);
    $advertisement->setCategory($this->category);

    return $advertisement;
}
```

### 📌 Linha por linha:

* 🔍 **Validações**:

  * `title` não pode estar vazio
  * `advertiserId` não pode ser zero
  * `category` não pode ser `null`

* ✅ **Criação segura**:

  * Só cria `Advertisement` **se todos os dados estiverem válidos**
  * Configura `advertiserId` e `category` com os métodos apropriados
  * Retorna o objeto montado e pronto para uso

---

### 🧪 **Exemplo Prático de Uso**

```php
$builder = new AdvertisementBuilder();
$ad = $builder
    ->setTitle('iPhone 15 Pro')
    ->setDescription('iPhone novo, na caixa')
    ->setPrice(5000.00)
    ->setAdvertiserId(1)
    ->setCategory($category)
    ->makeUrgent()
    ->build();
```

---

### 🏆 **Pontos-Chave para sua Apresentação**

#### 1. ✨ Fluência

> "Cada método retorna `$this`, permitindo um encadeamento que parece uma conversa em inglês."

#### 2. ✅ Segurança e Validação

> "O `build()` só cria o objeto se todos os dados forem válidos. Nada de objetos incompletos no sistema."

#### 3. 📦 Encapsulamento de Lógica

> "Funções como `makeUrgent()` centralizam lógicas de negócio que antes ficavam espalhadas."

#### 4. 🔁 Reutilização

> "Com um único Builder posso criar diversos anúncios — simples, urgentes, premium, etc."

---

### 🎯 Conclusão

O **Builder Pattern** ajuda você a:

* Organizar a construção de objetos complexos
* Validar dados antes de criar
* Escrever código mais legível e fluente
* Reduzir erros e duplicações
* Manter regras de negócio centralizadas

Se quiser, posso gerar um *slide*, um *resumo visual* ou até uma *fala de apresentação*. Só dizer o formato!
