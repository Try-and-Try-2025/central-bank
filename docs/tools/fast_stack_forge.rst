===================================
Using fast-stack-forge CLI Guide
===================================

.. image:: https://img.shields.io/badge/License-MIT-yellow.svg
   :target: https://opensource.org/licenses/MIT
   :alt: License: MIT

`fast-stack-forge` is a powerful, Symfony-style CLI scaffolder designed to bootstrap and accelerate the development of FastAPI and Data Science applications. It provides a robust, production-ready directory structure, fully integrated with ``uv`` for lightning-fast package management, alongside essential utilities like rate limiting, scheduling, WebSocket management, dbt pipelines, and JWT middleware.

------------
1. Features
------------

*   **Rapid Scaffolding**: Generate full FastAPI projects or AstroData-style Data Science repositories in seconds.
*   **Entity Generation**: Automatically create database models, Pydantic schemas, business logic controllers, and router endpoints for your entities.
*   **Data Science Skeletons**: Bootstrap data science structures, environment setups, Pytest suites, and interactive Streamlit pages.
*   **AI Service Integration**: Instantly scaffold Retrieval-Augmented Generation (RAG), tool-calling Agents, stateful Agentic workflows (LangGraph), and OCR/Vision extraction services.
*   **dbt (Data Build Tool) Support**: Built-in commands to initialize and manage analytical engineering pipelines.
*   **Data Synchronization**: Scaffolds ELT/sync scripts to move data from NoSQL (e.g., MongoDB) to relational systems (e.g., PostgreSQL) with background scheduling.
*   **Production-Ready Utilities**: Pre-configured JWT middleware, rate limiting (slowapi), background tasks (apscheduler), and WebSockets.

----------------
2. Installation
----------------

Since `fast-stack-forge` leverages ``uv`` for lightning-fast environment and dependency setup, the recommended approach is to install it globally using the ``uv tool``:

.. code-block:: bash

   uv tool install fast-stack-forge

Alternatively, you can install it using standard ``pip``:

.. code-block:: bash

   pip install fast-stack-forge

---------------------------------------
3. Project Bootstrapping (FastAPI)
---------------------------------------

Initializing a FastAPI App
^^^^^^^^^^^^^^^^^^^^^^^^^^

To bootstrap a new FastAPI project, run the ``init`` command. You can specify your preferred database engine:

.. code-block:: bash

   fast-stack-forge init my_project --db sqlite

Supported Database Engines (via ``--db`` option):
  - ``sqlite`` (default)
  - ``postgresql``
  - ``mysql``
  - ``mongodb``

Running the Application
^^^^^^^^^^^^^^^^^^^^^^^

Navigate into your newly created project and install dependencies using ``uv``:

.. code-block:: bash

   cd my_project
   make install
   source .venv/bin/activate

Start the development server:

.. code-block:: bash

   make run

---------------------------------------
4. AstroData Data Science Scaffolding
---------------------------------------

FastForge integrates closely with **AstroData** to scaffold data science project structures.

Bootstrapping a Data Science Project (``init:ds-data``)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Scaffold a complete data science structure with interactive questions:

.. code-block:: bash

   fast-stack-forge init:ds-data my_ds_project --api --data

Options:
  *   ``--api``: Include a FastAPI REST API skeleton in `src/<package_name>/api/`.
  *   ``--data``: Include raw, interim, processed, and external ETL folders under `data/`.

**Interactive Prompts:**
When run, the command will prompt for:
1.  *Companion FastAPI project?* (Asks to create a separate `<name>_api` project alongside).
2.  *Python version* (Interactive selector list based on your system python).
3.  *Open-source license* (MIT, BSD-3-Clause, Apache-2.0, GPL-3.0, or Proprietary).
4.  *Project description, Author Name, and Author Email*.

Adding Backend Capabilities to Existing Projects (``init:ds-make``)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you already have a data science project, you can inject a FastAPI + Streamlit backend & dashboard into it:

.. code-block:: bash

   cd my_ds_project
   fast-stack-forge init:ds-make

This automatically detects the package structure inside `src/` and creates:
  - `src/<package>/api/` — FastAPI application structure and router.
  - `app/<package>_app.py` — Streamlit dashboard pre-wired to the API.
  - `.streamlit/config.toml` — Streamlit styles & port settings.
  - `docker-compose.yml` and `dockerfiles/` — Preconfigured containerization.

---------------------------------------
5. Generating & Discarding Entities
---------------------------------------

FastForge provides standard Symfony-style maker commands for database entities.

Generating a Database Entity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Generate the database model, schema (Pydantic), controller, and API routes automatically:

