# 📖 Guia de Estudos: Diretrizes e Tendências em Cibersegurança Corporativa

## 1. Contexto e Objetivos
Este repositório consolidada um caderno temático focado em **Cibersegurança, Governança e Privacidade**, conectando conceitos teóricos fundamentais às principais tendências tecnológicas e ameaças projetadas corporativamente.

**Objetivos de Estudo:**
* **Compreensão de Frameworks:** Dominar a aplicação prática do NIST CSF, ISO/IEC 27001 e CIS Controls.
* **Análise de Vulnerabilidades:** Estudar o OWASP Top 10 com implementações práticas e análise de incidentes globais.
* **Privacidade e Conformidade:** Entender a implementação de *Privacy by Design* sob a ótica da LGPD.
* **Defesa Automatizada:** Explorar o uso de Inteligência Artificial para detecção de anomalias e segurança em redes.

---

## 2. Curadoria de Fontes (NotebookLM)

> 🚀 **Ambiente de Trabalho Ativo:** Todo o material de apoio, análise de documentos e cruzamento de dados deste guia estão centralizados e configurados para consulta no meu [NotebookLM - Guia de Cibersegurança Corporativa](https://notebooklm.google.com/notebook/7ec3e799-f937-4f90-9716-02974279a833).

As diretrizes técnicas apresentadas foram fundamentadas a partir das seguintes fontes oficiais:

### 2.1. NIST Cybersecurity Framework (CSF) 2.0 e SP 1800-35 (Zero Trust)
* **NIST SP 1800-35 (Implementação de Arquitetura Zero Trust):** O documento oficial de alto nível pode ser acessado em [https://doi.org/10.6028/NIST.SP.1800-35](https://doi.org/10.6028/NIST.SP.1800-35)
* **NIST CSF 2.0:** O framework de cibersegurança atualizado está disponível em [https://doi.org/10.6028/NIST.CSWP.29](https://doi.org/10.6028/NIST.CSWP.29)
* **NIST SP 800-207 (Conceitos de Zero Trust):** As diretrizes fundamentais da arquitetura estão em [https://doi.org/10.6028/NIST.SP.800-207](https://doi.org/10.6028/NIST.SP.800-207)
* **Portal Geral do NIST:** [https://www.nist.gov/](https://www.nist.gov/)

### 2.2 OWASP Top 10 Application Security Risks
* **Análise de Remediações:** Um guia detalhado sobre as vulnerabilidades do Top 10 e suas soluções pode ser encontrado em [https://xygeni.io/pt/blog/owasp-Top-10-and-their-remedies/](https://xygeni.io/pt/blog/owasp-Top-10-and-their-remedies/)
* **Vulnerabilidades A01 e A02:** Detalhamento prático focado na falha A01:2021 (Broken Access Control) e A02:2021 (Cryptographic Failures).

### 2.3 Cartilhas e Guias do CERT.br / NIC.br
* **Cartilha de Segurança para Internet:** O material de referência nacional para usuários e profissionais está em [http://cartilha.cert.br/](http://cartilha.cert.br/)
* **Guia sobre Ransomware:** Instruções específicas sobre como entender e responder a este tipo de ataque estão disponíveis em [https://cert.br/docs/ransomware/](https://cert.br/docs/ransomware/)
* **Portal Geral do CERT.br:** [https://cert.br/](https://cert.br/)

### 2.4 Guia de Boas Práticas da LGPD (SGD - Gov.br)
* **Guia de Boas Práticas LGPD:** O documento técnico oficial da Secretaria de Governo Digital (SGD) para a implementação da lei pode ser baixado em [https://www.gov.br/governodigital/pt-br/seguranca-e-protecao-de-dados/guias/guia_lgpd.pdf](https://www.gov.br/governodigital/pt-br/seguranca-e-protecao-de-dados/guias/guia_lgpd.pdf)
* **Repositório de Frameworks e Modelos da SGD:** A lista completa de guias operacionais, incluindo modelos de política de segurança e privacidade (Privacy by Design), está em [https://www.gov.br/governodigital/pt-br/privacidade-e-seguranca/framework-guias-e-modelos](https://www.gov.br/governodigital/pt-br/privacidade-e-seguranca/framework-guias-e-modelos)
* **Página Oficial da LGPD no Governo Federal:** [https://www.gov.br/esporte/pt-br/acesso-a-informacao/lgpd](https://www.gov.br/esporte/pt-br/acesso-a-informacao/lgpd)

---

## 3. Análise Prática de Vulnerabilidades (OWASP A01)

Para demonstrar a maturidade do desenvolvimento seguro, abaixo está documentado um cenário real de **Broken Access Control (A01)** em Ruby on Rails, demonstrando a falha clássica de IDOR (*Insecure Direct Object Reference*) e sua respectiva correção.

### ❌ Código Vulnerável
O controlador abaixo confia cegamente no ID enviado pelo usuário via parâmetro HTTP, permitindo que qualquer usuário autenticado altere o ID na URL para visualizar faturas de outras pessoas.

```ruby
# app/controllers/invoices_controller.rb
class InvoicesController < ApplicationController
  before_action :authenticate_user!

  def show
    # FALHA CRÍTICA: Busca a fatura direto pelo ID do parâmetro, sem checar se pertence ao usuário logado
    @invoice = Invoice.find(params[:id])
  end
end
