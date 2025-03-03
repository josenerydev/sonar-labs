# SonarQube com Docker Compose

Este repositório configura o SonarQube e o PostgreSQL usando Docker Compose.

## Configuração e Inicialização

1. Clone o repositório e navegue até a pasta:
   ```sh
   git clone <repo_url>
   cd sonar-labs
   ```

2. Crie os diretórios necessários para armazenar os dados no host:
   ```sh
   mkdir -p postgres_data
   mkdir -p sonarqube_extensions
   ```

3. Ajuste as permissões dos diretórios (se necessário):
   ```sh
   sudo chown -R 999:999 postgres_data
   ```

4. Inicie os containers:
   ```sh
   docker compose up -d
   ```

5. Acesse o SonarQube em: [http://localhost:9000](http://localhost:9000)

6. Primeiro acesso:
   - Usuário: **admin**
   - Senha: **admin**

## Configuração do Projeto no SonarQube

1. Crie um novo projeto no SonarQube.  
   ![Criar Projeto](image.png)
2. Configure como "Clean as You Code".  
   ![Clean as You Code](image-2.png)
3. Escolha "Analysis Method" como **Locally**.  
   ![Analysis Method](image-3.png)
4. Clique em **Generate**.  
   ![Generate](image-4.png)
5. Clique em **Continue**.  
   ![Continue](image-5.png)
6. Escolha a opção **.NET**.  
   ![Escolher .NET](image-6.png)
7. Execute os comandos sugeridos na tela.  
   ![Comandos](image-7.png)

## Comando de Análise do SonarScanner

Execute o seguinte comando para iniciar a análise:
```sh
dotnet sonarscanner begin /k:"DotNetOnion.Template" \  
  /d:sonar.host.url="http://localhost:9000" \  
  /d:sonar.token="sqp_5bbf7acd1e7ae6f1dcfde8219998ece112c1fde4" \  
  /d:sonar.scanner.scanAll=false
```

**Por que usar `/d:sonar.scanner.scanAll=false`?**  
Caso essa opção não seja utilizada, a análise será executada em múltiplas linguagens, incluindo arquivos que podem não ser relevantes, o que pode levar a problemas como:
- Estouro do limite de linhas de código (LOC) no SonarQube.
- Análise de arquivos indesejados.
- Maior tempo de execução da análise.

Para evitar esses problemas, é recomendável usar `/d:sonar.scanner.scanAll=false`, garantindo que apenas as mudanças relevantes sejam analisadas.

Se houver mais de uma solution no projeto, especifique:
```sh
dotnet build .\DotNetOnion.Template.sln
```

## Sobre o Armazenamento de Dados

Este Docker Compose está configurado para armazenar os dados do PostgreSQL e as extensões do SonarQube diretamente no host, em vez de usar volumes gerenciados pelo Docker. Isso facilita o backup, a migração e o acesso direto aos dados.

Os dados são armazenados nos seguintes diretórios:
- `./postgres_data`: Contém os dados do banco PostgreSQL
- `./sonarqube_extensions`: Contém as extensões e configurações do SonarQube

## Nota para Usuários Windows

Recomendamos o uso do WSL (Windows Subsystem for Linux) para executar o Docker e SonarQube, oferecendo melhor desempenho e compatibilidade.