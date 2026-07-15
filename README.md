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
