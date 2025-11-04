# EEWPW – Earthquake Early Warning Performance Viewer

<table>
  <tr>
    <td style="width: 300px; vertical-align: middle;">
      <img src="eewpw-logo-large-v1.png" alt="EEWPW Logo" width="400"/>
    </td>
    <td style="vertical-align: middle;">
      <strong>EEWPW</strong> is a modular, open-source framework for analyzing and visualizing the performance of Earthquake Early Warning (EEW) systems. 
      It integrates real-time detections, playback data, and algorithm outputs into a unified, interactive platform for both scientific and operational use.
    </td>
  </tr>
</table>

---

## Key Features

- **Backend API** – FastAPI-based service for data management, uploads, indexing, and streaming  
- **Dashboard** – Dash/Plotly web application for interactive performance visualization  
- **Deployment Stack** – Docker Compose environment bundling backend, frontend, and Redis cache  
- **Update Channel** – Automated image digest tracking for version control and upgrade notifications  
- **Planned Expansion** – AI-driven parser and monitoring agent for automated log analysis and performance scoring

---

## Architecture Overview

```mermaid
flowchart TB
    subgraph GitHub
      ACT[Action 15min] --> JSON[update.json]
    end

    subgraph Host Docker Compose with/without Redis
      FE[EEWPW Dashboard Dash]
      BE[FastAPI Backend]
      R[ Redis cache opt]
      subgraph DATA_ROOT bind-mounted
        D_FILES[/files/ uploads/]
        D_INDEX[/indexes/ derived/]
        D_LOGS[/logs/]
        D_AUX[/auxdata/]
        D_CFG[/config/eewpw-config.toml/]
      end
    end

    Browser/Internet --> FE
    FE -- polls update.json --> JSON
    FE -- HTTP (map, tables) --> BE
    FE -. reads cfg via env .-> D_CFG
    BE <-- cache lookups --> R
    BE -- write/read --> D_FILES
    BE -- write/read --> D_INDEX
    BE -- append --> D_LOGS
    BE -- read cfg --> D_CFG
```

## Repositories

| Repository | Description |
|-------------|-------------|
| [**eewpw**](https://github.com/eewpw/eewpw) | Docker Compose deployment with Makefile targets and smoke tests |
| [**eewpw-dashboard**](https://github.com/eewpw/eewpw-dashboard) | Interactive Dash/Plotly frontend for EEW performance visualization |
| [**eewpw-backend**](https://github.com/eewpw/eewpw-backend) | FastAPI backend providing data APIs, upload handling, and caching |
| [**eewpw-update-status**](https://github.com/eewpw/eewpw-update-status) | Provides image digests for update notifications |

----