.. code-block:: bash

   fast-stack-forge make:entity Product name:string price:float category:string:fk=Category is_available:bool description:text:nullable

Field Syntax Definition:
  Fields are defined as ``name:type[:modifier]``.

  *   **Supported Types**: ``string``, ``int``, ``float``, ``bool``, ``text``, ``date``, ``datetime``
  *   **Supported Modifiers**:
      - ``nullable``: Allows the database column/field to be null.
      - ``encrypt``: Automatically encrypts the field before writing to the database.
      - ``hash``: Hashes the field value (useful for passwords).
      - ``fk=ModelName``: Establishes a foreign key relationship with another entity.

Options:
  - ``--no-router``: Skip generating API routes/router.
  - ``--no-controller``: Skip generating business logic controller.

Discarding/Removing an Entity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you want to remove all files generated for an entity (model, schema, controller, router), run:

.. code-block:: bash

   fast-stack-forge make:discard Product

---------------------------------------
6. Analytical Engineering (dbt ETL)
---------------------------------------

FastForge includes native scaffolding for analytical engineering via **dbt** (Data Build Tool).

Initializing a dbt ETL Project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To set up a dbt project within your ``src/`` folder:

.. code-block:: bash

   fast-stack-forge init:etl my_dbt_project --archi medallion --connector local

Options:
  *   **``--archi``** (Architecture style):
      - ``default`` (creates stg, int, mart layers)
      - ``medallion`` (creates bronze, silver, gold layers)
      - ``star`` (creates raw, dim, fact tables)
  *   **``--connector``** (Data Warehouse connector):
      - ``local`` (DuckDB)
      - ``snowflake``
      - ``bigquery``
      - ``postgres``

Creating a dbt Model
^^^^^^^^^^^^^^^^^^^^

To generate a new dbt model and its schema definition file:

.. code-block:: bash

   fast-stack-forge make:dbt customer_orders --layer silver

Options:
  - ``--view``: Materialize model as a SQL view.
  - ``--incremental``: Generate incremental update block.
  - ``--python``: Generate a Python model instead of standard SQL.
  - ``--layer <layer>``: Choose a specific layer to place the model in (e.g. bronze, staging, mart).

---------------------------------------
7. AI & Sync Services
---------------------------------------

Scaffolding AI Services
^^^^^^^^^^^^^^^^^^^^^^^

Instantly generate fully-coded AI services and their API routers:

.. code-block:: bash

   fast-stack-forge make:service rag_helper --type rag --provider openai --vector chroma

Options:
  *   **``--type``**:
      - ``rag``: Retrieval-Augmented Generation.
      - ``agent``: Tool-calling agent.
      - ``agentic``: Stateful LangGraph workflow.
      - ``ocr``: Document parsing/Vision service.
  *   **``--provider``**: ``openai``, ``anthropic``, ``mistral``, ``gemini``, or ``azure``
  *   **``--vector``**: ``chroma``, ``qdrant``, ``supabase``, or ``upstash`` (RAG type only)

Creating Data Synchronization Scripts (ELT)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Generate scripts to replicate/sync data between database systems (NoSQL to Relational) powered by APScheduler for automated run intervals:

.. code-block:: bash

   fast-stack-forge make:sync MongoToPg --source mongodb --dest postgres

Generating Interactive Dashboards
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Scaffold a multi-page Streamlit dashboard inside `app/dashboard/` and register a `make dashboard` target in the project Makefile:

.. code-block:: bash

   fast-stack-forge make:dashboard Atelier Analytics Chatbot

---------------------------------------
8. Generated Project Architecture
---------------------------------------

A standard project generated with ``fast-stack-forge init my_project`` produces the following structure:

.. code-block:: text

   my_project/
   ├── app/
   │   ├── main.py              ← FastAPI entrypoint
   │   └── dashboard/           ← Streamlit pages (created via make:dashboard)
   ├── src/
   │   └── my_project/
   │       ├── entity/          ← Database models
   │       ├── schema/          ← Pydantic schemas
   │       ├── controller/      ← Business logic
   │       ├── router/          ← API routes
   │       ├── service/         ← AI services (created via make:service)
   │       ├── data/            ← DB connection setup
   │       ├── middleware/      ← JWTBearer authorization
   │       └── utils/
   │           ├── limiter.py   ← slowapi rate limiting
   │           ├── scheduling.py← apscheduler background tasks
   │           ├── crud_router.py← Generic CRUD routing factory
   │           └── connection_manager.py ← WS connection manager
   ├── Makefile
   ├── pyproject.toml
   └── .env
