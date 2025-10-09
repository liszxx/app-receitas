# app-receitas
# 🍲 App de Receitas

## 📖 Sobre o projeto

O **App de Receitas** foi desenvolvido como projeto integrador do curso de Análise e Desenvolvimento de Sistemas.
Seu objetivo é oferecer uma forma prática e intuitiva de **visualizar, cadastrar, editar, excluir e favoritar receitas**, com suporte a modo offline e tema claro/escuro.

O aplicativo foi desenvolvido em **Flutter**, utilizando **Provider** para gerenciamento de estado, **HTTP** para consumo de API e **SharedPreferences** para persistência local.

---

## 🧩 Funcionalidades

* Listagem de receitas (GET API)
* Exibição de detalhes da receita
* Cadastro, edição e exclusão de receitas (POST/PUT/DELETE)
* Favoritar receitas (armazenamento local)
* Pesquisa e filtro de receitas
* Tema claro e escuro
* Suporte básico offline (cache e mensagens de erro)

---

## 🧱 Estrutura do projeto

```
lib/
├── models/        # Classes (ex.: Receita)
├── services/      # Comunicação com API
├── repositories/  # Regras de negócio e persistência
├── providers/     # Gerenciamento de estado (Provider)
├── views/         # Telas e widgets
│   ├── widgets/   # Componentes reutilizáveis
├── utils/         # Helpers, máscaras e constantes
└── main.dart      # Ponto de entrada do app
```

---

## ⚙️ Tecnologias utilizadas

* **Flutter** (SDK)
* **Provider** – gerenciamento de estado
* **HTTP** – consumo de API REST
* **SharedPreferences** – armazenamento local
* **flutter_dotenv** – variáveis de ambiente

---

## 🔌 Configuração e execução

### Pré-requisitos

* Flutter SDK instalado
* Editor (VS Code / Android Studio)

### Passos para rodar o projeto

1. Clone o repositório:

   ```bash
   git clone https://github.com/seu-usuario/app-receitas.git
   ```
2. Entre na pasta do projeto:

   ```bash
   cd app-receitas
   ```
3. Instale as dependências:

   ```bash
   flutter pub get
   ```
4. Crie o arquivo `.env` baseado no `.env.example` e adicione a URL da sua API:

   ```
   API_URL=https://mockapi.io/api/v1/
   ```
5. Execute o app:

   ```bash
   flutter run
   ```

---

## 👥 Equipe de desenvolvimento

| Integrante   | Responsável por         | Telas principais |
| ------------ | ----------------------- | ---------------- |
| Integrante 1 | Home e Listagem         | 2                |
| Integrante 2 | Detalhe e Favoritos     | 2                |
| Integrante 3 | Cadastro e Edição       | 2                |
| Integrante 4 | Configurações e Offline | 2                |
| Integrante 5 | Login/Sobre e Delete    | 2                |

---

## 🧭 Diagrama de Navegação

(diagrama ilustrando o fluxo entre as telas — pode inserir aqui a imagem gerada)

---

## 🧪 Testes realizados

* CRUD de receitas
* Favoritar/desfavoritar
* Filtro e pesquisa
* Alternância de tema
* Modo offline
* Tratamento de erros de rede

---

## 📅 Cronograma de desenvolvimento

| Semana | Etapa                | Entregas principais                     |
| ------ | -------------------- | --------------------------------------- |
| 1      | Planejamento e setup | Estrutura do projeto, rotas, telas base |
| 2      | API e listagem       | Consumo de dados e exibição na Home     |
| 3      | CRUD completo        | Formulários com validação               |
| 4      | Favoritos e offline  | Persistência local e cache              |
| 5      | Finalização          | Testes, README, vídeo demo e slides     |

---

## 📹 Demonstração

🎬 
---

## 📦 Versão e licença

Versão 1.0 — uso educacional, sem fins comerciais.

---
