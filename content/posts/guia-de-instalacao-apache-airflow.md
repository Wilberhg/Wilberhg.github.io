---
title: "Guia de Instalação - Apache Airflow"
date: 2022-09-14T21:50:07-03:00
draft: false
tags: [python, airflow, apache, scheduler, orchestrator]
---

1. Criar a variável de ambiente contendo o diretório em que será instalado o Airflow:
```bash
export AIRFLOW_HOME=~/airflow
```

2. Criar constante contendo a versão do Airflow desejada:
```bash
AIRFLOW_VERSION=2.3.4
```

3. Criar constante contendo a versão do Python:
```bash
PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
```
- Explicando o código:
    - Coleta a versão instalada no equipamento - **Ex.:** "Python 3.8.10";
        - ```bash
          python --version
          ```
    - Gera um array separando pelo espaço no texto "Python 3.8.10" e seleciona o segundo elemento - **Ex.:** "3.8.10"; 
        - ```bash
          cut -d " " -f 2
          ```
    - Gera um novo array separando pelo ponto (".") e seleciona o primeiro e segundo elemento - **Ex.:** "3.8".  
        - ```bash
          cut -d "." -f 1-2
          ```

4. Criar constante com a URL de download do Airflow de acordo com a versão do mesmo e do Python no equipamento.
```bash
CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
```

5. Instalar o apache airflow na versão especificada na constante.
```bash
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
```

6. Executar o comando para inicializar os serviços:
    1. Subir tudo de uma vez só **(ambiente de desenvolvimento)**:
        ```bash
        airflow standalone
        ```
    2. Subir módulo por módulo **(ambiente produtivo)**:
        1. Subir banco de dados:
            ```bash
            airflow db init
            ```

        2. Subir o servidor:
            ```bash
            airflow webserver --port 8080
            ```

        3. Subir o agendador:
            ```bash
            airflow scheduler
            ```

7. **(EXTRA)** Criar usuário via linha de comando:
```bash
airflow users create \
    --username admin \
    --firstname Peter \
    --lastname Parker \
    --role Admin \
    --email spiderman@superhero.org
```

# Observações

- Durante a instalação do Airflow, será gerado um arquivo “airflow.cfg”, cujo contém as configurações defaults do Airflow;

- A base de dados padrão do Airflow é o SQLite;

- SQLite somente permite execuções sequênciais, caso deseje paralelizá-las, será necessário utilizar outro banco de dados;

- O Airflow possui N variáveis de ambiente que podem ser alteradas. Para saber quais existem e onde alterá-las, a Apache fornece um [guia de referências](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html).

# Referência Bibliográfica

Tutorial traduzido da documentação oficial do [Apache Airflow](https://airflow.apache.org/docs/apache-airflow/stable/start/local.html)

