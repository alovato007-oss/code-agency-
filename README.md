# code-agency-
cod-agents/
├── README.md (deployment checklist, setup order)
├── db/
│   └── schema.sql (Postgres tables: products, agents, pricing, inventory, messages, revenue)
├── agents/
│   ├── lister_agent.md
│   ├── repricer_agent.md
│   ├── sourcing_agent.md
│   └── message_responder_agent.md
├── workflows/
│   ├── products_sync.md (n8n: Shopify → Postgres every 2 hours)
│   ├── orders_sync.md (n8n: Shopify → Postgres every 30 min)
│   └── messages_sync.md (n8n: Shopify Inbox → Postgres every 15 min)
├── dashboard/
│   └── dashboard.html (live monitoring UI)
└── vault/
    ├── agents/ (Obsidian notes)
    ├── workflows/ (n8n specs)
    ├── products/ (Cod Bites SKUs, pricing rules)
    ├── platforms/ (Shopify, eBay, Poshmark, Mercari, Depop)
    ├── torqueflow/ (brand guide, visual identity, SKUs)
    └── logs/ (daily agent runs, decisions)
