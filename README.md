# ⚙️ Microsserviço Serverless para Validação de CPF

Projeto desenvolvido em **C# (.NET)** utilizando **Azure Functions (v4)** para criar uma função **HTTP POST** que valida CPFs recebidos no corpo da requisição.

---

## 🎯 Objetivo

Expor um endpoint HTTP no modelo **serverless**, capaz de:

- Receber um CPF via **JSON** no corpo (`body`);
- Validar se o CPF é válido conforme os dígitos verificadores;
- Retornar uma mensagem indicando o resultado da validação.

---

## 🧩 Tecnologias Utilizadas

- **Azure Functions v4**
- **.NET 6+ (in-process model)**
- **Newtonsoft.Json**
- **Microsoft.AspNetCore.Mvc**
- **Microsoft.Azure.WebJobs.Extensions.Http**

---

## 🔌 Endpoint

- **Método**: `POST`
- **Rota**: `/api/fnvalidacpf`
- **Cabeçalho**:  
  `Content-Type: application/json`
- **Body (JSON)**:
  ```json
  { "cpf": "52998224725" }
  ```

### ✅ Respostas possíveis

| Status | Retorno |
|--------|----------|
| `200 OK` | `"CPF válido."` |
| `400 Bad Request` | `"Por favor, informe o CPF."` ou `"CPF inválido."` |

---

## ▶️ Execução Local

1. **Restaure dependências e inicie o ambiente:**
   ```bash
   dotnet restore
   func start
   ```

2. **Teste a função localmente:**
   ```bash
   curl -X POST http://localhost:7071/api/fnvalidacpf      -H "Content-Type: application/json"      -d "{ \"cpf\": \"52998224725\" }"
   ```

3. **Saída esperada:**
   ```
   CPF válido.
   ```

---

## 🧮 Lógica de Validação

1. Remove todos os caracteres não numéricos.  
2. Garante que o CPF tenha **11 dígitos**.  
3. Rejeita CPFs com todos os dígitos iguais (ex: `11111111111`).  
4. Calcula os **dois dígitos verificadores (DV1 e DV2)** com base nos pesos:
   - **DV1:** pesos `10..2`
   - **DV2:** pesos `11..2`
5. Compara os DVs calculados com os dois últimos dígitos do CPF original.

---

## 🧱 Estrutura do Projeto

```
📦 DIO-Microsservico-Serverless-para--validacao-de-CPF
├── fnvalidacpf.cs              # Função principal (Azure Function HTTP)
├── Serverless_para_Validação_de_CPF.csproj
├── host.json
└── local.settings.json         # Configurações locais (não publicado)
```

---

## ☁️ Publicação no Azure

1. Autentique-se:
   ```bash
   az login
   ```

2. Publique a função:
   ```bash
   func azure functionapp publish <NOME_DA_FUNCTION_APP>
   ```

3. Após o deploy, teste com:
   ```bash
   curl -X POST https://<NOME_DA_FUNCTION_APP>.azurewebsites.net/api/fnvalidacpf      -H "Content-Type: application/json"      -d "{ \"cpf\": \"52998224725\" }"
   ```

---

## 📜 Autor

**Guilherme Pereira Lima**  
💻 Desenvolvedor Full Stack  
🔗 [GitHub](https://github.com/guilhermepl) | [LinkedIn](https://www.linkedin.com/in/guilherme-lima1602/)
