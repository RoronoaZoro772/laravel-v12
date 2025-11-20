<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

# Boilerplate Profissional (Bootstrap Edition)

![Laravel](https://img.shields.io/badge/Laravel-12-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.2%2B-777BB4?style=for-the-badge&logo=php&logoColor=white)
![PostgreSQL 16](https://img.shields.io/badge/PostgreSQL-16-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white)
![SASS](https://img.shields.io/badge/SASS-hotpink?style=for-the-badge&logo=sass&logoColor=white)

[![Build Status](https://github.com/angelluzk/laravel-v12/actions/workflows/laravel.yml/badge.svg)](https://github.com/angelluzk/laravel-v12/actions)
![Code Style](https://img.shields.io/badge/Code%20Style-Laravel%20Pint-blue)
![Static Analysis](https://img.shields.io/badge/Static%20Analysis-Larastan-yellow)

> **Starter moderno e “Enterprise-Ready” pré-configurado com Bootstrap 5, Docker Compose V2, ferramentas de QA, análise estática e CI/CD.**

---

## 📋 Sobre o Projeto

Este repositório fornece uma fundação sólida para projetos em **Laravel 12**, já configurado com a stack clássica de mercado (**Bootstrap 5 + SASS**). Focado em qualidade, padronização e ambiente Docker robusto (Sail).

---

## 🛠️ Tecnologias e Recursos

- **Framework:** Laravel 12  
- **Linguagem:** PHP 8.2+
- **Frontend:** Bootstrap 5 + SASS (via Vite)  
- **Banco de Dados:** PostgreSQL 16 (Docker)  
- **Ambiente de Desenvolvimento:** Laravel Sail (Docker Compose V2)  
- **Code Style:** Laravel Pint (PSR-12)  
- **Análise Estática:** Larastan (PHPStan – Level 5)  
- **CI/CD:** GitHub Actions  
- **Extras:** IDE Helper, Redis, Mailpit

---

## 🚀 Instalação

### ⚙️ Requisitos do Ambiente (php.ini)

Caso você opte por rodar o projeto **sem Docker** (instalação nativa), garanta que as seguintes extensões estejam habilitadas no seu arquivo `php.ini`:

- `ctype`
- `curl`
- `dom`
- `fileinfo`
- `filter`
- `hash`
- `mbstring`
- `openssl`
- `pcre`
- `pdo`
- `pdo_pgsql` (Driver do Banco de Dados)
- `session`
- `tokenizer`
- `xml`

> **Nota:** Se você estiver usando **Laravel Sail (Docker)**, pode ignorar esta lista. O container já vem com todas essas extensões configuradas e otimizadas automaticamente.

---

### 1. Clone o Repositório
```bash
git clone https://github.com/SEU-USUÁRIO/laravel-v12.git
cd laravel-v12
```

### 2. Instale as Dependências

#### Opção A — Composer Local

```bash
composer install
```

#### Opção B — Composer via Docker

```bash
docker run --rm -u "$(id -u):$(id -g)" \
  -v "$(pwd):/var/www/html" -w /var/www/html \
  laravelsail/php82-composer:latest \
  composer install --ignore-platform-reqs
```

### 3. Configure o Ambiente

Crie o arquivo .env. Importante: Se estiver no Windows, veja a seção de "Solução de Problemas" abaixo sobre o WWWUSER.

```bash
cp .env.example .env
```

No .env, ajuste o banco para PostgreSQL:

```bash
DB_CONNECTION=pgsql
DB_PORT=5432

# Se estiver no Windows, adicione também:
WWWUSER=1000
WWWGROUP=1000
```

### 4. Inicialize os Containers

```bash
./vendor/bin/sail up -d
# OU, se o comando sail falhar no Windows:
docker compose up -d
```

### 5. Setup Final & Frontend

Gere a chave, migre o banco e compile os assets do Bootstrap:

```bash
# Backend Setup
docker compose exec laravel.test php artisan key:generate
docker compose exec laravel.test php artisan migrate

# Frontend Setup (Instala e compila Bootstrap/SASS)
docker compose exec laravel.test npm install
docker compose exec laravel.test npm run build
```

Aplicação disponível em:
**[http://localhost](http://localhost)**

---

## ❓ Solução de Problemas Comuns (Troubleshooting)

> Erro: **"Unsupported operating system"** ou **Sail não roda**.

Se você usa Windows (Git Bash/Mingw), o script ./vendor/bin/sail pode falhar. Solução: Use os comandos nativos do Docker Compose:
- Em vez de `sail up`, use `docker compose up`.
-Em vez de `sail npm run...`, use `docker compose exec laravel.test npm run...`.

> Erro: **"groupadd: invalid group ID" (Docker build fail)**.

Se o container falhar ao subir com erro de groupadd, é porque o Docker no Windows não detectou seu ID de usuário. Solução: Adicione estas duas linhas ao final do seu arquivo .env:

```bash
WWWUSER=1000
WWWGROUP=1000
```
Depois reconstrua: `docker compose up -d --build`.

---

## 🛡️ Qualidade e Ferramentas de QA

### 🎨 Formatação — Laravel Pint

```bash
docker compose exec laravel.test ./vendor/bin/pint
```

### 🔎 Análise Estática — Larastan

```bash
docker compose exec laravel.test ./vendor/bin/phpstan analyse
```

### 🧪 Testes Automatizados

```bash
docker compose exec laravel.test php artisan test
```

### 🧠 Atualizar IDE Helper

```bash
docker compose exec laravel.test php artisan ide-helper:generate
```

---

## 🤖 CI/CD — GitHub Actions

O workflow `laravel.yml` executa automaticamente:

1. Verificação de padrão de código (Pint)
2. Análise estática (Larastan)
3. Testes completos

Tudo isso ao enviar alterações para a branch `main`.

---

## 📂 Arquivos Importantes

* **compose.yaml** — Serviços Docker (App, DB, Redis, Mailpit)
* **vite.config.js** — Configuração do Build (Bootstrap/SASS).
* **phpstan.neon** — Regras do PHPStan / Larastan
* **pint.json** — Configurações do Laravel Pint
* **.editorconfig** — Padronização entre editores

---

## 📚 Documentação Adicional
Quer entender profundamente as ferramentas usadas neste projeto?  
Leia a [Documentação de Conceitos Técnicos](./CONCEITOS_TECNICOS.md).

---

## 👩‍🎓 Autoria

<img src="https://github.com/angelluzk.png" width="100px;" alt="Foto de Angel Luz"/>

> Desenvolvido com 💛 por **Angel Luz**.

Se quiser conversar, colaborar ou oferecer uma oportunidade:

📬 E-mail: [contatoangelluz@gmail.com](mailto:contatoangelluz@gmail.com)  
🐙 GitHub: [@angelluzk](https://github.com/angelluzk)  
💼 LinkedIn: [linkedin.com/in/angelitaluz](https://www.linkedin.com/in/angelitaluz/)  
🗂️Website / Portfólio: [meu_portfolio/](https://angelluzk.github.io/meu_portfolio/) 

-----

<div align="center">

> “Transformando código em fluxo, e ideias em movimento.”

</div>