erDiagram
    dataset {
        text id PK "e.g., 'gmw-annual-extent'"
        text name "e.g., 'Global Mangrove Watch Annual Extent'"
    }

    geometries {
        text id PK "e.g., 'asgs-ste-2021'"
        text name "e.g., 'Australian States and Territories 2021'"
    }

    product {
        text id PK "e.g., 'gmw-aus-states'"
        text name "e.g., 'Mangrove Coverage by Australian State'"
    }

    dataset_run {
        text id PK
        text description "e.g., 'GMW 2020 processed to 1km Mollweide'"
        jsonb parameters "targetCrs, resolution, resamplingMethod"
    }

    geometries_run {
        text id PK
        text description "e.g., 'ASGS STE converted to GeoParquet'"
        jsonb parameters "nameProperty: 'STE_NAME21'"
    }

    product_run {
        text id PK
        text description "e.g., 'Mangrove stats 2020 for all states'"
        jsonb parameters "fillValue: 0"
    }

    geometry_output {
        text id PK
        text geometries_run_id FK
        text name "e.g., 'Tasmania', 'Queensland'"
        jsonb properties "e.g., {STE_CODE21: '6', AREASQKM21: 68401}"
        jsonb geometry "GeoJSON polygon"
    }

    product_output {
        text id PK
        text product_run_id FK
        text geometry_output_id FK
        text variable_id FK "e.g., 'mangrove_total_area'"
        numeric value "e.g., 12543.75"
    }

    variable_category {
        text id PK "e.g., 'ecology', 'coverage'"
        text name "e.g., 'Ecology', 'Coverage Metrics'"
    }

    variable {
        text id PK "e.g., 'mangrove_total_area'"
        text name "e.g., 'Mangrove Total Area'"
        text unit "e.g., 'm²', '%'"
    }

    dataset ||--o{ dataset_run : "has runs"
    dataset ||--o{ product : "has products"

    geometries ||--o{ geometries_run : "has runs"
    geometries ||--o{ product : "has products"

    product ||--o{ product_run : "has runs"

    dataset_run ||--o{ product_run : "used in"
    geometries_run ||--o{ product_run : "used in"

    geometries_run ||--o{ geometry_output : "produces"

    product_run ||--o{ product_output : "produces"
    geometry_output ||--o{ product_output : "referenced by"

    variable_category ||--o{ variable_category : "parent of"
    variable_category ||--o{ variable : "contains"

    variable ||--o{ product_output : "measured in"
