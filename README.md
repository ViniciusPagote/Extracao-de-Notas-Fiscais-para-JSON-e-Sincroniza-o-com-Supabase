# Extracao-de-Notas-Fiscais-para-JSON-e-Sincroniza-o-com-Supabase

Este projeto em Python é uma solução robusta para automatizar a extração de dados de notas fiscais (cupons fiscais em formato PDF) de diversos supermercados brasileiros. Ele identifica o padrão de cada loja, extrai as informações de itens, datas e valores, e as organiza em um formato JSON para, em seguida, sincronizá-las com um banco de dados Supabase.

<h3>⚙️ Tecnologias e Dependências</h3>

O projeto utiliza as seguintes bibliotecas Python para processamento de arquivos e manipulação de dados:

- **pdfplumber:** Para extração de texto de arquivos PDF.
- **pandas:** Para organização dos dados extraídos em um DataFrame, facilitando a visualização e manipulação.
- **supabase:** Para a conexão e operações de INSERT e DELETE no banco de dados.

As bibliotecas podem ser instaladas via `pip`:

```Bash
pip install pdfplumber supabase pandas
```

### ✨ Funcionalidades

O script foi projetado para ser modular e extensível, com as seguintes capacidades:

- **Extração de Dados:** Lê o texto de cupons fiscais em PDF.
- **Reconhecimento de Supermercados:** Identifica automaticamente o formato do cupom de diferentes supermercados (Assaí, Atacadão, Pão de Açúcar, Cometa, Guara, Pinheiro, São Luiz, etc.).
- **Normalização de Produtos:** Utiliza uma biblioteca de produtos (biblioteca_produtos) para normalizar descrições de itens e garantir a consistência dos dados.
- **Estrutura em JSON:** Converte os dados extraídos para o formato JSON, facilitando a integração com o Supabase.
- **Sincronização com Supabase:** Conecta-se a uma instância do Supabase e executa a sincronização dos dados, apagando registros antigos do mesmo cupom e inserindo os novos.

### 🏗️ Estrutura do Código

O script `Extração NF para Json e subir dados para Supase.py` é dividido em seções lógicas para clareza e manutenção:

  1. **BIBLIOTECAS:** Importação das dependências e instalação via pip.
  2. `biblioteca_produtos`: Um dicionário para mapeamento de descrições de produtos, garantindo que "FILE PEITO SEARA 1kg" e "Filé de Peito de Frango Seara 1kg" sejam tratados como o mesmo item.
  3. **CONFIGURAÇÃO DO SUPABASE:** Definição da URL e chave de API para a conexão com o banco de dados.
  4. **FUNÇÕES GERAIS:** Funções auxiliares para extração de texto de PDF (extrair_texto_do_pdf) e normalização de valores monetários (normalizar_valor).
  5. **FUNÇÕES DE ANÁLISE ESPECÍFICAS:** Um conjunto de funções (analisar_cupom_assai, analisar_cupom_atacadao, etc.) que usam regex (expressões regulares) para analisar os padrões específicos de cada supermercado.
  6. **FUNÇÃO PRINCIPAL DE ANÁLISE:** A função analisar_cupom atua como um "roteador", identificando o supermercado e chamando a função de análise correta.
  7. **FUNÇÃO DE TRANSFORMAÇÃO:** A função transformar_dados_para_df processa os dados extraídos, aplica a normalização de produtos e os prepara em um formato ideal para um DataFrame do pandas.
  8. **BLOCO PRINCIPAL DE EXECUÇÃO:** O bloco if __name__ == "__main__": é o ponto de entrada do script. Ele varre a pasta /content/ em busca de arquivos PDF, processa-os em um loop e, ao final, consolida e sincroniza os dados com o Supabase.

### 🚀 Como Executar o Projeto

Siga os passos abaixo para clonar o projeto e rodar o script no seu ambiente:

  1. **Clone o repositório:**
```
Bash
git clone https://github.com/SeuUsuario/NomeDoSeuRepositorio.git
cd NomeDoSeuRepositorio
```

  2. **Configure o Supabase:**
- Crie uma nova tabela no seu projeto Supabase, nomeando-a como supermercados_json.
- A tabela deve ter as seguintes colunas:
    `id` (auto-gerado)
    `arquivo` (texto)
    `local_da_compra` (texto)
    `dt_compra` (data)
    `codigo` (texto)
    `descricao` (texto)
    `descricao_tratada` (texto)
    `quantidade` (número)
    `preco_unitario` (número)
    `preco_total` (número)
    `data_insercao` (data)
- Copie sua URL e sua Anon Key do Supabase e substitua-as nas variáveis supabase_url e supabase_key no script.

  3. **Adicione os arquivos PDF:**
- Coloque os arquivos de notas fiscais em PDF na mesma pasta do script (/content/ no exemplo do script, que geralmente é a pasta raiz do projeto em ambientes como Google Colab).

  4. **Execute o script:*
```
Bash
python "Extração NF para Json e subir dados para Supase.py"
```

Ao final da execução, você verá uma mensagem de confirmação e os dados estarão populados na sua tabela do Supabase.

### 🤝 Como Contribuir

Sua contribuição é muito bem-vinda! Se você encontrar um novo formato de cupom fiscal ou quiser melhorar a precisão da extração, siga estes passos:
  1. Faça um fork deste repositório.
  2. Crie uma nova branch: git checkout -b feature/sua-melhoria.
  3. Implemente suas alterações.
  4. Envie suas mudanças: git push origin feature/sua-melhoria.
  5. Abra um Pull Request detalhado.

📜 Licença
Este projeto está sob a licença [MIT License](https://opensource.org/licenses/MIT).

