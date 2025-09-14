# AUVET - Documentação

Este é o repositório da documentação do projeto AUVET, um sistema de gerenciamento veterinário desenvolvido como parte da disciplina de TPPE (Técnicas de Programação e Projeto de Software) da Universidade de Brasília.

## 📋 Sobre o Projeto

O AUVET é uma aplicação web desenvolvida para facilitar o gerenciamento de clínicas veterinárias, incluindo:

- Cadastro e gerenciamento de clientes e animais
- Sistema de agendamento de consultas
- Prontuário digital veterinário
- Controle financeiro
- Relatórios e analytics

## 🚀 Como Executar a Documentação

### Pré-requisitos

- Python 3.8 ou superior
- pip (gerenciador de pacotes Python)

### Instalação

1. Clone o repositório:
```bash
git clone <url-do-repositorio>
cd auvet-documentacao
```

2. Instale as dependências:
```bash
pip install -r requirements.txt
```

3. Execute o servidor de desenvolvimento:
```bash
mkdocs serve
```

4. Acesse a documentação em: http://127.0.0.1:8000

### Build para Produção

Para gerar os arquivos estáticos da documentação:

```bash
mkdocs build
```

Os arquivos serão gerados na pasta `site/`.

## 📁 Estrutura do Projeto

```
auvet-documentacao/
├── docs/                    # Páginas da documentação
│   ├── index.md            # Página inicial
│   ├── sobre.md            # Sobre o projeto
│   ├── elicitacao.md       # Elicitação de requisitos
│   ├── backlog.md          # Backlog do produto
│   ├── modelagem.md        # Modelagem UML e EER
│   ├── prototipo.md        # Protótipos de interface
│   ├── arquitetura.md      # Arquitetura do sistema
│   └── assets/             # Diagramas e imagens
├── mkdocs.yml              # Configuração do MkDocs
├── requirements.txt        # Dependências Python
└── README.md              # Este arquivo
```

## 📚 Conteúdo da Documentação

### 🏠 Início
Página principal com visão geral do projeto e navegação.

### 📖 Sobre o Projeto
Informações detalhadas sobre objetivos, público-alvo, tecnologias e metodologia.

### 👥 Elicitação de Requisitos
Análise de requisitos baseada em personas, incluindo:
- Dr. Carlos Veterinário
- Maria Recepcionista
- Ana Administradora
- João Tutor

### 📋 Backlog
Lista detalhada de user stories organizadas por épicos e sprints.

### 🏗️ Modelagem
Diagramas UML e modelo de entidade-relacionamento (EER) do banco de dados.

### 🎨 Protótipo
Protótipos de interface e design system do projeto.

### 🏛️ Arquitetura
Documentação completa da arquitetura do sistema, incluindo:
- Arquitetura frontend e backend
- Estratégias de segurança
- Deploy e monitoramento

## 🛠️ Tecnologias Utilizadas

- **MkDocs**: Gerador de documentação estática
- **Material for MkDocs**: Tema moderno e responsivo
- **Mermaid**: Diagramas renderizados em tempo real
- **Python**: Runtime para o MkDocs

## 📝 Como Contribuir

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -am 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

## 👥 Equipe

- **Desenvolvedor Frontend**: [Nome]
- **Desenvolvedor Backend**: [Nome]
- **Designer UX/UI**: [Nome]
- **Gerente de Projeto**: [Nome]

## 📞 Contato

Para dúvidas ou sugestões sobre a documentação, entre em contato através dos canais oficiais do projeto.

---

**Universidade de Brasília - TPPE 2025.2**
