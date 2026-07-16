# code-agency-
-- Multi-Agent Resale System — Core Schema
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    shopify_product_id VARCHAR(64) UNIQUE,
    sku VARCHAR(64) UNIQUE NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    cost_price DECIMAL(10,2),
    current_price DECIMAL(10,2),
    min_price DECIMAL(10,2),
    max_price DECIMAL(10,2),
    inventory_qty INTEGER DEFAULT 0,
    reorder_threshold INTEGER DEFAULT 5,
    status VARCHAR(32) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE agent_runs (
    id SERIAL PRIMARY KEY,
    agent_name VARCHAR(32) NOT NULL,
    trigger_source VARCHAR(64),
    status VARCHAR(32) DEFAULT 'running',
    input_summary TEXT,
    output_summary TEXT,
    error_message TEXT,
    started_at TIMESTAMP DEFAULT NOW(),
    completed_at TIMESTAMP
);

CREATE TABLE price_history (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id),
    old_price DECIMAL(10,2),
    new_price DECIMAL(10,2),
    reason VARCHAR(255),
    changed_by VARCHAR(32) DEFAULT 'repricer_agent',
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE inventory_events (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id),
    event_type VARCHAR(32),
    qty_change INTEGER,
    note TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE customer_messages (
    id SERIAL PRIMARY KEY,
    platform VARCHAR(32),
    customer_identifier VARCHAR(255),
    inbound_message TEXT,
    ai_response TEXT,
    status VARCHAR(32) DEFAULT 'pending',
    escalation_reason TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE revenue_snapshots (
    id SERIAL PRIMARY KEY,
    snapshot_date DATE UNIQUE,
    total_revenue DECIMAL(12,2),
    total_orders INTEGER,
    avg_order_value DECIMAL(10,2),
    top_product_id INTEGER REFERENCES products(id)
);

CREATE INDEX idx_agent_runs_name_time ON agent_runs(agent_name, started_at);
CREATE INDEX idx_price_history_product ON price_history(product_id);
CREATE INDEX idx_messages_status ON customer_messages(status);
name: Recommend a Resource
description: Recommend a Claude Code resource for the list
title: "[Resource]: <name of your resource>"
labels: ["resource-submission", "validation-pending"]

body:
  - type: markdown
    attributes:
      value: |
        Thanks for recommending a resource! An automated check will validate the form
        and post results as a comment. Passing validation means your recommendation has
        been received — a maintainer reviews and approves at their discretion.

        Please make sure the resource is specific to Claude Code, of high quality, and
        not already on the list.

  - type: input
    id: display_name
    attributes:
      label: Display Name
      description: The name of the resource as it will appear in the list.
      placeholder: "e.g., Dev Browser"
    validations:
      required: true

  - type: dropdown
    id: category
    attributes:
      label: Category
      description: The primary category for your resource.
      options:
        - "Start Here"
        - "Documentation, Knowledge & Learning"
        - "Research & Scientific Inquiry"
        - "Providers, Runtime & Integration Infrastructure"
        - "Remote Control, Notifications & Voice I/O"
        - "Alternative Clients"
        - "Status Lines"
        - "Design & UI/UX"
        - "Writing & Prose Quality"
        - "Creative Media"
        - "Infrastructure & DevOps"
        - "Security"
        - "Multi-Agent Orchestration"
        - "Skills"
        - "Memory & Context Persistence"
        - "Usage & Cost Monitoring"
        - "Observability"
        - "Linting"
    validations:
      required: true

  - type: input
    id: link
    attributes:
      label: Link
      description: The main URL (must start with https://). Prefer the GitHub repo.
      placeholder: "https://github.com/username/repository"
    validations:
      required: true

  - type: input
    id: author_name
    attributes:
      label: Author Name
      description: The author's name, alias, or GitHub username.
      placeholder: "Jane Doe or janedoe"
    validations:
      required: true

  - type: input
    id: author_link
    attributes:
      label: Author Link
      description: Link to the author's GitHub profile or website (https://).
      placeholder: "https://github.com/janedoe"
    validations:
      required: true

  - type: textarea
    id: description
    attributes:
      label: Description
      description: "1–3 sentences. Descriptive, not promotional. Don't address the reader. (10–500 characters.)"
      placeholder: "Describe what the resource does and what makes it notable."
    validations:
      required: true

  - type: checkboxes
    id: checklist
    attributes:
      label: Checklist
      options:
        - label: "I checked that this resource isn't already on the list"
          required: true
        - label: "All links are working and publicly accessible"
          required: true
        - label: "This resource is specific to Claude Code"
          required: true