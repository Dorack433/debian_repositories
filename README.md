# 📦 Configuração de Repositórios no Debian 12 (Bookworm)

Este guia explica **onde** e **como** configurar corretamente os repositórios oficiais do Debian 12 (Bookworm) e o repositório do **Tor Project**, de forma segura e compatível.

---

## 📍 Onde fazer as alterações

No Debian, os repositórios APT são definidos em dois locais principais:

* **Arquivo principal:**

  ```bash
  /etc/apt/sources.list
  ```

* **Arquivos adicionais (recomendado):**

  ```bash
  /etc/apt/sources.list.d/
  ```

### ✅ Boa prática

* Repositórios **oficiais do Debian** → `sources.list`
* Repositórios de **terceiros (ex: Tor Project)** → `sources.list.d/`

---

## 🔐 Pré-requisitos

Você precisa ter permissões de **sudo**.

Verifique:

```bash
sudo whoami
```

Se retornar `root`, está tudo certo.

---

## 📝 Passo 1 – Editar os repositórios oficiais do Debian

Abra o arquivo principal:

```bash
sudo nano /etc/apt/sources.list
```

Substitua **todo o conteúdo** por:

```bash
# ================================
# Debian 12 (Bookworm) - Oficiais
# ================================

# Repositório principal
deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
deb-src http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware

# Atualizações estáveis
deb http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
deb-src http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware

# Segurança
deb http://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
deb-src http://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
```

Salve e saia:

* `CTRL + O` → Enter
* `CTRL + X`

---

## 🧅 Passo 2 – Adicionar o repositório do Tor Project (forma correta)

Crie um arquivo dedicado:

```bash
sudo nano /etc/apt/sources.list.d/tor.list
```

Cole o conteúdo:

```bash
# Tor Project - Debian 12 (Bookworm)
deb [signed-by=/usr/share/keyrings/tor-archive-keyring.gpg] https://deb.torproject.org/torproject.org bookworm main
deb-src [signed-by=/usr/share/keyrings/tor-archive-keyring.gpg] https://deb.torproject.org/torproject.org bookworm main
```

Salve e saia.

---

## 🔑 Passo 3 – Instalar a chave GPG do Tor

Forma recomendada:

```bash
sudo apt update
sudo apt install -y tor-archive-keyring
```

Ou manualmente:

```bash
curl -fsSL https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc \
| sudo gpg --dearmor -o /usr/share/keyrings/tor-archive-keyring.gpg
```

---

## 🔄 Passo 4 – Atualizar a lista de pacotes

```bash
sudo apt update
```

✔️ Se **não houver erros de assinatura ou Release**, os repositórios estão corretos.

---

## 🧪 Passo 5 – Validação

Verifique se o Tor está disponível:

```bash
apt policy tor
```

Instalação de teste:

```bash
sudo apt install tor
```

---

## ⚠️ Observações importantes

* `non-free-firmware` é **obrigatório no Debian 12**
* Não misture `testing`, `sid` ou releases antigos
* **Tor Browser não é instalado via APT**, apenas o daemon `tor`
* Para maior segurança, use Firejail ou namespaces

---

## 📚 Referências

* [https://www.debian.org/releases/bookworm/](https://www.debian.org/releases/bookworm/)
* [https://support.torproject.org/apt/](https://support.torproject.org/apt/)

---

## ✅ Conclusão

Seguindo este guia, seu Debian 12 estará:

* Com repositórios corretos
* Compatível com drivers modernos
* Pronto para uso do Tor de forma segura

Ideal para ambientes de **segurança, privacidade e pesquisa**.
